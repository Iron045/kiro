# GIAI ĐOẠN 3: THIẾT KẾ HỆ THỐNG
## GeriatricCare – Hệ thống Giám sát & Chăm sóc Người cao tuổi Độc cư

**Phiên bản:** 1.0.0 | **Ngày:** 2026-07-25 | **Trạng thái:** Hoàn chỉnh

---

## 1. MỤC TIÊU GIAI ĐOẠN

Giai đoạn này xác định:
- Kiến trúc tổng thể hệ thống (System Architecture)
- Kiến trúc ứng dụng Flutter (Clean Architecture)
- Cấu trúc thư mục dự án (Feature-first)
- Thiết kế các lớp: Presentation, Domain, Data
- Luồng dữ liệu và tương tác giữa các thành phần
- Sơ đồ UML: Component, Deployment, Sequence, Class
- Technology stack & lý do lựa chọn

---

## 2. KIẾN TRÚC TỔNG THỂ (SYSTEM ARCHITECTURE)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        GERIATRICCARE SYSTEM                             │
│                                                                         │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │                    MOBILE APP (Flutter)                          │  │
│  │                                                                  │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐  │  │
│  │  │  NCT Device │  │Family Device│  │     Doctor Device       │  │  │
│  │  │  (Android)  │  │  (Android)  │  │      (Android)          │  │  │
│  │  └──────┬──────┘  └──────┬──────┘  └──────────┬──────────────┘  │  │
│  └─────────┼───────────────┼───────────────────────┼───────────────┘  │
│            │               │                       │                   │
│            └───────────────┴───────────────────────┘                   │
│                                    │                                    │
│                         HTTPS / Firebase SDK                            │
│                                    │                                    │
│  ┌─────────────────────────────────▼───────────────────────────────┐   │
│  │                      FIREBASE BACKEND                           │   │
│  │                                                                  │   │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌─────────────────┐   │   │
│  │  │  Auth    │ │Firestore │ │  FCM     │ │    Storage      │   │   │
│  │  │(Firebase)│ │(Database)│ │(Push)    │ │  (Media Files)  │   │   │
│  │  └──────────┘ └──────────┘ └──────────┘ └─────────────────┘   │   │
│  │                                                                  │   │
│  │  ┌─────────────────────────────────────────────────────────┐   │   │
│  │  │               Cloud Functions (Node.js)                  │   │   │
│  │  │  • SOS Alert Dispatcher    • Health Threshold Checker   │   │   │
│  │  │  • Medication Reminder     • Report Generator           │   │   │
│  │  │  • Audit Logger            • Notification Orchestrator  │   │   │
│  │  └─────────────────────────────────────────────────────────┘   │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                                                         │
│  ┌───────────────────────┐    ┌──────────────────────────────────────┐  │
│  │  External Services    │    │         Monitoring                   │  │
│  │  • Google Maps API    │    │  • Firebase Crashlytics              │  │
│  │  • SMS Gateway        │    │  • Firebase Performance              │  │
│  │  • Google Places API  │    │  • Firebase Analytics                │  │
│  └───────────────────────┘    └──────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 3. KIẾN TRÚC FLUTTER – CLEAN ARCHITECTURE

### 3.1 Sơ đồ phân tầng

