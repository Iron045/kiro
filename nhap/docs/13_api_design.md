# Chương 13: API Design
## GeriatricCare – Thiết kế Firebase API (Client-side SDK + Cloud Functions)

---

## 13.1 Tổng quan kiến trúc API

GeriatricCare không dùng REST backend truyền thống. API được chia thành 3 tầng:

| Tầng | Công nghệ | Dùng cho |
|---|---|---|
| **Direct Firestore** | Firebase SDK (Flutter) | CRUD thông thường (read/write documents) |
| **Cloud Functions (HTTPS)** | Node.js Firebase Functions | Logic nghiệp vụ phức tạp (SOS, notification, report) |
| **Cloud Functions (Trigger)** | Firestore/Auth triggers | Side effects tự động (send alert khi thêm health record) |

---

## 13.2 Quy ước API

### Base URL (Cloud Functions)
```
https://us-central1-geriatriccare.cloudfunctions.net/api/v1
```

### Authentication Header
```
Authorization: Bearer <Firebase ID Token>
```

### Response Format chuẩn

**Success:**
```json
{
  "success": true,
  "data": { ... },
  "message": "Operation successful",
  "timestamp": "2026-07-22T08:00:00Z"
}
```

**Error:**
```json
{
  "success": false,
  "error": {
    "code": "ERR_CODE",
    "message": "Human-readable message",
    "details": { ... }
  },
  "timestamp": "2026-07-22T08:00:00Z"
}
```

### HTTP Status Codes

| Code | Ý nghĩa |
|---|---|
| 200 | OK – Thành công |
| 201 | Created – Tạo mới thành công |
| 400 | Bad Request – Dữ liệu đầu vào không hợp lệ |
| 401 | Unauthorized – Chưa xác thực |
| 403 | Forbidden – Không có quyền |
| 404 | Not Found – Không tìm thấy |
| 409 | Conflict – Dữ liệu trùng |
| 422 | Unprocessable Entity – Validation fail |
| 429 | Too Many Requests – Rate limit |
| 500 | Internal Server Error |

---

## 13.3 Authentication APIs (Firebase Auth SDK)

### API-AUTH-01: Đăng ký

```
POST /auth/register
```

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "SecureP@ss1",
  "fullName": "Nguyễn Văn A",
  "role": "FAMILY",
  "phoneNumber": "+84901234567"
}
```

**Response 201:**
```json
{
  "success": true,
  "data": {
    "userId": "uid_abc123",
    "email": "user@example.com",
    "fullName": "Nguyễn Văn A",
    "role": "FAMILY",
    "idToken": "eyJhbGci...",
    "refreshToken": "AMf-vBx...",
    "expiresIn": "3600"
  }
}
```

**Error Codes:**

| Code | HTTP | Mô tả |
|---|---|---|
| `AUTH_EMAIL_EXISTS` | 409 | Email đã tồn tại |
| `AUTH_WEAK_PASSWORD` | 422 | Mật khẩu không đủ mạnh |
| `AUTH_INVALID_EMAIL` | 400 | Email không hợp lệ |

---

### API-AUTH-02: Đăng nhập

```
POST /auth/login
```

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "SecureP@ss1",
  "fcmToken": "device_fcm_token_here",
  "deviceInfo": {
    "os": "Android 13",
    "model": "Samsung A54",
    "appVersion": "1.2.0"
  }
}
```

**Response 200:**
```json
{
  "success": true,
  "data": {
    "userId": "uid_abc123",
    "role": "FAMILY",
    "idToken": "eyJhbGci...",
    "refreshToken": "AMf-vBx...",
    "expiresIn": "3600"
  }
}
```

**Error Codes:**

| Code | HTTP | Mô tả |
|---|---|---|
| `AUTH_INVALID_CREDENTIALS` | 401 | Sai email hoặc mật khẩu |
| `AUTH_ACCOUNT_LOCKED` | 403 | Tài khoản bị khóa (quá 5 lần sai) |
| `AUTH_ACCOUNT_DISABLED` | 403 | Tài khoản bị vô hiệu hóa |

---

### API-AUTH-03: Refresh Token

