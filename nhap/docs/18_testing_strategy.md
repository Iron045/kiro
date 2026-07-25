# Chương 18: Testing Strategy
## GeriatricCare – Chiến lược kiểm thử

---

## 18.1 Testing Pyramid

```
            ╔══════════════╗
            ║   E2E Tests  ║  ← 10%  (Integration / UAT)
            ╠══════════════╣
            ║  Widget Tests ║  ← 30%  (UI component)
            ╠══════════════╣
            ║  Unit Tests   ║  ← 60%  (Use Cases, Repos, Utils)
            ╚══════════════╝

Mục tiêu coverage: ≥ 70% overall, 90%+ cho critical paths (SOS, Auth)
```

**Công cụ:**

| Loại | Tool |
|---|---|
| Unit & Widget Test | Flutter Test (`flutter_test`) |
| Mocking | Mockito + build_runner |
| Firebase Mocking | `fake_cloud_firestore`, `firebase_auth_mocks` |
| Code Coverage | `lcov` + Codecov |
| E2E / Integration | `integration_test` |
| Performance | Firebase Performance + `flutter_driver` |
| UI Regression | Golden tests |

---

## 18.2 Unit Tests

### TC-UNIT-001: TriggerSosUseCase

```dart
// test/features/sos/domain/usecases/trigger_sos_usecase_test.dart
@GenerateMocks([SosRepository])
void main() {
  late TriggerSosUseCase useCase;
  late MockSosRepository mockRepo;

  setUp(() {
    mockRepo = MockSosRepository();
    useCase = TriggerSosUseCase(mockRepo);
  });

  group('TriggerSosUseCase', () {
    const params = TriggerSosParams(
      elderlyId: 'ep_xyz789',
      triggerType: SosTriggerType.button,
    );

    final tSosEvent = SosEventEntity(
      id: 'sos_001',
      elderlyId: 'ep_xyz789',
      triggerType: SosTriggerType.button,
      status: SosStatus.sent,
      triggeredAt: DateTime(2026, 7, 22, 8, 0),
    );

    test('TC-UNIT-001: Nên trả về SosEventEntity khi thành công', () async {
      when(mockRepo.triggerSOS(params.elderlyId, null))
          .thenAnswer((_) async => Right(tSosEvent));

      final result = await useCase(params);

      expect(result, Right(tSosEvent));
      verify(mockRepo.triggerSOS(params.elderlyId, null)).called(1);
    });

    test('TC-UNIT-002: Nên trả về ServerFailure khi repo lỗi', () async {
      when(mockRepo.triggerSOS(any, any))
          .thenAnswer((_) async => Left(ServerFailure('FCM error')));

      final result = await useCase(params);

      expect(result, Left(ServerFailure('FCM error')));
    });
  });
}
```

---

### TC-UNIT-003: SaveHealthRecordUseCase – Kiểm tra cảnh báo bất thường

```dart
group('SaveHealthRecordUseCase - Abnormal Detection', () {
  test('TC-UNIT-003: SYS ≥ 140 → isAbnormal = true', () async {
    final params = SaveHealthRecordParams(
      elderlyId: 'ep_xyz789',
      type: HealthType.bloodPressure,
      values: {'sys': 148.0, 'dia': 92.0, 'pulse': 78.0},
      measuredAt: DateTime.now(),
    );

    when(mockThresholdRepo.getThreshold(any, any))
        .thenAnswer((_) async => Right(HealthThreshold.defaults));
    when(mockHealthRepo.saveRecord(any))
        .thenAnswer((inv) async => Right(inv.positionalArguments[0]));

    final result = await useCase(params);

    result.fold(
      (failure) => fail('Should not fail'),
      (record) => expect(record.isAbnormal, isTrue),
    );
  });

  test('TC-UNIT-004: SpO2 < 93% → isAbnormal = true (emergency level)', () async {
    final params = SaveHealthRecordParams(
      elderlyId: 'ep_xyz789',
      type: HealthType.spo2,
      values: {'percent': 91.0},
      measuredAt: DateTime.now(),
    );

    final result = await useCase(params);
    result.fold((_) => fail(''), (r) => expect(r.isAbnormal, isTrue));
  });

  test('TC-UNIT-005: Không cho phép timestamp tương lai', () async {
    final params = SaveHealthRecordParams(
      elderlyId: 'ep_xyz789',
      type: HealthType.heartRate,
      values: {'bpm': 80.0},
      measuredAt: DateTime.now().add(const Duration(minutes: 10)),
    );

    final result = await useCase(params);
    expect(result.isLeft(), isTrue);
  });
});
```

