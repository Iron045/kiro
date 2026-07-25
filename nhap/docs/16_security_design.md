# Chương 16: Security Design
## GeriatricCare – Thiết kế Bảo mật

---

## 16.1 Mô hình bảo mật tổng quan

GeriatricCare xử lý dữ liệu y tế nhạy cảm. Áp dụng mô hình **Defense in Depth** – nhiều lớp bảo vệ.

```
┌──────────────────────────────────────────────────────────┐
│                    DEFENSE IN DEPTH                      │
│                                                          │
│  Layer 1: Device Security (Android Keystore, Root Check) │
│  Layer 2: Transport Security (TLS 1.3, Cert Pinning)     │
│  Layer 3: Authentication (Firebase Auth, 2FA)            │
│  Layer 4: Authorization (RBAC, Firebase Security Rules)  │
│  Layer 5: Data Security (AES-256, Field Encryption)      │
│  Layer 6: Audit & Monitoring (Audit Log, Crashlytics)    │
└──────────────────────────────────────────────────────────┘
```

**Tiêu chuẩn tham chiếu:**
- OWASP Mobile Security Testing Guide (MSTG)
- OWASP Top 10 Mobile Risks
- Firebase Security Best Practices
- NIST SP 800-63B (Digital Identity)

---

## 16.2 Authentication Security

### 16.2.1 Firebase Authentication Flow

```
User → Flutter App → Firebase Auth SDK → Google Identity Platform
                           │
                    Validate credentials
                           │
                    Issue: idToken (1h) + refreshToken (30d)
                           │
              Flutter stores in flutter_secure_storage (encrypted)
```

### 16.2.2 Token Management

```dart
// Lưu token an toàn – KHÔNG dùng SharedPreferences
class SecureTokenStorage {
  static const _storage = FlutterSecureStorage(
    aOptions: AndroidOptions(
      encryptedSharedPreferences: true,
      keyCipherAlgorithm: KeyCipherAlgorithm.RSA_ECB_OAEPwithSHA_256andMGF1Padding,
      storageCipherAlgorithm: StorageCipherAlgorithm.AES_GCM_NoPadding,
    ),
  );

  static Future<void> saveToken(String token) async {
    await _storage.write(key: 'id_token', value: token);
  }

  static Future<String?> getToken() async {
    return _storage.read(key: 'id_token');
  }

  static Future<void> deleteAll() async {
    await _storage.deleteAll();
  }
}
```

### 16.2.3 Brute Force Protection

| Cơ chế | Cấu hình |
|---|---|
| Lockout sau sai mật khẩu | 5 lần → khóa 15 phút |
| Progressive delay | 1s, 2s, 4s, 8s, 16s |
| CAPTCHA | Sau 3 lần sai (Firebase reCAPTCHA) |
| IP rate limiting | Firebase AppCheck + Cloud Armor |

### 16.2.4 Session Management

```dart
// Auto logout khi token hết hạn + idle
class SessionManager {
  static Timer? _idleTimer;
  static const _idleTimeout = Duration(days: 30);

  static void resetIdleTimer() {
    _idleTimer?.cancel();
    _idleTimer = Timer(_idleTimeout, () => _handleSessionExpiry());
  }

  static Future<void> _handleSessionExpiry() async {
    await SecureTokenStorage.deleteAll();
    // Navigate to login
    Get.offAllNamed(Routes.login);
  }
}
```

---

## 16.3 Authorization – RBAC Chi tiết

### 16.3.1 Permission Matrix

| Permission | ELDERLY | FAMILY | DOCTOR | ADMIN |
|---|---|---|---|---|
| Xem hồ sơ của mình | ✅ | ❌ | ❌ | ✅ |
| Xem hồ sơ người liên kết | ❌ | ✅ | ✅ | ✅ |
| Nhập chỉ số sức khỏe | ✅ | ✅ | ❌ | ❌ |
| Tạo lịch uống thuốc | ❌ | ✅ | ✅ | ❌ |
| Xác nhận uống thuốc | ✅ | ❌ | ❌ | ❌ |
| Kích hoạt SOS | ✅ | ❌ | ❌ | ❌ |
| Xem vị trí GPS | ✅ (of self) | ✅ | ❌ | ✅ |
| Ghi chú bác sĩ | ❌ | ❌ | ✅ | ❌ |
| Xuất báo cáo | ❌ | ✅ | ✅ | ✅ |
| Quản lý người dùng | ❌ | ❌ | ❌ | ✅ |
| Tắt notification SOS | ❌ | ❌ | ❌ | ❌ |