```
┌──────────────────────────────────────────────────────────────┐
│                   PRESENTATION LAYER                         │
│                                                              │
│  ┌────────────────┐  ┌──────────────┐  ┌─────────────────┐  │
│  │    Screens     │  │   Widgets    │  │  Riverpod       │  │
│  │  (UI pages)    │  │ (Components) │  │  Notifiers      │  │
│  └────────────────┘  └──────────────┘  └─────────────────┘  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  GoRouter (Navigation) │ AppTheme │ Localization     │   │
│  └──────────────────────────────────────────────────────┘   │
├──────────────────────────────────────────────────────────────┤
│                   APPLICATION LAYER                          │
│                                                              │
│  ┌────────────────┐  ┌──────────────┐  ┌─────────────────┐  │
│  │   Use Cases    │  │    DTOs      │  │    Mappers      │  │
│  │ (Business ops) │  │(Data models) │  │  (transforms)   │  │
│  └────────────────┘  └──────────────┘  └─────────────────┘  │
├──────────────────────────────────────────────────────────────┤
│                     DOMAIN LAYER                             │
│                                                              │
│  ┌────────────────┐  ┌──────────────┐  ┌─────────────────┐  │
│  │    Entities    │  │  Repository  │  │  Value Objects  │  │
│  │ (Core models)  │  │ Interfaces   │  │  Failures       │  │
│  └────────────────┘  └──────────────┘  └─────────────────┘  │
├──────────────────────────────────────────────────────────────┤
│                      DATA LAYER                              │
│                                                              │
│  ┌────────────────┐  ┌──────────────┐  ┌─────────────────┐  │
│  │  Repository    │  │   Remote     │  │     Local       │  │
│  │  Impl.         │  │  DataSource  │  │   DataSource    │  │
│  │                │  │ (Firestore)  │  │    (Hive)       │  │
│  └────────────────┘  └──────────────┘  └─────────────────┘  │
├──────────────────────────────────────────────────────────────┤
│                  INFRASTRUCTURE LAYER                        │
│                                                              │
│  Firebase │ Hive │ FCM │ Geolocator │ TTS │ sensors_plus   │
│  Google Maps │ flutter_tts │ fl_chart │ pdf │ printing      │
└──────────────────────────────────────────────────────────────┘
         Dependency rule: chỉ hướng vào trong (→ Domain)
```

### 3.2 Cấu trúc thư mục (Feature-first)

```
lib/
├── main.dart                          # Entry point, DI setup
├── app.dart                           # MaterialApp root, theme, router
│
├── core/
│   ├── constants/
│   │   ├── app_constants.dart         # Timeouts, limits, defaults
│   │   ├── route_constants.dart       # Named route paths
│   │   └── firebase_constants.dart    # Collection/field names
│   ├── errors/
│   │   ├── failures.dart              # Sealed class: Failures
│   │   └── exceptions.dart            # Data layer exceptions
│   ├── theme/
│   │   ├── app_theme.dart             # Light + Dark theme
│   │   ├── color_scheme.dart          # Color tokens
│   │   └── text_theme.dart            # Typography
│   ├── utils/
│   │   ├── date_utils.dart
│   │   ├── health_utils.dart          # Threshold checkers
│   │   └── validators.dart            # Form validators
│   ├── widgets/
│   │   ├── sos_button.dart            # Shared SOS button widget
│   │   ├── health_chart.dart          # Shared fl_chart wrapper
│   │   └── loading_overlay.dart
│   └── services/
│       ├── notification_service.dart  # FCM + local notifications
│       ├── tts_service.dart           # flutter_tts wrapper
│       ├── location_service.dart      # Geolocator wrapper
│       └── shake_service.dart         # sensors_plus wrapper
│
├── features/
│   ├── auth/
│   │   ├── data/
│   │   │   ├── datasources/
│   │   │   │   └── auth_remote_datasource.dart
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
│   │   │       ├── verify_otp_usecase.dart
│   │   │       └── sign_out_usecase.dart
│   │   └── presentation/
│   │       ├── notifiers/
│   │       │   └── auth_notifier.dart
│   │       └── screens/
│   │           ├── login_screen.dart
│   │           ├── register_screen.dart
│   │           └── role_selection_screen.dart
│   │
│   ├── sos/
│   │   ├── data/ ...
│   │   ├── domain/
│   │   │   ├── entities/sos_event_entity.dart
│   │   │   └── usecases/
│   │   │       ├── trigger_sos_usecase.dart
│   │   │       └── get_sos_history_usecase.dart
│   │   └── presentation/
│   │       ├── notifiers/sos_notifier.dart
│   │       └── screens/sos_countdown_screen.dart
│   │
│   ├── medication/
│   │   ├── data/ ...
│   │   ├── domain/
│   │   │   ├── entities/
│   │   │   │   ├── medication_schedule_entity.dart
│   │   │   │   └── medication_log_entity.dart
│   │   │   └── usecases/
│   │   │       ├── create_schedule_usecase.dart
│   │   │       ├── confirm_medication_usecase.dart
│   │   │       └── get_medication_history_usecase.dart
│   │   └── presentation/ ...
│   │
│   ├── health/
│   │   ├── data/ ...
│   │   ├── domain/
│   │   │   ├── entities/health_record_entity.dart
│   │   │   └── usecases/
│   │   │       ├── add_health_record_usecase.dart
│   │   │       └── get_health_history_usecase.dart
│   │   └── presentation/ ...
│   │
│   ├── dashboard/
│   │   ├── family/
│   │   │   └── presentation/screens/family_dashboard_screen.dart
│   │   └── doctor/
│   │       └── presentation/screens/doctor_dashboard_screen.dart
│   │
│   ├── location/
│   │   └── ...
│   │
│   └── profile/
│       └── ...
│
└── injection_container.dart           # GetIt DI registration
```

