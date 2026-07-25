# Chương 14: Flutter Architecture
## GeriatricCare – Kiến trúc ứng dụng Flutter

---

## 14.1 Tổng quan kiến trúc

GeriatricCare áp dụng **Clean Architecture** kết hợp **Feature-first** folder structure.

```
┌──────────────────────────────────────────────────────────┐
│                  PRESENTATION LAYER                      │
│  Screens │ Widgets │ Notifiers (Riverpod) │ Navigation   │
├──────────────────────────────────────────────────────────┤
│                  APPLICATION LAYER                       │
│         Use Cases │ DTOs │ Mappers                       │
├──────────────────────────────────────────────────────────┤
│                   DOMAIN LAYER                           │
│     Entities │ Repository Interfaces │ Value Objects     │
├──────────────────────────────────────────────────────────┤
│                    DATA LAYER                            │
│  Repository Impl │ Remote DataSource │ Local DataSource  │
├──────────────────────────────────────────────────────────┤
│                INFRASTRUCTURE LAYER                      │
│  Firebase │ Hive │ FCM │ Geolocator │ TTS │ Accelerom.   │
└──────────────────────────────────────────────────────────┘
```

**Nguyên tắc cốt lõi:**
- Dependency chỉ đi vào trong (Domain không phụ thuộc Data hay Presentation)
- Mỗi feature là một module độc lập
- Repository pattern tách domain khỏi datasource
- Use Cases là đơn vị logic nghiệp vụ

---

## 14.2 Cấu trúc thư mục (Feature-first)

```
lib/
├── main.dart
├── app.dart                        # MaterialApp root, theme, router
│
├── core/
│   ├── constants/
│   │   ├── app_constants.dart      # App-wide constants
│   │   ├── route_constants.dart    # Named routes
│   │   └── firebase_constants.dart # Collection names
│   ├── errors/
│   │   ├── failures.dart           # Domain failures (sealed class)
│   │   └── exceptions.dart         # Data layer exceptions
│   ├── usecases/
│   │   └── usecase.dart            # Base UseCase<Type, Params>
│   ├── utils/
│   │   ├── either.dart             # Result<Failure, Success>
│   │   ├── validators.dart
│   │   ├── date_utils.dart
│   │   └── health_threshold_utils.dart
│   ├── theme/
│   │   ├── app_theme.dart
│   │   ├── color_scheme.dart
│   │   └── typography.dart
│   ├── navigation/
│   │   ├── app_router.dart         # GoRouter config
│   │   └── route_guards.dart       # Auth guards
│   └── di/
│       └── injection_container.dart # GetIt setup
│
├── features/
│   ├── auth/
│   │   ├── data/
│   │   │   ├── datasources/
│   │   │   │   └── firebase_auth_datasource.dart
│   │   │   ├── models/
│   │   │   │   └── user_model.dart
│   │   │   └── repositories/
│   │   │       └── auth_repository_impl.dart
│   │   ├── domain/
│   │   │   ├── entities/
│   │   │   │   └── user_entity.dart
│   │   │   ├── repositories/
│   │   │   │   └── auth_repository.dart
│   │   │   └── usecases/
│   │   │       ├── sign_in_usecase.dart
│   │   │       ├── sign_up_usecase.dart
│   │   │       └── sign_out_usecase.dart
│   │   └── presentation/
│   │       ├── screens/
│   │       │   ├── login_screen.dart
│   │       │   ├── register_screen.dart
│   │       │   └── forgot_password_screen.dart
│   │       ├── widgets/
│   │       │   └── auth_text_field.dart
│   │       └── notifiers/
│   │           └── auth_notifier.dart
│   │
│   ├── sos/
│   │   ├── data/
│   │   │   ├── datasources/
│   │   │   │   ├── firestore_sos_datasource.dart
│   │   │   │   ├── fcm_datasource.dart
│   │   │   │   └── shake_detector_datasource.dart
│   │   │   └── repositories/
│   │   │       └── sos_repository_impl.dart
│   │   ├── domain/
│   │   │   ├── entities/
│   │   │   │   └── sos_event_entity.dart
│   │   │   ├── repositories/
│   │   │   │   └── sos_repository.dart
│   │   │   └── usecases/
│   │   │       ├── trigger_sos_usecase.dart
│   │   │       ├── cancel_sos_usecase.dart
│   │   │       └── get_sos_history_usecase.dart
│   │   └── presentation/
│   │       ├── screens/
│   │       │   ├── sos_button_widget.dart
│   │       │   ├── sos_countdown_screen.dart
│   │       │   └── sos_history_screen.dart
│   │       └── notifiers/
│   │           └── sos_notifier.dart
│   │
│   ├── health/
│   │   ├── data/ ...
│   │   ├── domain/ ...
│   │   └── presentation/
│   │       ├── screens/
│   │       │   ├── health_input_screen.dart
│   │       │   ├── health_chart_screen.dart
│   │       │   └── health_history_screen.dart
│   │       └── notifiers/
│   │           └── health_notifier.dart
│   │
│   ├── medication/ ...
│   ├── dashboard/
│   │   ├── family/
│   │   │   ├── family_dashboard_screen.dart
│   │   │   └── family_dashboard_notifier.dart
│   │   ├── doctor/
│   │   │   ├── doctor_dashboard_screen.dart
│   │   │   └── doctor_dashboard_notifier.dart
│   │   └── elderly/
│   │       ├── elderly_home_screen.dart
│   │       └── elderly_home_notifier.dart
│   ├── location/ ...
│   ├── profile/ ...
│   ├── report/ ...
│   └── settings/ ...
│
└── shared/
    ├── widgets/
    │   ├── sos_fab_button.dart
    │   ├── health_metric_card.dart
    │   ├── health_line_chart.dart
    │   ├── medication_reminder_card.dart
    │   ├── loading_overlay.dart
    │   ├── error_view.dart
    │   └── empty_state_view.dart
    └── extensions/
        ├── datetime_extensions.dart
        └── string_extensions.dart
```