### 16.3.2 Kiểm tra quyền trong Flutter

```dart
// Middleware kiểm tra quyền trước khi thực hiện usecase
class PermissionChecker {
  final AuthRepository authRepo;

  Future<Either<Failure, Unit>> checkCanReadHealthData(
      String elderlyId) async {
    final user = authRepo.getCurrentUser();
    if (user == null) return Left(AuthFailure('Chưa đăng nhập'));

    return switch (user.role) {
      UserRole.elderly => user.elderlyProfileId == elderlyId
          ? Right(unit)
          : Left(PermissionFailure('Không có quyền truy cập')),
      UserRole.family => _checkFamilyLink(user.id, elderlyId),
      UserRole.doctor => _checkDoctorLink(user.id, elderlyId),
      UserRole.admin => Right(unit),
    };
  }
}
```

---

## 16.4 Data Encryption

### 16.4.1 Data at Rest

```dart
// Mã hóa dữ liệu nhạy cảm trong Hive
class EncryptedHiveSetup {
  static Future<void> init() async {
    final key = await _getOrGenerateKey();
    Hive.registerAdapter(HealthRecordModelAdapter());
    await Hive.openBox<HealthRecordModel>(
      'health_records',
      encryptionCipher: HiveAesCipher(key),
    );
  }

  static Future<Uint8List> _getOrGenerateKey() async {
    const storage = FlutterSecureStorage();
    final existing = await storage.read(key: 'hive_encryption_key');
    if (existing != null) {
      return base64Decode(existing);
    }
    final key = Hive.generateSecureKey();
    await storage.write(
      key: 'hive_encryption_key',
      value: base64Encode(key),
    );
    return Uint8List.fromList(key);
  }
}
```

### 16.4.2 Sensitive Fields trong Firestore

Các trường nhạy cảm được mã hóa thêm tầng application-level trước khi lưu Firestore:

| Field | Lý do mã hóa |
|---|---|
| `drugAllergies` | Thông tin y tế cực nhạy cảm |
| `medicalConditions` | Thông tin y tế |
| `emergencyContact.phone` | PII |
| `latitude/longitude` trong location_history | Location privacy |

```dart
class FieldEncryptionService {
  static String encrypt(String value, String userId) {
    // Dùng AES-256-GCM với key riêng per user
    final key = _deriveKeyFromUserId(userId);
    final encrypter = Encrypter(AES(key, mode: AESMode.gcm));
    final iv = IV.fromSecureRandom(12);
    final encrypted = encrypter.encrypt(value, iv: iv);
    return '${iv.base64}:${encrypted.base64}';
  }

  static String decrypt(String encryptedValue, String userId) {
    final parts = encryptedValue.split(':');
    final iv = IV.fromBase64(parts[0]);
    final key = _deriveKeyFromUserId(userId);
    final encrypter = Encrypter(AES(key, mode: AESMode.gcm));
    return encrypter.decrypt64(parts[1], iv: iv);
  }
}
```

### 16.4.3 Data in Transit

```dart
// Certificate Pinning trong Flutter
class HttpClientConfig {
  static HttpClient buildSecureClient() {
    final client = HttpClient();
    client.badCertificateCallback = (cert, host, port) => false;
    // Pin Firebase certificates
    client.connectionFactory = (uri, proxyHost, proxyPort) async {
      final socket = await SecureSocket.connect(
        uri.host, uri.port,
        onBadCertificate: (_) => false,
      );
      return socket;
    };
    return client;
  }
}
```

---

## 16.5 Android Security

### 16.5.1 Root Detection