---

## 4. THIẾT KẾ DOMAIN LAYER

### 4.1 Core Entities

```dart
// features/auth/domain/entities/user_entity.dart
class UserEntity extends Equatable {
  final String uid;
  final String phoneNumber;
  final String displayName;
  final UserRole role;            // elderly | family | doctor | admin
  final String? photoUrl;
  final DateTime createdAt;
  final bool isActive;
}

// features/sos/domain/entities/sos_event_entity.dart
class SosEventEntity extends Equatable {
  final String id;
  final String elderlyId;
  final DateTime timestamp;
  final GeoPoint? location;
  final String triggerType;       // manual | shake
  final SosStatus status;         // pending | acknowledged | resolved
  final List<String> notifiedUsers;
}

// features/medication/domain/entities/medication_schedule_entity.dart
class MedicationScheduleEntity extends Equatable {
  final String id;
  final String elderlyId;
  final String medicineName;
  final String dosage;
  final String unit;              // viên, ml, mg
  final List<TimeOfDay> times;    // giờ uống trong ngày
  final DateTime startDate;
  final DateTime? endDate;
  final String? notes;
  final bool isActive;
}

// features/health/domain/entities/health_record_entity.dart
class HealthRecordEntity extends Equatable {
  final String id;
  final String elderlyId;
  final HealthType type;          // bloodPressure | glucose | heartRate | ...
  final double value;
  final double? valueSystolic;    // cho huyết áp
  final String unit;
  final DateTime measuredAt;
  final HealthStatus status;      // normal | warning | critical
  final String? notes;
  final String recordedBy;        // uid người nhập
}
```

### 4.2 Failures & Result

```dart
// core/errors/failures.dart
sealed class Failure {
  final String message;
  const Failure(this.message);
}

class NetworkFailure extends Failure {
  const NetworkFailure([super.message = 'Không có kết nối mạng']);
}
class AuthFailure extends Failure {
  const AuthFailure(super.message);
}
class FirestoreFailure extends Failure {
  const FirestoreFailure(super.message);
}
class LocationFailure extends Failure {
  const LocationFailure([super.message = 'Không lấy được GPS']);
}
class PermissionFailure extends Failure {
  const PermissionFailure(super.message);
}

// Dùng Either từ fpdart
typedef Result<T> = Either<Failure, T>;
```

---

## 5. THIẾT KẾ USE CASES

### 5.1 TriggerSosUseCase

```dart
class TriggerSosUseCase {
  final SosRepository _sosRepository;
  final LocationRepository _locationRepository;
  final NotificationService _notificationService;

  Future<Result<SosEventEntity>> call(TriggerSosParams params) async {
    // 1. Lấy GPS (timeout 10s)
    final locationResult = await _locationRepository.getCurrentLocation();
    final location = locationResult.fold((_) => null, (l) => l);

    // 2. Tạo SOS event
    final sosEvent = SosEventEntity(
      elderlyId: params.elderlyId,
      timestamp: DateTime.now(),
      location: location,
      triggerType: params.triggerType,
      status: SosStatus.pending,
    );

    // 3. Lưu vào Firestore
    final saveResult = await _sosRepository.saveSosEvent(sosEvent);

    // 4. Gửi FCM (Cloud Function xử lý)
    await _notificationService.triggerSosAlert(sosEvent);

    return saveResult;
  }
}
```

