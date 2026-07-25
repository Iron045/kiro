# TÀI LIỆU THAM KHẢO

---

## Sách và giáo trình

[1] Robert C. Martin. (2017). *Clean Architecture: A Craftsman's Guide to Software Structure and Design*. Prentice Hall.

[2] Eric Evans. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley Professional.

[3] Grady Booch, James Rumbaugh, Ivar Jacobson. (2005). *The Unified Modeling Language User Guide* (2nd ed.). Addison-Wesley.

[4] Robert C. Martin. (2008). *Clean Code: A Handbook of Agile Software Craftsmanship*. Prentice Hall.

[5] Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides. (1994). *Design Patterns: Elements of Reusable Object-Oriented Software*. Addison-Wesley.

[6] Mike Cohn. (2004). *User Stories Applied: For Agile Software Development*. Addison-Wesley Professional.

---

## Tài liệu kỹ thuật và đặc tả

[7] Flutter Development Team. (2024). *Flutter Documentation*. Google LLC. Truy cập từ https://docs.flutter.dev

[8] Firebase Team. (2024). *Firebase Documentation*. Google LLC. Truy cập từ https://firebase.google.com/docs

[9] OWASP Foundation. (2024). *OWASP Mobile Application Security Verification Standard (MASVS) v2.0*. Truy cập từ https://mas.owasp.org/MASVS

[10] OWASP Foundation. (2023). *OWASP Mobile Security Testing Guide (MSTG)*. Truy cập từ https://owasp.org/www-project-mobile-security-testing-guide

[11] IEEE. (1998). *IEEE Std 830-1998: IEEE Recommended Practice for Software Requirements Specifications*. IEEE Computer Society.

[12] W3C Web Accessibility Initiative. (2023). *Web Content Accessibility Guidelines (WCAG) 2.2*. W3C Recommendation. Truy cập từ https://www.w3.org/TR/WCAG22

[13] Google. (2023). *Material Design 3 Guidelines*. Truy cập từ https://m3.material.io

[14] Riverpod Team. (2024). *Riverpod Documentation*. Truy cập từ https://riverpod.dev/docs

[15] Object Management Group. (2017). *Unified Modeling Language Specification Version 2.5.1*. OMG Document. Truy cập từ https://www.omg.org/spec/UML/2.5.1

---

## Bài báo và nghiên cứu khoa học

[16] Tổng cục Thống kê Việt Nam. (2023). *Báo cáo điều tra dân số và nhà ở giữa kỳ năm 2023*. Nhà xuất bản Thống kê, Hà Nội.

[17] WHO. (2022). *Ageing and Health: Key Facts*. World Health Organization. Truy cập từ https://www.who.int/news-room/fact-sheets/detail/ageing-and-health

[18] Gia, K. N., et al. (2019). *IoT-based Continuous Glucose Monitoring System: A Feasibility Study*. *Procedia Computer Science*, 109, 327–334.

[19] Majumder, S., et al. (2017). *Smart Homes for Elderly Healthcare—An IoT Based Approach*. Sensors, 17(11), 2496.

[20] Fagerström, C., et al. (2020). *Fall Detection Algorithm for Accelerometer Data: A Machine Learning Approach*. *Journal of Medical Systems*, 44(5), 1–10.

---

## Các nguồn trực tuyến

[21] Kodeco (formerly Ray Wenderlich). (2024). *Flutter & Dart Tutorials*. Truy cập từ https://www.kodeco.com/flutter

[22] pub.dev. (2024). *Flutter & Dart Package Repository*. Truy cập từ https://pub.dev

[23] Stack Overflow. (2024). *Flutter Community Q&A*. Truy cập từ https://stackoverflow.com/questions/tagged/flutter

---

# PHỤ LỤC

---

## Phụ lục A: Cài đặt môi trường phát triển

### A.1 Yêu cầu phần mềm

```bash
# 1. Cài đặt Flutter SDK
# Tải từ https://docs.flutter.dev/get-started/install
flutter --version   # Kiểm tra: Flutter 3.19.0+

# 2. Kiểm tra môi trường
flutter doctor

# 3. Clone dự án
git clone https://github.com/[username]/geriatriccare.git
cd geriatriccare

# 4. Cài đặt dependencies
flutter pub get

# 5. Sinh code (Freezed, Riverpod, GetIt)
flutter pub run build_runner build --delete-conflicting-outputs
```

### A.2 Cấu hình Firebase

```bash
# Cài đặt Firebase CLI
npm install -g firebase-tools

# Đăng nhập Firebase
firebase login

# Khởi tạo FlutterFire
dart pub global activate flutterfire_cli
flutterfire configure --project=geriatriccare-dev
```

### A.3 Chạy ứng dụng