---

## 14.3 State Management – Riverpod

### 14.3.1 Provider hierarchy

```dart
// core/di/providers.dart

// --- Auth ---
final firebaseAuthProvider = Provider<FirebaseAuth>(
  (ref) => FirebaseAuth.instance,
);

final authRepositoryProvider = Provider<AuthRepository>((ref) {
  return AuthRepositoryImpl(
    remoteDs: ref.watch(firebaseAuthDatasourceProvider),
    firestoreDs: ref.watch(firestoreDatasourceProvider),
  );
});

final authNotifierProvider =
    StateNotifierProvider<AuthNotifier, AuthState>((ref) {
  return AuthNotifier(
    signInUseCase: ref.watch(signInUseCaseProvider),
    signUpUseCase: ref.watch(signUpUseCaseProvider),
    signOutUseCase: ref.watch(signOutUseCaseProvider),
  );
});

// --- SOS ---
final sosNotifierProvider =
    StateNotifierProvider<SosNotifier, SosState>((ref) {
  return SosNotifier(
    triggerSos: ref.watch(triggerSosUseCaseProvider),
    cancelSos: ref.watch(cancelSosUseCaseProvider),
  );
});

// --- Health ---
final healthNotifierProvider = StateNotifierProvider
    .family<HealthNotifier, HealthState, String>((ref, elderlyId) {
  return HealthNotifier(
    elderlyId: elderlyId,
    saveRecord: ref.watch(saveHealthRecordUseCaseProvider),
    getHistory: ref.watch(getHealthHistoryUseCaseProvider),
  );
});

// --- Family Dashboard ---
final familyDashboardProvider = StreamProvider
    .family<FamilyDashboardState, String>((ref, elderlyId) {
  return ref.watch(familyDashboardUseCaseProvider).watch(elderlyId);
});
```

### 14.3.2 State classes (Sealed / Freezed)

