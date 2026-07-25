# Chương 11: Database Design
## GeriatricCare – Thiết kế Cơ sở dữ liệu

---

## 11.1 Tổng quan

**Database Engine:** Cloud Firestore (NoSQL Document Database)

**Lý do chọn Firestore:**
- Realtime listeners cho SOS và health monitoring
- Offline-first với automatic sync
- Scalability tự động
- Security Rules mạnh mẽ
- Tích hợp tốt với Firebase ecosystem
- Pay-as-you-go phù hợp startup

**Backup Strategy:**
- Firebase tự động backup hàng ngày
- Export định kỳ sang Cloud Storage
- Point-in-time recovery 7 ngày

---

## 11.2 Chiến lược Denormalization

Firestore là NoSQL nên áp dụng **denormalization có kiểm soát** để tối ưu reads:

| Nguyên tắc | Lý do |
|---|---|
| Embed dữ liệu ít thay đổi | Giảm số lần read |
| Lưu redundant metadata | Dashboard load nhanh hơn |
| Trade-off: Write phức tạp hơn | Acceptable vì read >> write |

---

## 11.3 Entity-Relationship Overview

```
users (1) ──────────────── (1) elderly_profiles
                                      │
                        ┌─────────────┼──────────────┐
                        │             │              │
               health_records   sos_events   medication_schedules
                        │                            │
                 (per metric)               medication_logs
                        │
               health_thresholds (per user)

users (1) ──── (*) family_links ──── (1) elderly_profiles
users (1) ──── (*) doctor_links ──── (1) elderly_profiles

doctor_links ──── doctor_notes

users (1) ──────────────────── (1) notification_settings
elderly_profiles (1) ──── (*) location_history
elderly_profiles (1) ──── (*) activity_logs
```

---

## 11.4 Data Dictionary – Tất cả Collections

### Collection: `users`

| Field | Type | Required | Mô tả |
|---|---|---|---|
| `id` | string | ✅ | Firebase Auth UID |
| `email` | string | ✅ | Email đăng nhập |
| `fullName` | string | ✅ | Họ tên đầy đủ |
| `role` | enum | ✅ | `ELDERLY` / `FAMILY` / `DOCTOR` / `ADMIN` |
| `photoUrl` | string | ❌ | URL ảnh đại diện (Firebase Storage) |
| `phoneNumber` | string | ❌ | Số điện thoại |
| `fcmToken` | string | ❌ | Token FCM hiện tại |
| `fcmTokens` | array\<string\> | ❌ | Tất cả tokens (đa thiết bị) |
| `isActive` | boolean | ✅ | Tài khoản hoạt động |
| `isEmailVerified` | boolean | ✅ | Đã xác thực email |
| `createdAt` | timestamp | ✅ | Thời điểm tạo tài khoản |
| `updatedAt` | timestamp | ✅ | Lần cập nhật cuối |
| `lastLoginAt` | timestamp | ❌ | Lần đăng nhập gần nhất |
| `deviceInfo` | map | ❌ | `{os, model, appVersion}` |

**Indexes:** `email` (unique), `role`, `isActive`

---

### Collection: `elderly_profiles`

| Field | Type | Required | Mô tả |
|---|---|---|---|
| `id` | string | ✅ | Auto ID |
| `userId` | string | ✅ | Ref → users.id |
| `fullName` | string | ✅ | Họ tên |
| `dateOfBirth` | timestamp | ✅ | Ngày sinh |
| `gender` | enum | ✅ | `MALE` / `FEMALE` / `OTHER` |
| `bloodType` | enum | ❌ | `A/B/AB/O` (+ NEG variants) |
| `height` | number | ❌ | cm |
| `weight` | number | ❌ | kg |
| `medicalConditions` | array\<string\> | ❌ | Danh sách bệnh nền |
| `drugAllergies` | array\<string\> | ❌ | Dị ứng thuốc |
| `address` | string | ❌ | Địa chỉ thường trú |
| `photoUrl` | string | ❌ | Firebase Storage URL |
| `emergencyContact` | map | ✅ | `{name, phone, relationship}` |
| `linkCode` | string | ❌ | Mã liên kết 6 chữ số |
| `linkCodeExpiresAt` | timestamp | ❌ | Hết hạn sau 24h |
| `createdAt` | timestamp | ✅ | |
| `updatedAt` | timestamp | ✅ | |

**Subcollections:** `health_records`, `sos_events`, `medication_schedules`, `location_history`, `activity_logs`

---

### Subcollection: `elderly_profiles/{elderlyId}/health_records`