```dart
class SecurityChecker {
  static Future<SecurityStatus> checkDeviceSecurity() async {
    final isRooted = await FlutterJailbreakDetection.jailbroken;
    final isDeveloperMode = await FlutterJailbreakDetection.developerMode;

    if (isRooted) {
      return SecurityStatus.rooted;
    }
    if (isDeveloperMode) {
      return SecurityStatus.developerMode;
    }
    return SecurityStatus.secure;
  }
}

// Trong main.dart
void main() async {
  final status = await SecurityChecker.checkDeviceSecurity();
  if (status == SecurityStatus.rooted) {
    // Hiển thị cảnh báo nhưng không block (UX cho người cao tuổi)
    // Chỉ log + disable sensitive features
  }
}
```

### 16.5.2 ProGuard Rules

```proguard
# android/app/proguard-rules.pro

# Flutter
-keep class io.flutter.** { *; }
-keep class io.flutter.plugins.** { *; }

# Firebase
-keep class com.google.firebase.** { *; }
-keep class com.google.android.gms.** { *; }

# Obfuscate business logic
-obfuscationdictionary dictionary.txt
-classobfuscationdictionary dictionary.txt

# Remove logging in release
-assumenosideeffects class android.util.Log {
    public static *** d(...);
    public static *** v(...);
    public static *** i(...);
}
```

### 16.5.3 Android Manifest Permissions

```xml
<!-- android/app/src/main/AndroidManifest.xml -->
<!-- REQUIRED permissions -->
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
<uses-permission android:name="android.permission.VIBRATE"/>
<uses-permission android:name="android.permission.WAKE_LOCK"/>
<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>
<uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>

<!-- Location -->
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION"/>

<!-- Sensors -->
<uses-permission android:name="android.permission.BODY_SENSORS"/>

<!-- Camera (for profile photo) -->
<uses-permission android:name="android.permission.CAMERA"/>
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES"/>
```

---

## 16.6 Audit Log System

```dart
// Mọi action nhạy cảm đều ghi Audit Log
class AuditLogger {
  final FirebaseFirestore _firestore;

  Future<void> log({
    required String elderlyId,
    required String actorId,
    required AuditEventType eventType,
    Map<String, dynamic>? metadata,
  }) async {
    await _firestore
        .collection('elderly_profiles')
        .doc(elderlyId)
        .collection('activity_logs')
        .add({
      'elderlyId': elderlyId,
      'actorId': actorId,
      'eventType': eventType.name,
      'metadata': metadata ?? {},
      'createdAt': FieldValue.serverTimestamp(),
      // IP không lưu (privacy), chỉ lưu device fingerprint
      'deviceId': await _getDeviceFingerprint(),
    });
  }
}
```

**Các sự kiện bắt buộc audit:**

| Event | Metadata ghi kèm |
|---|---|
| LOGIN | deviceId, appVersion |
| LOGOUT | reason (manual/timeout) |
| SOS_TRIGGERED | triggerType, hasGps |
| SOS_CANCELLED | timeToCancel |
| HEALTH_RECORD_ADDED | type, isAbnormal |
| MEDICATION_TAKEN | scheduleId, responseTime |
| MEDICATION_MISSED | scheduleId, alertSent |
| PROFILE_UPDATED | fieldsChanged (names only) |
| DOCTOR_LINK_CREATED | doctorId |
| REPORT_EXPORTED | reportType, period |
| ACCOUNT_DISABLED | disabledBy (admin) |

---

## 16.7 Privacy & PDPA Compliance

| Yêu cầu | Triển khai |
|---|---|
| Informed Consent | Màn hình consent rõ ràng khi đăng ký |
| Data Minimization | Chỉ thu thập dữ liệu cần thiết |
| Right to Access | Người dùng xem dữ liệu của mình |
| Right to Erasure | Xóa tài khoản → anonymize 30 ngày → xóa hẳn |
| Data Portability | Xuất dữ liệu dạng JSON/PDF |
| Breach Notification | Quy trình thông báo trong 72 giờ |
| Data Retention | Health data: 5 năm; Location: 30 ngày; Logs: 12 tháng |