```dart
// features/auth/presentation/notifiers/auth_state.dart
@freezed
class AuthState with _$AuthState {
  const factory AuthState.initial() = _Initial;
  const factory AuthState.loading() = _Loading;
  const factory AuthState.authenticated(UserEntity user) = _Authenticated;
  const factory AuthState.unauthenticated() = _Unauthenticated;
  const factory AuthState.error(String message) = _Error;
}

// features/sos/presentation/notifiers/sos_state.dart
@freezed
class SosState with _$SosState {
  const factory SosState.idle() = _Idle;
  const factory SosState.countdown(int seconds) = _Countdown;
  const factory SosState.sending() = _Sending;
  const factory SosState.sent(SosEventEntity event) = _Sent;
  const factory SosState.cancelled() = _Cancelled;
  const factory SosState.error(String message) = _Error;
}

// features/health/presentation/notifiers/health_state.dart
@freezed
class HealthState with _$HealthState {
  const factory HealthState.initial() = _Initial;
  const factory HealthState.loading() = _Loading;
  const factory HealthState.loaded({
    required List<HealthRecordEntity> records,
    required Map<HealthType, HealthRecordEntity?> latest,
  }) = _Loaded;
  const factory HealthState.saving() = _Saving;
  const factory HealthState.saved({
    required HealthRecordEntity record,
    required bool alertSent,
  }) = _Saved;
  const factory HealthState.error(String message) = _Error;
}
```

---

## 14.4 Domain Layer – Use Cases

```dart
// core/usecases/usecase.dart
abstract class UseCase<Type, Params> {
  Future<Either<Failure, Type>> call(Params params);
}

class NoParams extends Equatable {
  @override
  List<Object?> get props => [];
}

// features/sos/domain/usecases/trigger_sos_usecase.dart
class TriggerSosUseCase implements UseCase<SosEventEntity, TriggerSosParams> {
  final SosRepository repository;
  TriggerSosUseCase(this.repository);

  @override
  Future<Either<Failure, SosEventEntity>> call(TriggerSosParams params) async {
    return repository.triggerSOS(params.elderlyId, params.position);
  }
}

@freezed
class TriggerSosParams with _$TriggerSosParams {
  const factory TriggerSosParams({
    required String elderlyId,
    Position? position,
    required SosTriggerType triggerType,
  }) = _TriggerSosParams;
}

// features/health/domain/usecases/save_health_record_usecase.dart
class SaveHealthRecordUseCase
    implements UseCase<HealthRecordEntity, SaveHealthRecordParams> {
  final HealthRepository repository;
  final HealthThresholdRepository thresholdRepository;
  SaveHealthRecordUseCase(this.repository, this.thresholdRepository);

  @override
  Future<Either<Failure, HealthRecordEntity>> call(
      SaveHealthRecordParams params) async {
    // 1. Get threshold for this elderly
    final thresholdResult = await thresholdRepository.getThreshold(
      params.elderlyId, params.type,
    );
    // 2. Check if abnormal
    final isAbnormal = thresholdResult.fold(
      (_) => false,
      (threshold) => threshold.isExceeded(params.values),
    );
    // 3. Save record
    final record = HealthRecordEntity(
      elderlyId: params.elderlyId,
      type: params.type,
      values: params.values,
      measuredAt: params.measuredAt,
      isAbnormal: isAbnormal,
      pendingSync: false,
    );
    return repository.saveRecord(record);
  }
}
```

---

## 14.5 Data Layer – Repository Implementation

```dart
// features/health/data/repositories/health_repository_impl.dart
class HealthRepositoryImpl implements HealthRepository {
  final FirestoreHealthDatasource remoteDs;
  final LocalHealthDatasource localDs;
  final NetworkInfo networkInfo;

  HealthRepositoryImpl({
    required this.remoteDs,
    required this.localDs,
    required this.networkInfo,
  });

  @override
  Future<Either<Failure, HealthRecordEntity>> saveRecord(
      HealthRecordEntity entity) async {
    try {
      final model = HealthRecordModel.fromEntity(entity);

      // Always save locally first (offline-first)
      await localDs.saveRecord(model.copyWith(pendingSync: true));

      if (await networkInfo.isConnected) {
        final docId = await remoteDs.addRecord(model);
        await localDs.markSynced(model.id, docId);
        return Right(entity.copyWith(pendingSync: false));
      }

      return Right(entity.copyWith(pendingSync: true));
    } on FirestoreException catch (e) {
      return Left(ServerFailure(e.message));
    } on CacheException catch (e) {
      return Left(CacheFailure(e.message));
    }
  }

  @override
  Stream<List<HealthRecordEntity>> getRecords(
    String elderlyId, HealthType type, DateRange range,
  ) {
    return remoteDs
        .getRecordsStream(elderlyId, type.name, range)
        .map((models) => models.map((m) => m.toEntity()).toList());
  }

  @override
  Future<Either<Failure, int>> syncPendingRecords() async {
    try {
      final pending = await localDs.getPendingRecords();
      int synced = 0;
      for (final record in pending) {
        await remoteDs.addRecord(record);
        await localDs.markSynced(record.id, record.id);
        synced++;
      }
      return Right(synced);
    } catch (e) {
      return Left(ServerFailure(e.toString()));
    }
  }
}
```