### 5.2 ConfirmMedicationUseCase

```dart
class ConfirmMedicationUseCase {
  Future<Result<void>> call(ConfirmMedicationParams params) async {
    // 1. Lưu log xác nhận
    // 2. Hủy scheduled notification
    // 3. Update dashboard người thân qua Firestore stream
    // 4. Nếu status = skipped → không cảnh báo ngay
    //    Nếu không phản hồi sau 30 phút → Cloud Function cảnh báo
  }
}
```

---

## 6. STATE MANAGEMENT – RIVERPOD

### 6.1 Provider hierarchy

```
┌────────────────────────────────────────────────────────────┐
│                   PROVIDER DEPENDENCY TREE                 │
│                                                            │
│  firebaseAuthProvider (StreamProvider)                     │
│       └── authNotifierProvider (NotifierProvider)          │
│                └── currentUserProvider (Provider)          │
│                         └── userRoleProvider (Provider)    │
│                                                            │
│  elderlyListProvider (StreamProvider)                      │
│       └── elderlyDetailProvider (family: AsyncNotifier)    │
│                └── healthRecordsProvider (StreamProvider)  │
│                └── medicationSchedulesProvider (Stream)    │
│                └── sosHistoryProvider (StreamProvider)     │
│                                                            │
│  locationProvider (StateNotifierProvider)                  │
│  shakeDetectionProvider (StateNotifierProvider)            │
│  sosCountdownProvider (StateNotifierProvider)              │
└────────────────────────────────────────────────────────────┘
```

### 6.2 SOS Notifier mẫu

```dart
@riverpod
class SosNotifier extends _$SosNotifier {
  @override
  SosState build() => const SosState.idle();

  Future<void> startSos(String triggerType) async {
    state = const SosState.countdown(secondsLeft: 5);
    // Countdown loop
    for (int i = 5; i > 0; i--) {
      if (state is SosCancelled) return;
      state = SosState.countdown(secondsLeft: i);
      await Future.delayed(const Duration(seconds: 1));
    }
    state = const SosState.sending();
    final result = await ref.read(triggerSosUseCaseProvider).call(
      TriggerSosParams(triggerType: triggerType),
    );
    result.fold(
      (failure) => state = SosState.failed(failure.message),
      (event)   => state = SosState.sent(event),
    );
  }

  void cancel() => state = const SosState.cancelled();
}
```

---

## 7. NAVIGATION – GoRouter

```dart
final appRouter = GoRouter(
  initialLocation: '/splash',
  redirect: (context, state) {
    final isAuthenticated = ref.read(isAuthenticatedProvider);
    final role = ref.read(userRoleProvider);
    if (!isAuthenticated) return '/login';
    if (state.location == '/login') {
      return switch (role) {
        UserRole.elderly => '/home',
        UserRole.family  => '/family-dashboard',
        UserRole.doctor  => '/doctor-dashboard',
        UserRole.admin   => '/admin',
        null             => '/role-selection',
      };
    }
    return null;
  },
  routes: [
    GoRoute(path: '/splash',            builder: (_, __) => const SplashScreen()),
    GoRoute(path: '/login',             builder: (_, __) => const LoginScreen()),
    GoRoute(path: '/register',          builder: (_, __) => const RegisterScreen()),
    GoRoute(path: '/role-selection',    builder: (_, __) => const RoleSelectionScreen()),
    GoRoute(path: '/home',              builder: (_, __) => const ElderlyHomeScreen()),
    GoRoute(path: '/sos-countdown',     builder: (_, __) => const SosCountdownScreen()),
    GoRoute(path: '/family-dashboard',  builder: (_, __) => const FamilyDashboardScreen()),
    GoRoute(path: '/doctor-dashboard',  builder: (_, __) => const DoctorDashboardScreen()),
    GoRoute(path: '/health/input',      builder: (_, __) => const HealthInputScreen()),
    GoRoute(path: '/health/history',    builder: (_, __) => const HealthHistoryScreen()),
    GoRoute(path: '/medication',        builder: (_, __) => const MedicationScreen()),
    GoRoute(path: '/medication/create', builder: (_, __) => const MedicationFormScreen()),
    GoRoute(path: '/profile/:id',       builder: (_, s)  => ElderlyProfileScreen(id: s.pathParameters['id']!)),
    GoRoute(path: '/map',               builder: (_, __) => const MapScreen()),
    GoRoute(path: '/settings',          builder: (_, __) => const SettingsScreen()),
    GoRoute(path: '/reports',           builder: (_, __) => const ReportsScreen()),
  ],
);
```

