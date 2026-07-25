# Chương 17: Deployment Architecture
## GeriatricCare – Kiến trúc triển khai

---

## 17.1 Tổng quan môi trường

| Môi trường | Mục đích | Firebase Project |
|---|---|---|
| **Development** | Dev cá nhân, thử nghiệm tính năng | `geriatriccare-dev` |
| **Staging** | QA testing, UAT | `geriatriccare-staging` |
| **Production** | Người dùng thực | `geriatriccare-prod` |

---

## 17.2 Firebase Project Configuration

### 17.2.1 Cấu hình Flavors trong Flutter

```dart
// lib/core/constants/app_config.dart
enum AppEnvironment { dev, staging, prod }

class AppConfig {
  final AppEnvironment environment;
  final String firebaseProjectId;
  final String apiBaseUrl;
  final bool enableCrashlytics;
  final bool enableAnalytics;
  final bool enablePerformanceMonitoring;

  static AppConfig? _instance;

  static AppConfig get instance {
    assert(_instance != null, 'AppConfig must be initialized');
    return _instance!;
  }

  static void initialize(AppEnvironment env) {
    _instance = switch (env) {
      AppEnvironment.dev => AppConfig._(
          environment: env,
          firebaseProjectId: 'geriatriccare-dev',
          apiBaseUrl: 'https://us-central1-geriatriccare-dev.cloudfunctions.net/api/v1',
          enableCrashlytics: false,
          enableAnalytics: false,
          enablePerformanceMonitoring: false,
        ),
      AppEnvironment.staging => AppConfig._(
          environment: env,
          firebaseProjectId: 'geriatriccare-staging',
          apiBaseUrl: 'https://us-central1-geriatriccare-staging.cloudfunctions.net/api/v1',
          enableCrashlytics: true,
          enableAnalytics: false,
          enablePerformanceMonitoring: true,
        ),
      AppEnvironment.prod => AppConfig._(
          environment: env,
          firebaseProjectId: 'geriatriccare-prod',
          apiBaseUrl: 'https://us-central1-geriatriccare-prod.cloudfunctions.net/api/v1',
          enableCrashlytics: true,
          enableAnalytics: true,
          enablePerformanceMonitoring: true,
        ),
    };
  }
  const AppConfig._({...});
}
```

### 17.2.2 Entry Points theo Flavor

```dart
// lib/main_dev.dart
void main() async {
  AppConfig.initialize(AppEnvironment.dev);
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform, // từ google-services-dev.json
  );
  runApp(const ProviderScope(child: App()));
}

// lib/main_staging.dart
void main() async {
  AppConfig.initialize(AppEnvironment.staging);
  ...
}

// lib/main_prod.dart
void main() async {
  AppConfig.initialize(AppEnvironment.prod);
  ...
}
```

### 17.2.3 Build Commands

```bash
# Development
flutter run --flavor dev -t lib/main_dev.dart

# Staging
flutter run --flavor staging -t lib/main_staging.dart

# Production release
flutter build apk --flavor prod -t lib/main_prod.dart --release
flutter build appbundle --flavor prod -t lib/main_prod.dart --release
```

---

## 17.3 Android Build Configuration

```groovy
// android/app/build.gradle
android {
    flavorDimensions "environment"
    productFlavors {
        dev {
            dimension "environment"
            applicationIdSuffix ".dev"
            versionNameSuffix "-dev"
            resValue "string", "app_name", "GeriatricCare Dev"
        }
        staging {
            dimension "environment"
            applicationIdSuffix ".staging"
            versionNameSuffix "-staging"
            resValue "string", "app_name", "GeriatricCare Staging"
        }
        prod {
            dimension "environment"
            resValue "string", "app_name", "GeriatricCare"
        }
    }

    buildTypes {
        release {
            minifyEnabled true
            shrinkResources true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'),
                          'proguard-rules.pro'
            signingConfig signingConfigs.release
        }
        debug {
            applicationIdSuffix ".debug"
            debuggable true
        }
    }

    signingConfigs {
        release {
            storeFile file(KEYSTORE_PATH)
            storePassword KEYSTORE_PASSWORD
            keyAlias KEY_ALIAS
            keyPassword KEY_PASSWORD
        }
    }
}
```

---

## 17.4 CI/CD Pipeline