---

## 14.6 SOS Service – Chi tiết triển khai

```dart
// features/sos/data/datasources/shake_detector_datasource.dart
class ShakeDetectorDatasource {
  final _accelerometerEvents = StreamController<bool>.broadcast();
  StreamSubscription? _subscription;

  static const double _threshold = 20.0; // m/s²
  static const int _requiredShakes = 3;
  static const Duration _window = Duration(seconds: 2);

  final _shakeTimes = <DateTime>[];

  Stream<bool> get shakeStream => _accelerometerEvents.stream;

  void startListening() {
    _subscription = accelerometerEvents.listen((event) {
      final magnitude = sqrt(
        event.x * event.x + event.y * event.y + event.z * event.z,
      );
      if (magnitude > _threshold) {
        _shakeTimes.add(DateTime.now());
        _cleanOldShakes();
        if (_shakeTimes.length >= _requiredShakes) {
          _shakeTimes.clear();
          _accelerometerEvents.add(true);
        }
      }
    });
  }

  void _cleanOldShakes() {
    final cutoff = DateTime.now().subtract(_window);
    _shakeTimes.removeWhere((t) => t.isBefore(cutoff));
  }

  void dispose() {
    _subscription?.cancel();
    _accelerometerEvents.close();
  }
}
```

---

## 14.7 Navigation – GoRouter với Role Guards

```dart
// core/navigation/app_router.dart
final routerProvider = Provider<GoRouter>((ref) {
  final authState = ref.watch(authNotifierProvider);

  return GoRouter(
    initialLocation: '/splash',
    redirect: (context, state) {
      final isAuthenticated = authState is _Authenticated;
      final isOnAuth = state.matchedLocation.startsWith('/auth');

      if (!isAuthenticated && !isOnAuth) return '/auth/login';
      if (isAuthenticated && isOnAuth) {
        final role = (authState as _Authenticated).user.role;
        return _dashboardRouteByRole(role);
      }
      return null;
    },
    routes: [
      GoRoute(path: '/splash', builder: (_, __) => const SplashScreen()),
      GoRoute(
        path: '/auth',
        builder: (_, __) => const AuthScreen(),
        routes: [
          GoRoute(path: 'login', builder: (_, __) => const LoginScreen()),
          GoRoute(path: 'register', builder: (_, __) => const RegisterScreen()),
          GoRoute(path: 'forgot', builder: (_, __) => const ForgotPasswordScreen()),
        ],
      ),
      GoRoute(
        path: '/elderly',
        builder: (_, __) => const ElderlyHomeScreen(),
        routes: [
          GoRoute(path: 'health/input', builder: (_, s) =>
              HealthInputScreen(type: s.extra as HealthType)),
          GoRoute(path: 'health/chart', builder: (_, s) =>
              HealthChartScreen(elderlyId: s.extra as String)),
          GoRoute(path: 'medication', builder: (_, __) => const MedicationScreen()),
          GoRoute(path: 'profile', builder: (_, __) => const ProfileScreen()),
        ],
      ),
      GoRoute(
        path: '/family',
        builder: (_, __) => const FamilyDashboardScreen(),
        routes: [
          GoRoute(path: 'detail/:elderlyId', builder: (_, s) =>
              ElderlyDetailScreen(elderlyId: s.pathParameters['elderlyId']!)),
          GoRoute(path: 'map/:elderlyId', builder: (_, s) =>
              MapScreen(elderlyId: s.pathParameters['elderlyId']!)),
          GoRoute(path: 'sos-history/:elderlyId', builder: (_, s) =>
              SosHistoryScreen(elderlyId: s.pathParameters['elderlyId']!)),
        ],
      ),
      GoRoute(path: '/doctor', builder: (_, __) => const DoctorDashboardScreen()),
      GoRoute(path: '/admin', builder: (_, __) => const AdminScreen()),
    ],
  );
});

String _dashboardRouteByRole(UserRole role) => switch (role) {
  UserRole.elderly => '/elderly',
  UserRole.family => '/family',
  UserRole.doctor => '/doctor',
  UserRole.admin => '/admin',
};
```