---

## 8. DEPENDENCY INJECTION – GetIt

```dart
// injection_container.dart
final sl = GetIt.instance;

Future<void> configureDependencies() async {
  // ── External Services ──────────────────────────────────
  sl.registerLazySingleton(() => FirebaseFirestore.instance);
  sl.registerLazySingleton(() => FirebaseAuth.instance);
  sl.registerLazySingleton(() => FirebaseStorage.instance);
  sl.registerLazySingleton(() => FirebaseMessaging.instance);

  // ── Core Services ──────────────────────────────────────
  sl.registerLazySingleton<NotificationService>(() => NotificationServiceImpl());
  sl.registerLazySingleton<TtsService>(() => TtsServiceImpl());
  sl.registerLazySingleton<LocationService>(() => LocationServiceImpl());
  sl.registerLazySingleton<ShakeService>(() => ShakeServiceImpl());

  // ── Data Sources ───────────────────────────────────────
  sl.registerLazySingleton<AuthRemoteDataSource>(
      () => AuthRemoteDataSourceImpl(firebaseAuth: sl(), firestore: sl()));
  sl.registerLazySingleton<SosRemoteDataSource>(
      () => SosRemoteDataSourceImpl(firestore: sl(), functions: sl()));
  sl.registerLazySingleton<HealthRemoteDataSource>(
      () => HealthRemoteDataSourceImpl(firestore: sl()));
  sl.registerLazySingleton<HealthLocalDataSource>(
      () => HealthLocalDataSourceImpl(hiveBox: sl()));

  // ── Repositories ───────────────────────────────────────
  sl.registerLazySingleton<AuthRepository>(
      () => AuthRepositoryImpl(remote: sl()));
  sl.registerLazySingleton<SosRepository>(
      () => SosRepositoryImpl(remote: sl(), local: sl()));
  sl.registerLazySingleton<HealthRepository>(
      () => HealthRepositoryImpl(remote: sl(), local: sl()));

  // ── Use Cases ──────────────────────────────────────────
  sl.registerLazySingleton(() => TriggerSosUseCase(sl(), sl(), sl()));
  sl.registerLazySingleton(() => AddHealthRecordUseCase(sl()));
  sl.registerLazySingleton(() => ConfirmMedicationUseCase(sl()));
  sl.registerLazySingleton(() => SignInUseCase(sl()));
  sl.registerLazySingleton(() => SignUpUseCase(sl()));
}
```

---

## 9. SƠ ĐỒ SEQUENCE – LUỒNG SOS

```
NCT App         ShakeService     SosNotifier      SosRepository    Firebase FCM
    │                │                │                 │               │
    │──lắc điện thoại──▶              │                 │               │
    │                │──onShakeDetected──▶              │               │
    │                │                │──startSos()     │               │
    │                │                │                 │               │
    │◀───────────────────SosState.countdown(5)──────────│               │
    │  [hiển thị đếm ngược 5 giây]                      │               │
    │                │                │                 │               │
    │  [sau 5 giây]  │                │                 │               │
    │                │                │──SosState.sending()             │
    │                │                │──saveSosEvent()──▶              │
    │                │                │                 │──write Firestore
    │                │                │                 │◀──success─────│
    │                │                │──triggerAlert()──────────────────▶
    │                │                │                 │     FCM sent  │
    │                │                │                 │◀──delivered───│
    │◀───────────────────SosState.sent(event)───────────│               │
    │  [phát còi + hiển thị đã gửi]                     │               │
```