### 17.4.1 GitHub Actions Workflow

```yaml
# .github/workflows/ci.yml
name: GeriatricCare CI/CD

on:
  push:
    branches: [main, develop, staging]
  pull_request:
    branches: [main, develop]

env:
  FLUTTER_VERSION: '3.19.0'

jobs:
  # ──────── Job 1: Lint & Test ────────
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Flutter
        uses: subosito/flutter-action@v2
        with:
          flutter-version: ${{ env.FLUTTER_VERSION }}

      - name: Get dependencies
        run: flutter pub get

      - name: Run code generation
        run: flutter pub run build_runner build --delete-conflicting-outputs

      - name: Analyze
        run: flutter analyze

      - name: Run tests
        run: flutter test --coverage

      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          file: coverage/lcov.info

  # ──────── Job 2: Build Staging APK ────────
  build-staging:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/staging'
    steps:
      - uses: actions/checkout@v4

      - name: Setup Flutter
        uses: subosito/flutter-action@v2
        with:
          flutter-version: ${{ env.FLUTTER_VERSION }}

      - name: Decode keystore
        run: |
          echo "${{ secrets.KEYSTORE_BASE64 }}" | base64 --decode > android/app/keystore.jks

      - name: Setup signing
        run: |
          echo "KEYSTORE_PATH=keystore.jks" >> local.properties
          echo "KEYSTORE_PASSWORD=${{ secrets.KEYSTORE_PASSWORD }}" >> local.properties
          echo "KEY_ALIAS=${{ secrets.KEY_ALIAS }}" >> local.properties
          echo "KEY_PASSWORD=${{ secrets.KEY_PASSWORD }}" >> local.properties

      - name: Build Staging APK
        run: |
          flutter build apk \
            --flavor staging \
            -t lib/main_staging.dart \
            --release

      - name: Upload to Firebase App Distribution
        uses: wzieba/Firebase-Distribution-Github-Action@v1
        with:
          appId: ${{ secrets.FIREBASE_STAGING_APP_ID }}
          serviceCredentialsFileContent: ${{ secrets.FIREBASE_SERVICE_ACCOUNT }}
          groups: testers
          file: build/app/outputs/flutter-apk/app-staging-release.apk
          releaseNotes: "Build from commit ${{ github.sha }}"

  # ──────── Job 3: Deploy Production ────────
  deploy-prod:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment: production     # Requires manual approval
    steps:
      - uses: actions/checkout@v4

      - name: Setup Flutter
        uses: subosito/flutter-action@v2

      - name: Build Production AAB
        run: |
          flutter build appbundle \
            --flavor prod \
            -t lib/main_prod.dart \
            --release \
            --obfuscate \
            --split-debug-info=build/debug-info

      - name: Upload to Play Store (Internal Track)
        uses: r0adkll/upload-google-play@v1
        with:
          serviceAccountJsonPlainText: ${{ secrets.PLAY_STORE_SERVICE_ACCOUNT }}
          packageName: com.geriatriccare.app
          releaseFiles: build/app/outputs/bundle/prodRelease/app-prod-release.aab
          track: internal
          status: completed
```

---

## 17.5 Firebase Infrastructure

### 17.5.1 Firestore Capacity Planning

| Metric | Free (Spark) | Paid (Blaze) – Dự kiến |
|---|---|---|
| Reads/ngày | 50,000 | ~500,000 (1,000 users) |
| Writes/ngày | 20,000 | ~200,000 |
| Deletes/ngày | 20,000 | ~50,000 |
| Storage | 1 GB | ~10 GB |
| Bandwidth | 10 GB/tháng | ~100 GB |

**Chi phí ước tính (1,000 MAU):**
- Firestore: ~$15–$30/tháng
- Firebase Storage: ~$5/tháng
- FCM: Miễn phí
- Auth: Miễn phí (< 10K MAU/tháng)
- Cloud Functions: ~$5/tháng
- **Tổng: ~$25–$40/tháng**

### 17.5.2 Firebase Cloud Functions – Regions