```bash
# Development
flutter run --flavor dev -t lib/main_dev.dart

# Staging
flutter run --flavor staging -t lib/main_staging.dart

# Build Production APK
flutter build apk --flavor prod -t lib/main_prod.dart --release

# Build Production App Bundle (Play Store)
flutter build appbundle --flavor prod -t lib/main_prod.dart --release
```

---

## Phụ lục B: pubspec.yaml đầy đủ

```yaml
name: geriatric_care
description: GeriatricCare – Hệ thống Giám sát Người cao tuổi
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
  dartz: ^0.10.1

  # Local Storage
  hive: ^2.2.3
  hive_flutter: ^1.1.0

  # Location & Maps
  google_maps_flutter: ^2.5.3
  geolocator: ^10.1.0

  # Sensors
  sensors_plus: ^4.0.2

  # Notifications
  flutter_local_notifications: ^16.3.0

  # TTS
  flutter_tts: ^3.8.5

  # Charts
  fl_chart: ^0.66.1

  # Network
  http: ^1.2.0
  connectivity_plus: ^5.0.2

  # Utilities
  intl: ^0.19.0
  uuid: ^4.3.3
  permission_handler: ^11.2.0
  url_launcher: ^6.2.4
  share_plus: ^7.2.2
  image_picker: ^1.0.7
  cached_network_image: ^3.3.1

  # PDF & Excel
  pdf: ^3.10.8
  printing: ^5.12.0
  excel: ^4.0.2

  # Security
  flutter_secure_storage: ^9.0.0
  local_auth: ^2.1.8

  # Logging
  logger: ^2.1.0
  workmanager: ^0.5.2

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
  integration_test:
    sdk: flutter
```

---

## Phụ lục C: Firebase Security Rules hoàn chỉnh

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // ===== HELPER FUNCTIONS =====
    function isAuthenticated() {
      return request.auth != null;
    }
    function isOwner(userId) {
      return request.auth.uid == userId;
    }
    function getUserRole() {
      return get(/databases/$(database)/documents/users/$(request.auth.uid))
             .data.role;
    }
    function isAdmin() { return getUserRole() == 'ADMIN'; }
    function isElderly() { return getUserRole() == 'ELDERLY'; }
    function isFamily() { return getUserRole() == 'FAMILY'; }
    function isDoctor() { return getUserRole() == 'DOCTOR'; }

    function isFamilyOfElderly(elderlyId) {
      return exists(/databases/$(database)/documents/family_links/
             $(request.auth.uid + '_' + elderlyId))
          && get(/databases/$(database)/documents/family_links/
             $(request.auth.uid + '_' + elderlyId)).data.status == 'ACTIVE';
    }
    function isDoctorOfElderly(elderlyId) {
      return exists(/databases/$(database)/documents/doctor_links/
             $(request.auth.uid + '_' + elderlyId))
          && get(/databases/$(database)/documents/doctor_links/
             $(request.auth.uid + '_' + elderlyId)).data.status == 'ACTIVE';
    }
    function isElderlyOwner(elderlyId) {
      return get(/databases/$(database)/documents/elderly_profiles/
             $(elderlyId)).data.userId == request.auth.uid;
    }
    function canReadElderlyData(elderlyId) {
      return isAdmin() || isElderlyOwner(elderlyId)
          || isFamilyOfElderly(elderlyId) || isDoctorOfElderly(elderlyId);
    }
    function canWriteElderlyData(elderlyId) {
      return isAdmin() || isElderlyOwner(elderlyId)
          || isFamilyOfElderly(elderlyId);
    }

    // ===== USERS =====
    match /users/{userId} {
      allow read: if isAuthenticated() && (isOwner(userId) || isAdmin());
      allow create: if isAuthenticated() && isOwner(userId);
      allow update: if isAuthenticated() && (isOwner(userId) || isAdmin());
      allow delete: if isAdmin();
    }

    // ===== ELDERLY PROFILES =====
    match /elderly_profiles/{elderlyId} {
      allow read: if isAuthenticated() && canReadElderlyData(elderlyId);
      allow create: if isAuthenticated() && (isElderly() || isFamily());
      allow update: if isAuthenticated() && canWriteElderlyData(elderlyId);
      allow delete: if isAdmin();

      match /health_records/{recordId} {
        allow read: if isAuthenticated() && canReadElderlyData(elderlyId);
        allow create: if isAuthenticated() && canWriteElderlyData(elderlyId)
            && request.resource.data.measuredAt
               <= request.time + duration.value(5, 'm');
        allow update, delete: if false; // Immutable
      }

      match /sos_events/{sosId} {
        allow read: if isAuthenticated() && canReadElderlyData(elderlyId);
        allow create: if isAuthenticated() && isElderlyOwner(elderlyId);
        allow update: if isAuthenticated()
            && (isFamilyOfElderly(elderlyId) || isElderlyOwner(elderlyId)
                || isAdmin());
        allow delete: if false;
      }

      match /medication_schedules/{scheduleId} {
        allow read: if isAuthenticated() && canReadElderlyData(elderlyId);
        allow create, update: if isAuthenticated()
            && (isFamilyOfElderly(elderlyId)
                || isDoctorOfElderly(elderlyId) || isAdmin());
        allow delete: if isAuthenticated()
            && (isFamilyOfElderly(elderlyId) || isAdmin());

        match /medication_logs/{logId} {
          allow read: if isAuthenticated() && canReadElderlyData(elderlyId);
          allow create: if isAuthenticated()
              && (isElderlyOwner(elderlyId) || isFamilyOfElderly(elderlyId));
          allow update, delete: if false; // Immutable
        }
      }

      match /location_history/{locationId} {
        allow read: if isAuthenticated()
            && (isElderlyOwner(elderlyId) || isFamilyOfElderly(elderlyId)
                || isAdmin());
        allow create: if isAuthenticated() && isElderlyOwner(elderlyId);
        allow update, delete: if false;
      }
    }

    // ===== NOTIFICATION SETTINGS =====
    match /notification_settings/{userId} {
      allow read: if isAuthenticated() && isOwner(userId);
      // SOS alerts không thể tắt
      allow write: if isAuthenticated() && isOwner(userId)
          && request.resource.data.sosAlerts == true;
    }
  }
}
```

---

## Phụ lục D: Cấu hình CI/CD GitHub Actions

```yaml
# .github/workflows/ci.yml
name: GeriatricCare CI/CD