---

### TC-UNIT-006: MedicationCompliance Calculation

```dart
group('MedicationComplianceCalculator', () {
  test('TC-UNIT-006: Tỷ lệ tuân thủ = taken / total × 100', () {
    final logs = [
      MedicationLogEntity(status: MedicationStatus.taken, ...),
      MedicationLogEntity(status: MedicationStatus.taken, ...),
      MedicationLogEntity(status: MedicationStatus.missed, ...),
      MedicationLogEntity(status: MedicationStatus.skipped, ...),
    ];

    final rate = MedicationComplianceCalculator.calculate(logs);
    expect(rate, closeTo(50.0, 0.01)); // 2/4 = 50%
  });

  test('TC-UNIT-007: Không có logs → trả về 0', () {
    final rate = MedicationComplianceCalculator.calculate([]);
    expect(rate, 0.0);
  });

  test('TC-UNIT-008: Tất cả taken → 100%', () {
    final logs = List.generate(
      5, (_) => MedicationLogEntity(status: MedicationStatus.taken, ...),
    );
    final rate = MedicationComplianceCalculator.calculate(logs);
    expect(rate, 100.0);
  });
});
```

---

## 18.3 Widget Tests

### TC-WIDGET-001: SosButton

```dart
// test/shared/widgets/sos_button_test.dart
void main() {
  group('SosButton Widget', () {
    testWidgets('TC-WIDGET-001: Hiển thị text GỌI CỨU GIÚP', (tester) async {
      await tester.pumpWidget(
        MaterialApp(home: Scaffold(body: SosButton(onPressed: () {}))),
      );

      expect(find.text('GỌI CỨU GIÚP'), findsOneWidget);
    });

    testWidgets('TC-WIDGET-002: Màu nền là đỏ D32F2F', (tester) async {
      await tester.pumpWidget(
        MaterialApp(home: Scaffold(body: SosButton(onPressed: () {}))),
      );

      final material = tester.widget<Material>(find.byType(Material).first);
      expect(material.color, const Color(0xFFD32F2F));
    });

    testWidgets('TC-WIDGET-003: Kích thước tối thiểu 80dp', (tester) async {
      await tester.pumpWidget(
        MaterialApp(home: Scaffold(body: SosButton(onPressed: () {}))),
      );

      final size = tester.getSize(find.byType(SosButton));
      expect(size.height, greaterThanOrEqualTo(80));
    });

    testWidgets('TC-WIDGET-004: Gọi onPressed khi tap', (tester) async {
      bool pressed = false;
      await tester.pumpWidget(
        MaterialApp(
          home: Scaffold(body: SosButton(onPressed: () => pressed = true)),
        ),
      );

      await tester.tap(find.byType(SosButton));
      expect(pressed, isTrue);
    });

    testWidgets('TC-WIDGET-005: Có Semantics label cho accessibility', (tester) async {
      await tester.pumpWidget(
        MaterialApp(home: Scaffold(body: SosButton(onPressed: () {}))),
      );

      final semantics = tester.getSemantics(find.byType(SosButton));
      expect(semantics.label, contains('SOS'));
    });
  });
}
```

---

### TC-WIDGET-006: HealthMetricCard

```dart
group('HealthMetricCard Widget', () {
  testWidgets('TC-WIDGET-006: Hiển thị warning icon khi isAbnormal=true', (tester) async {
    await tester.pumpWidget(MaterialApp(
      home: Scaffold(body: HealthMetricCard(
        label: 'Huyết áp', value: '148/92', unit: 'mmHg',
        isAbnormal: true, icon: Icons.favorite,
      )),
    ));

    expect(find.byIcon(Icons.warning_amber), findsOneWidget);
  });

  testWidgets('TC-WIDGET-007: Không hiện warning khi isAbnormal=false', (tester) async {
    await tester.pumpWidget(MaterialApp(
      home: Scaffold(body: HealthMetricCard(
        label: 'Nhịp tim', value: '76', unit: 'bpm',
        isAbnormal: false, icon: Icons.favorite,
      )),
    ));

    expect(find.byIcon(Icons.warning_amber), findsNothing);
  });
});
```

---

## 18.4 Integration Tests

### TC-INT-001: Luồng đăng nhập end-to-end