| Field | Type | Required | Mô tả |
|---|---|---|---|
| `id` | string | ✅ | Auto ID |
| `elderlyId` | string | ✅ | Denormalized parent ID |
| `type` | enum | ✅ | `BLOOD_PRESSURE/GLUCOSE/HEART_RATE/TEMPERATURE/SPO2/WEIGHT` |
| `values` | map | ✅ | `{sys: 120, dia: 80, pulse: 72}` (theo type) |
| `measuredAt` | timestamp | ✅ | Thời điểm đo (gốc, không phải sync time) |
| `note` | string | ❌ | Ghi chú |
| `isAbnormal` | boolean | ✅ | Tính toán tự động |
| `pendingSync` | boolean | ✅ | `true` khi lưu offline |
| `createdBy` | string | ✅ | userId người nhập |
| `syncedAt` | timestamp | ❌ | Thời điểm sync lên cloud |

**Values map theo type:**

| Type | Keys trong `values` |
|---|---|
| BLOOD_PRESSURE | `sys`, `dia`, `pulse` |
| GLUCOSE | `value`, `measurementType` (`fasting`/`postprandial`) |
| HEART_RATE | `bpm` |
| TEMPERATURE | `celsius` |
| SPO2 | `percent` |
| WEIGHT | `kg` |

**Composite Indexes:** `(elderlyId, type, measuredAt DESC)`, `(elderlyId, isAbnormal, measuredAt DESC)`

---

### Subcollection: `elderly_profiles/{elderlyId}/sos_events`

| Field | Type | Required | Mô tả |
|---|---|---|---|
| `id` | string | ✅ | Auto ID |
| `elderlyId` | string | ✅ | |
| `triggerType` | enum | ✅ | `BUTTON` / `SHAKE` |
| `latitude` | number | ❌ | GPS lat |
| `longitude` | number | ❌ | GPS lng |
| `locationAccuracy` | number | ❌ | Meters |
| `status` | enum | ✅ | `ACTIVE/SENT/CANCELLED/ACKNOWLEDGED/FAILED` |
| `notifiedUserIds` | array\<string\> | ❌ | Danh sách user đã nhận |
| `acknowledgedBy` | string | ❌ | userId người xác nhận |
| `acknowledgedAt` | timestamp | ❌ | |
| `cancelledBy` | string | ❌ | userId hủy |
| `cancelledAt` | timestamp | ❌ | |
| `triggeredAt` | timestamp | ✅ | Thời điểm kích hoạt |
| `sentAt` | timestamp | ❌ | Thời điểm gửi thành công |

**Indexes:** `(elderlyId, triggeredAt DESC)`, `(elderlyId, status)`

---

### Subcollection: `elderly_profiles/{elderlyId}/medication_schedules`

| Field | Type | Required | Mô tả |
|---|---|---|---|
| `id` | string | ✅ | Auto ID |
| `elderlyId` | string | ✅ | |
| `createdBy` | string | ✅ | userId (FAMILY/DOCTOR) |
| `medicationName` | string | ✅ | Tên thuốc |
| `dosage` | string | ✅ | Liều lượng |
| `timesPerDay` | number | ✅ | Số lần/ngày |
| `scheduledTimes` | array\<string\> | ✅ | `["07:00", "12:00", "19:00"]` |
| `startDate` | timestamp | ✅ | |
| `endDate` | timestamp | ❌ | null = vô thời hạn |
| `note` | string | ❌ | |
| `photoUrl` | string | ❌ | Ảnh viên thuốc |
| `color` | string | ❌ | Hex color |
| `isActive` | boolean | ✅ | Bật/tắt lịch nhắc |
| `createdAt` | timestamp | ✅ | |
| `updatedAt` | timestamp | ✅ | |

**Sub-subcollection:** `medication_logs`

---

### Sub-subcollection: `.../medication_schedules/{scheduleId}/medication_logs`

| Field | Type | Required | Mô tả |
|---|---|---|---|
| `id` | string | ✅ | |
| `scheduleId` | string | ✅ | |
| `elderlyId` | string | ✅ | Denormalized |
| `scheduledTime` | timestamp | ✅ | Giờ dự kiến uống |
| `status` | enum | ✅ | `PENDING/TAKEN/SKIPPED/MISSED` |
| `respondedAt` | timestamp | ❌ | Khi người dùng phản hồi |
| `familyAlertedAt` | timestamp | ❌ | Khi gửi alert cho người thân |

---

### Collection: `family_links`

| Field | Type | Required | Mô tả |
|---|---|---|---|
| `id` | string | ✅ | Auto ID |
| `familyUserId` | string | ✅ | Ref → users.id (người thân) |
| `elderlyProfileId` | string | ✅ | Ref → elderly_profiles.id |
| `elderlyUserId` | string | ✅ | Denormalized |
| `relationship` | string | ❌ | "con", "cháu"... |
| `status` | enum | ✅ | `PENDING/ACTIVE/REJECTED/REMOVED` |
| `permissions` | map | ✅ | `{viewHealth, viewLocation, manageMeds}` |
| `createdAt` | timestamp | ✅ | |
| `approvedAt` | timestamp | ❌ | |