```
POST /auth/refresh
```

**Request Body:**
```json
{ "refreshToken": "AMf-vBx..." }
```

**Response 200:**
```json
{
  "success": true,
  "data": {
    "idToken": "eyJhbGci...",
    "expiresIn": "3600"
  }
}
```

---

## 13.4 SOS APIs (Cloud Functions)

### API-SOS-01: Kích hoạt SOS

```
POST /sos/trigger
Authorization: Bearer <idToken>
```

**Request Body:**
```json
{
  "elderlyId": "ep_xyz789",
  "triggerType": "BUTTON",
  "latitude": 10.7769,
  "longitude": 106.7009,
  "accuracy": 12.5
}
```

**Response 201:**
```json
{
  "success": true,
  "data": {
    "sosId": "sos_20260722_001",
    "status": "SENT",
    "notifiedCount": 3,
    "notifiedUsers": [
      { "userId": "uid_abc123", "name": "Nguyễn Văn A" }
    ],
    "timestamp": "2026-07-22T08:04:02Z"
  }
}
```

**Server-side Logic (Cloud Function):**
```
1. Verify Auth token
2. Check role = ELDERLY
3. Create SOS document in Firestore
4. Get all active family_links for elderlyId
5. Get FCM tokens for each family user
6. Send FCM multicast (HIGH priority)
7. Send SMS to emergency contact
8. Update SOS status = SENT
9. Log activity
10. Return response
```

**Error Codes:**

| Code | HTTP | Mô tả |
|---|---|---|
| `SOS_ELDERLY_NOT_FOUND` | 404 | Không tìm thấy hồ sơ người cao tuổi |
| `SOS_NO_CONTACTS` | 422 | Không có người thân nào để thông báo |
| `SOS_RATE_LIMITED` | 429 | Gửi SOS quá thường (> 1 trong 30 giây) |

---

### API-SOS-02: Hủy SOS

```
PATCH /sos/{sosId}/cancel
Authorization: Bearer <idToken>
```

**Request Body:**
```json
{ "elderlyId": "ep_xyz789" }
```

**Response 200:**
```json
{
  "success": true,
  "data": {
    "sosId": "sos_20260722_001",
    "status": "CANCELLED",
    "cancelledAt": "2026-07-22T08:03:58Z"
  }
}
```

---

### API-SOS-03: Xác nhận SOS (Người thân)

```
PATCH /sos/{sosId}/acknowledge
Authorization: Bearer <idToken>
```

**Request Body:**
```json
{ "elderlyId": "ep_xyz789" }
```

**Response 200:**
```json
{
  "success": true,
  "data": {
    "sosId": "sos_20260722_001",
    "status": "ACKNOWLEDGED",
    "acknowledgedBy": "uid_abc123",
    "acknowledgedAt": "2026-07-22T08:05:30Z"
  }
}
```

---

### API-SOS-04: Lịch sử SOS

```
GET /sos/history?elderlyId={elderlyId}&limit=20&startAfter={lastDocId}
Authorization: Bearer <idToken>
```