---

## 10. SƠ ĐỒ CLASS – SOS MODULE

```
┌──────────────────────────────────────────────────────────────────┐
│                        SOS MODULE CLASSES                        │
│                                                                  │
│  ┌─────────────────┐     uses    ┌──────────────────────────┐   │
│  │  SosNotifier    │────────────▶│   TriggerSosUseCase      │   │
│  │  (Riverpod)     │             │  + call(params): Result  │   │
│  └────────┬────────┘             └───────────┬──────────────┘   │
│           │ observes                         │ uses             │
│  ┌────────▼────────┐             ┌───────────▼──────────────┐   │
│  │  SosState       │             │   SosRepositoryImpl      │   │
│  │  (sealed class) │             │  + saveSosEvent()        │   │
│  │  • idle         │             │  + getSosHistory()       │   │
│  │  • countdown    │             └───────────┬──────────────┘   │
│  │  • sending      │                         │ implements       │
│  │  • sent         │             ┌───────────▼──────────────┐   │
│  │  • cancelled    │             │   SosRepository          │   │
│  │  • failed       │             │  (abstract interface)    │   │
│  └─────────────────┘             └──────────────────────────┘   │
│                                                                  │
│  ┌─────────────────┐     uses    ┌──────────────────────────┐   │
│  │  ShakeService   │────────────▶│   SosEventEntity         │   │
│  │  + onShake      │             │  + id, elderlyId         │   │
│  │  + sensitivity  │             │  + timestamp, location   │   │
│  └─────────────────┘             │  + triggerType, status   │   │
│                                  └──────────────────────────┘   │
└──────────────────────────────────────────────────────────────────┘
```

---

## 11. SƠ ĐỒ COMPONENT