---

## 14.8 Dependency Injection – GetIt

```dart
// core/di/injection_container.dart
final sl = GetIt.instance;

Future<void> initDependencies() async {
  // === EXTERNAL ===
  sl.registerLazySingleton(() => FirebaseFirestore.instance);
  sl.registerLazySingleton(() => FirebaseAuth.instance);
  sl.registerLazySingleton(() => FirebaseMessaging.instance);
  sl.registerLazySingleton<NetworkInfo>(
    () => NetworkInfoImpl(connectivity: Connectivity()),
  );

  // === DATASOURCES ===
  sl.registerLazySingleton<FirestoreHealthDatasource>(
    () => FirestoreHealthDatasourceImpl(firestore: sl()),
  );
  sl.registerLazySingleton<LocalHealthDatasource>(
    () => LocalHealthDatasourceImpl(hiveBox: Hive.box('health_records')),
  );
  sl.registerLazySingleton<FcmDatasource>(
    () => FcmDatasourceImpl(messaging: sl()),
  );
  sl.registerLazySingleton<GeolocatorDatasource>(
    () => GeolocatorDatasourceImpl(),
  );
  sl.registerLazySingleton<ShakeDetectorDatasource>(
    () => ShakeDetectorDatasource(),
  );
  sl.registerLazySingleton<TtsDatasource>(
    () => TtsDatasourceImpl(),
  );

  // === REPOSITORIES ===
  sl.registerLazySingleton<AuthRepository>(
    () => AuthRepositoryImpl(remoteDs: sl(), networkInfo: sl()),
  );
  sl.registerLazySingleton<HealthRepository>(
    () => HealthRepositoryImpl(remoteDs: sl(), localDs: sl(), networkInfo: sl()),
  );
  sl.registerLazySingleton<SosRepository>(
    () => SosRepositoryImpl(remoteDs: sl(), fcmDs: sl(), geoDs: sl()),
  );
  sl.registerLazySingleton<MedicationRepository>(
    () => MedicationRepositoryImpl(remoteDs: sl(), notifDs: sl(), ttsDs: sl()),
  );

  // === USE CASES ===
  sl.registerLazySingleton(() => SignInUseCase(sl()));
  sl.registerLazySingleton(() => SignUpUseCase(sl()));
  sl.registerLazySingleton(() => TriggerSosUseCase(sl()));
  sl.registerLazySingleton(() => SaveHealthRecordUseCase(sl(), sl()));
  sl.registerLazySingleton(() => GetHealthHistoryUseCase(sl()));
  sl.registerLazySingleton(() => CreateMedScheduleUseCase(sl()));
  sl.registerLazySingleton(() => LogMedicationUseCase(sl()));
}
```

---

## 14.9 pubspec.yaml – Tất cả dependencies