**Response 200:**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "sosId": "sos_20260722_001",
        "triggerType": "BUTTON",
        "latitude": 10.7769,
        "longitude": 106.7009,
        "status": "ACKNOWLEDGED",
        "triggeredAt": "2026-07-22T08:04:00Z"
      }
    ],
    "pagination": {
      "lastDocId": "sos_20260722_001",
      "hasMore": false
    }
  }
}
```

---

## 13.5 Health APIs

### API-HLT-01: Lưu chỉ số sức khỏe

```
POST /health/records
Authorization: Bearer <idToken>
```

**Request Body:**
```json
{
  "elderlyId": "ep_xyz789",
  "type": "BLOOD_PRESSURE",
  "values": {
    "sys": 148,
    "dia": 92,
    "pulse": 78
  },
  "measuredAt": "2026-07-22T07:30:00Z",
  "note": "Sau khi thức dậy"
}
```

**Response 201:**
```json
{
  "success": true,
  "data": {
    "recordId": "hr_bp_001",
    "isAbnormal": true,
    "alertSent": true,
    "alertMessage": "Huyết áp cao: 148/92 mmHg",
    "thresholds": {
      "sys": { "min": 90, "max": 140 },
      "dia": { "min": 60, "max": 90 }
    }
  }
}
```

**Validation Rules:**

| Field | Rule |
|---|---|
| `type` | Phải là enum hợp lệ |
| `values` | Keys phải khớp với type |
| `measuredAt` | Không được là tương lai (> now + 5 phút) |

---

### API-HLT-02: Lấy lịch sử sức khỏe

```
GET /health/records?elderlyId={id}&type={type}&from={ISO}&to={ISO}&limit=50
Authorization: Bearer <idToken>
```

**Response 200:**
```json
{
  "success": true,
  "data": {
    "elderlyId": "ep_xyz789",
    "type": "BLOOD_PRESSURE",
    "records": [
      {
        "recordId": "hr_bp_001",
        "values": { "sys": 148, "dia": 92, "pulse": 78 },
        "measuredAt": "2026-07-22T07:30:00Z",
        "isAbnormal": true,
        "note": "Sau khi thức dậy"
      }
    ],
    "summary": {
      "count": 1,
      "avgSys": 148,
      "avgDia": 92,
      "abnormalCount": 1
    }
  }
}
```

---

### API-HLT-03: Lấy chỉ số mới nhất

```
GET /health/latest?elderlyId={id}
Authorization: Bearer <idToken>
```

**Response 200:**
```json
{
  "success": true,
  "data": {
    "BLOOD_PRESSURE": {
      "values": { "sys": 148, "dia": 92, "pulse": 78 },
      "measuredAt": "2026-07-22T07:30:00Z",
      "isAbnormal": true
    },
    "GLUCOSE": {
      "values": { "value": 6.2, "measurementType": "fasting" },
      "measuredAt": "2026-07-22T06:45:00Z",
      "isAbnormal": false
    }
  }
}
```

---

## 13.6 Medication APIs

### API-MED-01: Tạo lịch uống thuốc

```
POST /medications/schedules
Authorization: Bearer <idToken>
```

**Request Body:**
```json
{
  "elderlyId": "ep_xyz789",
  "medicationName": "Amlodipine 5mg",
  "dosage": "1 viên",
  "timesPerDay": 1,
  "scheduledTimes": ["07:00"],
  "startDate": "2026-07-22",
  "endDate": null,
  "note": "Uống sau khi ăn sáng",
  "color": "#4CAF50"
}
```

**Response 201:**
```json
{
  "success": true,
  "data": {
    "scheduleId": "ms_001",
    "nextReminder": "2026-07-23T07:00:00Z",
    "totalScheduledPerDay": 1
  }
}
```

---

### API-MED-02: Xác nhận uống thuốc

```
POST /medications/logs
Authorization: Bearer <idToken>
```

**Request Body:**
```json
{
  "scheduleId": "ms_001",
  "elderlyId": "ep_xyz789",
  "scheduledTime": "2026-07-22T07:00:00Z",
  "status": "TAKEN"
}
```

**Response 201:**
```json
{
  "success": true,
  "data": {
    "logId": "ml_20260722_001",
    "status": "TAKEN",
    "complianceRateToday": 100.0,
    "complianceRateWeek": 95.2
  }
}
```

---

### API-MED-03: Tỷ lệ tuân thủ

```
GET /medications/compliance?elderlyId={id}&period=WEEK
Authorization: Bearer <idToken>
```

**Response 200:**
```json
{
  "success": true,
  "data": {
    "elderlyId": "ep_xyz789",
    "period": "WEEK",
    "from": "2026-07-16",
    "to": "2026-07-22",
    "overall": 92.3,
    "byMedication": [
      {
        "scheduleId": "ms_001",
        "medicationName": "Amlodipine 5mg",
        "scheduled": 7,
        "taken": 7,
        "skipped": 0,
        "missed": 0,
        "rate": 100.0
      }
    ],
    "byDay": [
      { "date": "2026-07-22", "rate": 100.0 },
      { "date": "2026-07-21", "rate": 85.7 }
    ]
  }
}
```

---

## 13.7 Report API

### API-RPT-01: Tạo báo cáo PDF

```
POST /reports/generate
Authorization: Bearer <idToken>
```

**Request Body:**
```json
{
  "elderlyId": "ep_xyz789",
  "reportType": "HEALTH_SUMMARY",
  "period": "MONTH",
  "from": "2026-07-01",
  "to": "2026-07-22",
  "includeCharts": true,
  "includeMedication": true,
  "includeSOS": true
}
```

**Response 200:**
```json
{
  "success": true,
  "data": {
    "reportId": "rpt_001",
    "downloadUrl": "https://storage.googleapis.com/.../reports/rpt_001.pdf",
    "expiresAt": "2026-07-22T20:00:00Z",
    "pages": 8,
    "generatedAt": "2026-07-22T08:10:00Z"
  }
}
```

---

## 13.8 Cloud Function Triggers (Auto)

### TRIGGER-01: Gửi Health Alert khi nhập chỉ số bất thường

```javascript
// Kích hoạt khi document health_record được tạo
exports.onHealthRecordCreated = functions.firestore
  .document('elderly_profiles/{elderlyId}/health_records/{recordId}')
  .onCreate(async (snap, context) => {
    const record = snap.data();
    if (!record.isAbnormal) return;

    // Get family links and doctor links
    // Send FCM HIGH priority to all authorized users
    // Log alert event
  });