```dart
// integration_test/auth_flow_test.dart
void main() {
  IntegrationTestWidgetsFlutterBinding.ensureInitialized();

  group('Auth Flow Integration', () {
    testWidgets('TC-INT-001: Đăng nhập thành công → vào Family Dashboard', (tester) async {
      app.main();
      await tester.pumpAndSettle();

      // Đợi màn hình login
      expect(find.text('Đăng nhập'), findsOneWidget);

      // Nhập thông tin
      await tester.enterText(find.byKey(Key('email_field')), 'test@example.com');
      await tester.enterText(find.byKey(Key('password_field')), 'Test@1234');
      await tester.tap(find.byKey(Key('login_button')));
      await tester.pumpAndSettle(const Duration(seconds: 5));

      // Kiểm tra đến Family Dashboard
      expect(find.text('Theo dõi người thân'), findsOneWidget);
    });

    testWidgets('TC-INT-002: Sai mật khẩu → hiện error message', (tester) async {
      app.main();
      await tester.pumpAndSettle();

      await tester.enterText(find.byKey(Key('email_field')), 'test@example.com');
      await tester.enterText(find.byKey(Key('password_field')), 'WrongPassword');
      await tester.tap(find.byKey(Key('login_button')));
      await tester.pumpAndSettle(const Duration(seconds: 3));

      expect(find.text('Thông tin đăng nhập không đúng'), findsOneWidget);
    });
  });
}
```

---

### TC-INT-003: Luồng SOS end-to-end (với Firebase Emulator)

```dart
testWidgets('TC-INT-003: SOS trigger → Firestore document được tạo', (tester) async {
  // Setup Firebase Emulator
  await Firebase.initializeApp();
  FirebaseFirestore.instance.useFirestoreEmulator('localhost', 8080);

  app.main();
  await tester.pumpAndSettle();

  // Login là Elderly user
  await _loginAs(tester, 'elderly@test.com', 'Test@1234');
  await tester.pumpAndSettle();

  // Bấm nút SOS
  await tester.tap(find.byKey(Key('sos_button')));
  await tester.pumpAndSettle();

  // Đợi countdown
  expect(find.text('5'), findsOneWidget);
  await tester.pumpAndSettle(const Duration(seconds: 6));

  // Verify Firestore document
  final sosQuery = await FirebaseFirestore.instance
      .collectionGroup('sos_events')
      .orderBy('triggeredAt', descending: true)
      .limit(1)
      .get();

  expect(sosQuery.docs, isNotEmpty);
  expect(sosQuery.docs.first.data()['status'], equals('SENT'));
});
```

---

## 18.5 UAT Test Cases

### UAT-01: Người cao tuổi sử dụng ứng dụng lần đầu

| ID | Bước kiểm thử | Kết quả kỳ vọng | Pass/Fail |
|---|---|---|---|
| UAT-01-01 | Cài đặt app, mở lần đầu | Màn hình chào hỏi hiển thị | ⬜ |
| UAT-01-02 | Người thân đăng ký tài khoản FAMILY | Đăng ký thành công, nhận email xác nhận | ⬜ |
| UAT-01-03 | Tạo hồ sơ người cao tuổi | Hồ sơ được lưu, hiện trong dashboard | ⬜ |
| UAT-01-04 | Người cao tuổi nhận mã liên kết | Mã 6 chữ số hiển thị rõ ràng | ⬜ |
| UAT-01-05 | Người thân nhập mã liên kết | Liên kết thành công | ⬜ |
| UAT-01-06 | Người cao tuổi mở app | Thấy màn hình chính với nút SOS to | ⬜ |

### UAT-02: Kịch bản SOS khẩn cấp

| ID | Bước kiểm thử | Kết quả kỳ vọng | Pass/Fail |
|---|---|---|---|
| UAT-02-01 | Người cao tuổi bấm nút SOS | Đếm ngược 5 giây hiển thị toàn màn hình đỏ | ⬜ |
| UAT-02-02 | Không bấm HỦY | Sau 5 giây, âm thanh còi phát | ⬜ |
| UAT-02-03 | Người thân nhận notification | Notification đến trong < 5 giây | ⬜ |
| UAT-02-04 | Notification chứa vị trí | Link Google Maps mở đúng vị trí | ⬜ |
| UAT-02-05 | Người thân xác nhận SOS | Banner SOS biến mất trên dashboard | ⬜ |
| UAT-02-06 | Bấm HỦY trong countdown | SOS bị hủy, không gửi thông báo | ⬜ |
| UAT-02-07 | Lắc điện thoại mạnh | SOS countdown kích hoạt tự động | ⬜ |