```
┌─────────────────────────────────────────────────────────────────┐
│                    FLUTTER APP COMPONENTS                       │
│                                                                 │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────────┐   │
│  │   Auth   │  │   SOS    │  │   Med.   │  │   Health     │   │
│  │ Component│  │Component │  │Component │  │  Component   │   │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └──────┬───────┘   │
│       │              │              │               │           │
│  ┌────▼──────────────▼──────────────▼───────────────▼───────┐  │
│  │                    CORE SERVICES                          │  │
│  │  NotificationSvc │ TtsSvc │ LocationSvc │ ShakeSvc       │  │
│  └────┬──────────────┬────────────────────────────┬──────────┘  │
│       │              │                            │             │
│  ┌────▼──────────────▼────────────────────────────▼──────────┐  │
│  │                   FIREBASE SERVICES                        │  │
│  │  FirebaseAuth │ Firestore │ FCM │ Storage │ Functions      │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 12. OFFLINE STRATEGY

```
┌───────────────────────────────────────────────────────────────┐
│                   OFFLINE / ONLINE FLOW                       │
│                                                               │
│  App Start                                                    │
│      │                                                        │
│      ├─ Has Internet? ──Yes──▶ Firestore real-time sync       │
│      │                         + update Hive cache            │
│      │                                                        │
│      └─ No Internet ──▶ Load từ Hive cache                   │
│                          + Queue writes locally               │
│                          + Show offline banner                │
│                                                               │
│  SOS khi offline:                                             │
│      │                                                        │
│      ├─ Save to Hive queue                                    │
│      ├─ Try SMS fallback (nếu có cấu hình)                    │
│      └─ Sync khi có Internet                                  │
│                                                               │
│  Dữ liệu cache Hive:                                          │
│  • Hồ sơ NCT            (box: elderly_profiles)              │
│  • Lịch thuốc active    (box: medication_schedules)           │
│  • Sức khỏe 7 ngày gần  (box: health_records)                │
│  • Danh sách SOS 30 ngày(box: sos_events)                    │
│  • User session         (box: user_session, encrypted)        │
└───────────────────────────────────────────────────────────────┘
```

---

## 13. TECHNOLOGY STACK & LÝ DO CHỌN

| Thành phần | Công nghệ | Lý do chọn |
|-----------|-----------|------------|
| Frontend | Flutter 3.24+ | Cross-platform, performance tốt, Dart type-safe |
| State Mgmt | Riverpod 2.x | Compile-safe, no context, testable, better than Provider |
| Navigation | GoRouter 14.x | Declarative, deep link, redirect, shell route |
| Architecture | Clean Architecture | Tách biệt concerns, testable, scalable |
| DI | GetIt + injectable | Lightweight, không phụ thuộc context |
| Local Storage | Hive (encrypted) | Nhanh, NoSQL, hỗ trợ offline, encrypt sensitive data |
| Backend | Firebase | Serverless, real-time, auto-scale, tiết kiệm chi phí |
| Database | Cloud Firestore | Real-time stream, offline sync, hierarchical |
| Auth | Firebase Auth | OTP/SMS, Google, token management |
| Push Notif | FCM | Reliable, free tier đủ dùng, priority messages |
| Storage | Firebase Storage | Media files, avatar ảnh |
| Maps | Google Maps Flutter | API quen thuộc, chất lượng cao tại VN |
| GPS | Geolocator | Plugin chính thức, background mode |
| TTS | flutter_tts | Hỗ trợ tiếng Việt, điều chỉnh tốc độ/pitch |
| Shake | sensors_plus | Accelerometer chính xác, stream-based |
| Charts | fl_chart | Đẹp, tùy biến cao, pure Dart |
| PDF | pdf + printing | Báo cáo offline, không cần server |
| Monitoring | Crashlytics + Perf | Debug lỗi production, track performance |
| CI/CD | GitHub Actions | Tự động hóa build + test + deploy |

---

## 14. CLOUD FUNCTIONS ARCHITECTURE

```
functions/
├── src/
│   ├── sos/
│   │   └── sosAlertDispatcher.ts    # Trigger: Firestore onCreate sos_events
│   │                                # Action: Gửi FCM đến người thân
│   │                                #         Gửi SMS fallback
│   │                                #         Tạo audit log
│   │
│   ├── medication/
│   │   └── missedMedicationAlert.ts # Trigger: Pub/Sub scheduled
│   │                                # Check: Log chưa xác nhận sau 30 phút
│   │                                # Action: FCM cảnh báo người thân
│   │
│   ├── health/
│   │   └── healthThresholdChecker.ts# Trigger: Firestore onCreate health_records
│   │                                # Check: So sánh với ngưỡng cấu hình
│   │                                # Action: FCM cảnh báo nếu bất thường
│   │
│   ├── reports/
│   │   └── reportGenerator.ts       # HTTPS callable
│   │                                # Input: elderlyId, period
│   │                                # Output: PDF/Excel data
│   │
│   └── audit/
│       └── auditLogger.ts           # Trigger: Auth + Firestore writes
│                                    # Action: Ghi audit_logs collection
└── package.json
```

---

## 15. KẾT LUẬN GIAI ĐOẠN 3

**Quyết định kiến trúc chính:**
1. **Clean Architecture + Feature-first** – phù hợp team 2–5 người, dễ phân chia task
2. **Riverpod** thay vì BLoC – giảm boilerplate, dễ test, compile-safe
3. **Firebase all-in-one** – tiết kiệm chi phí xây dựng backend, auto-scale
4. **Hive encrypted** cho offline – nhanh hơn SQLite, hỗ trợ mã hóa sẵn
5. **GoRouter** – declarative navigation, dễ kiểm soát deep link và RBAC redirect

**Đầu ra chuyển sang Giai đoạn 4:** Kiến trúc, folder structure, technology stack → làm nền tảng thiết kế UI/UX và sau đó là CSDL/Backend.