```yaml
name: geriatric_care
description: GeriatricCare - Hệ thống Giám sát Người cao tuổi
publish_to: none
version: 1.0.0+1

environment:
  sdk: ">=3.0.0 <4.0.0"

dependencies:
  flutter:
    sdk: flutter

  # Firebase
  firebase_core: ^2.24.2
  firebase_auth: ^4.16.0
  cloud_firestore: ^4.14.0
  firebase_storage: ^11.6.0
  firebase_messaging: ^14.7.10
  firebase_analytics: ^10.8.0
  firebase_crashlytics: ^3.4.9
  firebase_performance: ^0.9.3+8
  firebase_remote_config: ^4.3.8

  # State Management
  flutter_riverpod: ^2.4.9
  riverpod_annotation: ^2.3.3

  # Navigation
  go_router: ^13.0.1

  # DI
  get_it: ^7.6.7
  injectable: ^2.3.2

  # Data / Models
  freezed_annotation: ^2.4.1
  json_annotation: ^4.8.1
  equatable: ^2.0.5
  dartz: ^0.10.1        # Either<Failure, Success>

  # Local Storage
  hive: ^2.2.3
  hive_flutter: ^1.1.0

  # Maps & Location
  google_maps_flutter: ^2.5.3
  geolocator: ^10.1.0
  geocoding: ^2.1.1

  # Sensors
  sensors_plus: ^4.0.2

  # Notifications
  flutter_local_notifications: ^16.3.0

  # TTS
  flutter_tts: ^3.8.5

  # Charts
  fl_chart: ^0.66.1

  # HTTP / Network
  http: ^1.2.0
  connectivity_plus: ^5.0.2

  # Utilities
  intl: ^0.19.0
  uuid: ^4.3.3
  path_provider: ^2.1.2
  permission_handler: ^11.2.0
  url_launcher: ^6.2.4
  share_plus: ^7.2.2
  image_picker: ^1.0.7
  cached_network_image: ^3.3.1

  # PDF
  pdf: ^3.10.8
  printing: ^5.12.0

  # Excel
  excel: ^4.0.2

  # Security
  local_auth: ^2.1.8
  flutter_secure_storage: ^9.0.0

  # Logging
  logger: ^2.1.0

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^3.0.1
  build_runner: ^2.4.7
  freezed: ^2.4.6
  json_serializable: ^6.7.1
  riverpod_generator: ^2.3.9
  injectable_generator: ^2.4.1
  hive_generator: ^2.0.1
  mockito: ^5.4.4
  fake_cloud_firestore: ^2.4.2
  firebase_auth_mocks: ^0.13.0
```

---

## 14.10 Background Services

### WorkManager – Sync offline data

```dart
// Đăng ký task sync khi có mạng
void registerSyncTask() {
  Workmanager().registerPeriodicTask(
    'sync-health-data',
    'syncHealthData',
    frequency: const Duration(hours: 1),
    constraints: Constraints(networkType: NetworkType.connected),
  );
}

// Callback thực thi
@pragma('vm:entry-point')
void callbackDispatcher() {
  Workmanager().executeTask((taskName, data) async {
    if (taskName == 'syncHealthData') {
      await initDependencies();
      final syncUseCase = sl<SyncPendingRecordsUseCase>();
      await syncUseCase(NoParams());
    }
    return Future.value(true);
  });
}
```

### Foreground Service – GPS & Shake Detection

```dart
// android/app/src/main/AndroidManifest.xml cần:
// <uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>
// <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION"/>

// Chạy foreground service cho GPS + Shake khi app background
class BackgroundMonitoringService {
  static Future<void> start() async {
    await FlutterForegroundTask.startService(
      notificationTitle: 'GeriatricCare đang theo dõi',
      notificationText: 'Bảo vệ bạn 24/7',
      callback: _serviceCallback,
    );
  }
}
```

---

## 14.11 Error Handling Strategy

```dart
// Failures (Domain Layer)
abstract class Failure extends Equatable {}
class ServerFailure extends Failure {
  final String message;
  const ServerFailure(this.message);
  @override List<Object?> get props => [message];
}
class CacheFailure extends Failure { ... }
class NetworkFailure extends Failure { ... }
class AuthFailure extends Failure { ... }
class PermissionFailure extends Failure { ... }

// Trong Presentation Layer
Widget build(BuildContext context, WidgetRef ref) {
  final state = ref.watch(healthNotifierProvider(elderlyId));
  return state.when(
    initial: () => const SizedBox.shrink(),
    loading: () => const LoadingOverlay(),
    loaded: (records, latest) => HealthContent(records: records),
    saving: () => const SavingOverlay(),
    saved: (record, alertSent) => SavedConfirmation(alertSent: alertSent),
    error: (msg) => ErrorView(
      message: msg,
      onRetry: () => ref.refresh(healthNotifierProvider(elderlyId)),
    ),
  );
}
```