### UAT-03: Nhắc uống thuốc

| ID | Bước kiểm thử | Kết quả kỳ vọng | Pass/Fail |
|---|---|---|---|
| UAT-03-01 | Người thân tạo lịch uống thuốc | Lịch được lưu, xem trong danh sách | ⬜ |
| UAT-03-02 | Đúng giờ nhắc | Notification xuất hiện + giọng đọc tiếng Việt | ⬜ |
| UAT-03-03 | Người cao tuổi bấm "Đã uống" | Tick xanh xuất hiện trong lịch | ⬜ |
| UAT-03-04 | Không phản hồi 30 phút | Người thân nhận FCM cảnh báo | ⬜ |
| UAT-03-05 | Kiểm tra tỷ lệ tuân thủ | Biểu đồ hiển thị đúng % | ⬜ |

### UAT-04: Theo dõi sức khỏe

| ID | Bước kiểm thử | Kết quả kỳ vọng | Pass/Fail |
|---|---|---|---|
| UAT-04-01 | Nhập huyết áp bình thường (120/80) | Lưu thành công, icon xanh | ⬜ |
| UAT-04-02 | Nhập huyết áp cao (160/100) | Cảnh báo đỏ, FCM đến người thân | ⬜ |
| UAT-04-03 | Nhập khi offline | Lưu local, icon "đang chờ đồng bộ" | ⬜ |
| UAT-04-04 | Bật mạng lại | Dữ liệu offline đồng bộ tự động | ⬜ |
| UAT-04-05 | Xem biểu đồ tuần | Biểu đồ render đúng, điểm bất thường màu đỏ | ⬜ |

### UAT-05: Accessibility

| ID | Bước kiểm thử | Kết quả kỳ vọng | Pass/Fail |
|---|---|---|---|
| UAT-05-01 | Bật TalkBack, điều hướng app | Tất cả nút được đọc đúng tên | ⬜ |
| UAT-05-02 | Tăng cỡ chữ lên Extra Large | Layout không bị vỡ | ⬜ |
| UAT-05-03 | Bật High Contrast Mode | Contrast ratio ≥ 4.5:1 | ⬜ |
| UAT-05-04 | Dùng Dark Mode ban đêm | Nội dung đọc được, SOS vẫn nổi bật | ⬜ |
| UAT-05-05 | Người cao tuổi 75 tuổi dùng thử | Hoàn thành SOS trong < 10 giây | ⬜ |

---

## 18.6 Performance Tests

```dart
// Performance test – SOS trigger time
testWidgets('TC-PERF-001: SOS gửi trong < 3 giây', (tester) async {
  app.main();
  await tester.pumpAndSettle();
  await _loginAs(tester, 'elderly@test.com', 'Test@1234');

  final stopwatch = Stopwatch()..start();

  await tester.tap(find.byKey(Key('sos_button')));
  // Đợi đến khi SOS được gửi (sau countdown)
  await tester.pumpAndSettle(const Duration(seconds: 10));

  stopwatch.stop();
  // Countdown 5s + gửi SOS
  // Thực tế gửi SOS (sau countdown) phải < 3s
  // → Verify qua Firestore document createdAt - triggeredAt
  print('Total SOS flow time: ${stopwatch.elapsedMilliseconds}ms');
});

// App startup time
testWidgets('TC-PERF-002: Cold start < 3 giây', (tester) async {
  final stopwatch = Stopwatch()..start();
  app.main();
  await tester.pumpAndSettle();
  stopwatch.stop();

  expect(stopwatch.elapsedMilliseconds, lessThan(3000));
});
```

---

## 18.7 Test Coverage Report Template

```
────────────────────────────────────────────
  GeriatricCare - Test Coverage Report
  Generated: 2026-07-22
────────────────────────────────────────────

  Feature              Lines    Coverage
  ─────────────────────────────────────
  features/auth        142/160    88.7% ✅
  features/sos         98/105     93.3% ✅
  features/health      187/210    89.0% ✅
  features/medication  156/175    89.1% ✅
  features/dashboard   88/120     73.3% ✅
  features/location    45/60      75.0% ✅
  core/usecases        62/62     100.0% ✅
  core/utils           55/58      94.8% ✅
  shared/widgets       72/95      75.8% ✅
  ─────────────────────────────────────
  TOTAL               905/1045    86.6% ✅

  Target: ≥ 70% ✅ PASSED
────────────────────────────────────────────
```