```javascript
// functions/index.js
const functions = require('firebase-functions');
const admin = require('firebase-admin');
admin.initializeApp();

// Deploy đến region asia-southeast1 (Singapore) – gần Việt Nam nhất
exports.triggerSos = functions
  .region('asia-southeast1')
  .https.onCall(async (data, context) => {
    if (!context.auth) throw new functions.https.HttpsError('unauthenticated');
    // ... SOS logic
  });

exports.onHealthRecordCreated = functions
  .region('asia-southeast1')
  .firestore
  .document('elderly_profiles/{elderlyId}/health_records/{recordId}')
  .onCreate(async (snap, context) => {
    // ... Health alert logic
  });
```

### 17.5.3 Firebase Security Rules Deployment

```bash
# Deploy Security Rules
firebase deploy --only firestore:rules --project geriatriccare-prod

# Deploy Indexes
firebase deploy --only firestore:indexes --project geriatriccare-prod

# Deploy Functions
firebase deploy --only functions --project geriatriccare-prod

# Deploy tất cả
firebase deploy --project geriatriccare-prod
```

---

## 17.6 Monitoring & Alerting

### 17.6.1 Firebase Crashlytics

```dart
// Cấu hình crash reporting
void configureCrashlytics() {
  FlutterError.onError = FirebaseCrashlytics.instance.recordFlutterFatalError;
  PlatformDispatcher.instance.onError = (error, stack) {
    FirebaseCrashlytics.instance.recordError(error, stack, fatal: true);
    return true;
  };
}

// Custom keys để debug dễ hơn
FirebaseCrashlytics.instance.setCustomKey('user_role', user.role.name);
FirebaseCrashlytics.instance.setCustomKey('app_env', AppConfig.instance.environment.name);
```

### 17.6.2 Firebase Performance Monitoring

```dart
// Theo dõi thời gian gửi SOS
final trace = FirebasePerformance.instance.newTrace('sos_trigger');
await trace.start();
final result = await sosRepository.triggerSOS(params);
await trace.stop();
trace.putAttribute('success', result.isRight().toString());
```

### 17.6.3 Firebase Remote Config

```dart
// Feature flags cho rollout kiểm soát
class FeatureFlags {
  static bool get isShakeDetectionEnabled =>
      FirebaseRemoteConfig.instance.getBool('shake_detection_enabled');

  static double get shakeThreshold =>
      FirebaseRemoteConfig.instance.getDouble('shake_threshold');

  static bool get isPdfReportEnabled =>
      FirebaseRemoteConfig.instance.getBool('pdf_report_enabled');

  static bool get isDoctorDashboardEnabled =>
      FirebaseRemoteConfig.instance.getBool('doctor_dashboard_enabled');
}
```

---

## 17.7 Play Store Deployment Checklist

| Hạng mục | Yêu cầu | Status |
|---|---|---|
| App Signing | Keystore bảo mật, không commit vào git | ⬜ |
| Privacy Policy | URL công khai, nội dung đầy đủ | ⬜ |
| App Icon | 512×512 PNG, adaptive icon | ⬜ |
| Screenshots | Phone + Tablet, VI + EN | ⬜ |
| Short description | ≤ 80 ký tự | ⬜ |
| Full description | ≤ 4000 ký tự | ⬜ |
| Content rating | Medical: Health & Fitness | ⬜ |
| Data safety | Khai báo đầy đủ dữ liệu thu thập | ⬜ |
| Target SDK | Android 14 (API 34) | ⬜ |
| Min SDK | Android 8.0 (API 26) | ⬜ |
| Release track | Internal → Closed Testing → Open Testing → Production | ⬜ |

---

## 17.8 Release Management

### Versioning Strategy

```
Major.Minor.Patch+BuildNumber
v1.0.0+1   → MVP release
v1.1.0+10  → Feature update (Doctor Dashboard)
v1.1.1+11  → Bug fix
v2.0.0+50  → Major update (AI features)
```

### Release Notes Template (Tiếng Việt)

```
Phiên bản 1.0.0 – Tháng 7/2026

🆕 Tính năng mới:
• Nút SOS khẩn cấp với đếm ngược
• Nhắc uống thuốc bằng giọng nói tiếng Việt
• Theo dõi huyết áp, đường huyết, nhịp tim
• Bản đồ vị trí GPS thời gian thực

🐛 Sửa lỗi:
• Không có (phiên bản đầu tiên)

⚡ Cải thiện hiệu năng:
• App khởi động nhanh hơn 30%
```