on:
  push:
    branches: [main, develop, staging]
  pull_request:
    branches: [main, develop]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: subosito/flutter-action@v2
        with:
          flutter-version: '3.19.0'
      - run: flutter pub get
      - run: flutter pub run build_runner build --delete-conflicting-outputs
      - run: flutter analyze
      - run: flutter test --coverage
      - uses: codecov/codecov-action@v3
        with:
          file: coverage/lcov.info

  build-staging:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/staging'
    steps:
      - uses: actions/checkout@v4
      - uses: subosito/flutter-action@v2
      - run: echo "${{ secrets.KEYSTORE_BASE64 }}"
             | base64 --decode > android/app/keystore.jks
      - run: flutter build apk
               --flavor staging
               -t lib/main_staging.dart
               --release
      - uses: wzieba/Firebase-Distribution-Github-Action@v1
        with:
          appId: ${{ secrets.FIREBASE_STAGING_APP_ID }}
          serviceCredentialsFileContent:
            ${{ secrets.FIREBASE_SERVICE_ACCOUNT }}
          groups: testers
          file: build/app/outputs/flutter-apk/app-staging-release.apk

  deploy-prod:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment: production
    steps:
      - uses: actions/checkout@v4
      - uses: subosito/flutter-action@v2
      - run: flutter build appbundle
               --flavor prod
               -t lib/main_prod.dart
               --release
               --obfuscate
               --split-debug-info=build/debug-info
      - uses: r0adkll/upload-google-play@v1
        with:
          serviceAccountJsonPlainText:
            ${{ secrets.PLAY_STORE_SERVICE_ACCOUNT }}
          packageName: com.geriatriccare.app
          releaseFiles: build/app/outputs/bundle/prodRelease/app-prod-release.aab
          track: internal
```

---

## Phụ lục E: Ngưỡng sức khỏe mặc định (health_thresholds/default)

```json
{
  "BLOOD_PRESSURE_SYS_MIN": 90,
  "BLOOD_PRESSURE_SYS_MAX": 139,
  "BLOOD_PRESSURE_DIA_MIN": 60,
  "BLOOD_PRESSURE_DIA_MAX": 89,
  "HEART_RATE_MIN": 60,
  "HEART_RATE_MAX": 100,
  "GLUCOSE_FASTING_MIN": 3.9,
  "GLUCOSE_FASTING_MAX": 6.9,
  "GLUCOSE_POSTPRANDIAL_MAX": 11.0,
  "TEMPERATURE_MIN": 35.5,
  "TEMPERATURE_MAX": 38.0,
  "SPO2_MIN": 93,
  "SPO2_EMERGENCY": 90,
  "BMI_MIN": 18.5,
  "BMI_MAX": 29.9,
  "updatedBy": null,
  "updatedAt": "2026-07-22T00:00:00Z"
}
```

*Ghi chú: Ngưỡng trên dựa theo hướng dẫn của WHO và Bộ Y tế Việt Nam. Bác sĩ có thể điều chỉnh ngưỡng riêng cho từng bệnh nhân thông qua Doctor Dashboard.*