```

### TRIGGER-02: Gửi cảnh báo bỏ lỡ thuốc

```javascript
// Cloud Scheduler chạy mỗi phút
// Kiểm tra medication_logs có status PENDING và scheduledTime < now - 30min
// Nếu tìm thấy → update status = MISSED, gửi FCM đến người thân
```

### TRIGGER-03: Cleanup Location History (TTL)

```javascript
// Cloud Scheduler chạy hàng ngày 2:00 AM
// Xóa location_history records có recordedAt < now - 30 days
```

---

## 13.9 Bảng tổng hợp tất cả API endpoints

| ID | Method | Endpoint | Auth | Role |
|---|---|---|---|---|
| API-AUTH-01 | POST | `/auth/register` | ❌ | All |
| API-AUTH-02 | POST | `/auth/login` | ❌ | All |
| API-AUTH-03 | POST | `/auth/refresh` | ❌ | All |
| API-SOS-01 | POST | `/sos/trigger` | ✅ | ELDERLY |
| API-SOS-02 | PATCH | `/sos/{id}/cancel` | ✅ | ELDERLY |
| API-SOS-03 | PATCH | `/sos/{id}/acknowledge` | ✅ | FAMILY |
| API-SOS-04 | GET | `/sos/history` | ✅ | FAMILY, ELDERLY |
| API-HLT-01 | POST | `/health/records` | ✅ | ELDERLY, FAMILY |
| API-HLT-02 | GET | `/health/records` | ✅ | FAMILY, DOCTOR |
| API-HLT-03 | GET | `/health/latest` | ✅ | FAMILY, DOCTOR |
| API-MED-01 | POST | `/medications/schedules` | ✅ | FAMILY, DOCTOR |
| API-MED-02 | POST | `/medications/logs` | ✅ | ELDERLY |
| API-MED-03 | GET | `/medications/compliance` | ✅ | FAMILY, DOCTOR |
| API-RPT-01 | POST | `/reports/generate` | ✅ | FAMILY, DOCTOR |
| API-LINK-01 | POST | `/links/family` | ✅ | FAMILY |
| API-LINK-02 | PATCH | `/links/family/{id}/approve` | ✅ | ELDERLY |
| API-LINK-03 | POST | `/links/doctor` | ✅ | DOCTOR |
| API-LINK-04 | PATCH | `/links/doctor/{id}/approve` | ✅ | FAMILY |
| API-PROF-01 | GET | `/profiles/{elderlyId}` | ✅ | All |
| API-PROF-02 | PUT | `/profiles/{elderlyId}` | ✅ | ELDERLY, FAMILY |
| API-ADM-01 | GET | `/admin/users` | ✅ | ADMIN |
| API-ADM-02 | PATCH | `/admin/users/{id}/disable` | ✅ | ADMIN |