**Indexes:** `(familyUserId, status)`, `(elderlyProfileId, status)`, `(familyUserId, elderlyProfileId)` UNIQUE

---

### Collection: `doctor_links`

| Field | Type | Required | Mô tả |
|---|---|---|---|
| `id` | string | ✅ | Auto ID |
| `doctorUserId` | string | ✅ | |
| `elderlyProfileId` | string | ✅ | |
| `elderlyUserId` | string | ✅ | Denormalized |
| `status` | enum | ✅ | `PENDING/ACTIVE/REMOVED` |
| `approvedBy` | string | ❌ | Family userId |
| `createdAt` | timestamp | ✅ | |
| `approvedAt` | timestamp | ❌ | |

**Sub-subcollection:** `doctor_notes`

---

### Collection: `notification_settings`

| Field | Type | Required | Mô tả |
|---|---|---|---|
| `id` | string | ✅ | = userId |
| `userId` | string | ✅ | |
| `sosAlerts` | boolean | ✅ | Luôn true, không tắt được |
| `healthAlerts` | boolean | ✅ | Default true |
| `medicationReminders` | boolean | ✅ | Default true |
| `medicationMissed` | boolean | ✅ | Default true |
| `batteryLow` | boolean | ✅ | Default true |
| `dailySummary` | boolean | ✅ | Default false |
| `quietHoursStart` | string | ❌ | "22:00" |
| `quietHoursEnd` | string | ❌ | "07:00" |
| `updatedAt` | timestamp | ✅ | |

---

### Subcollection: `elderly_profiles/{elderlyId}/location_history`

| Field | Type | Required | Mô tả |
|---|---|---|---|
| `id` | string | ✅ | Auto ID |
| `elderlyId` | string | ✅ | |
| `latitude` | number | ✅ | |
| `longitude` | number | ✅ | |
| `accuracy` | number | ✅ | Meters |
| `batteryLevel` | number | ❌ | % pin |
| `recordedAt` | timestamp | ✅ | |

**TTL:** 30 ngày (xóa tự động qua Cloud Function)
**Indexes:** `(elderlyId, recordedAt DESC)`

---

### Subcollection: `elderly_profiles/{elderlyId}/activity_logs`

| Field | Type | Required | Mô tả |
|---|---|---|---|
| `id` | string | ✅ | Auto ID |
| `elderlyId` | string | ✅ | |
| `actorId` | string | ✅ | userId thực hiện |
| `eventType` | enum | ✅ | `LOGIN/LOGOUT/SOS_TRIGGERED/MED_TAKEN/MED_MISSED/HEALTH_RECORDED/PROFILE_UPDATED/REPORT_EXPORTED` |
| `metadata` | map | ❌ | Dữ liệu thêm theo eventType |
| `createdAt` | timestamp | ✅ | |

**TTL:** 12 tháng
**Indexes:** `(elderlyId, eventType, createdAt DESC)`

---

## 11.5 Health Thresholds – Ngưỡng cảnh báo mặc định

```
Collection: health_thresholds
Document ID: default (global)
Document ID: {elderlyId} (custom per patient)
```

| Field | Type | Mô tả |
|---|---|---|
| `elderlyId` | string | null = global default |
| `BLOOD_PRESSURE_SYS_MIN` | number | 90 |
| `BLOOD_PRESSURE_SYS_MAX` | number | 140 |
| `BLOOD_PRESSURE_DIA_MIN` | number | 60 |
| `BLOOD_PRESSURE_DIA_MAX` | number | 90 |
| `HEART_RATE_MIN` | number | 60 |
| `HEART_RATE_MAX` | number | 100 |
| `GLUCOSE_MIN` | number | 3.9 |
| `GLUCOSE_MAX` | number | 7.0 |
| `TEMPERATURE_MIN` | number | 35.5 |
| `TEMPERATURE_MAX` | number | 38.0 |
| `SPO2_MIN` | number | 93 |
| `WEIGHT_BMI_MIN` | number | 18.5 |
| `WEIGHT_BMI_MAX` | number | 30.0 |
| `updatedBy` | string | doctorId hoặc null |
| `updatedAt` | timestamp | |

---

## 11.6 Conflict Resolution Strategy

| Tình huống | Chiến lược |
|---|---|
| Offline record sync | Last-Write-Wins dựa trên `measuredAt` (thời gian đo gốc) |
| Concurrent medication log | Server timestamp làm tiebreaker |
| Profile update conflict | Last-Write-Wins với `updatedAt` |
| SOS duplicate | Debounce 10 giây ở client, server check unique trong 5 phút |
