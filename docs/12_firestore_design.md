# Chương 12: Firestore Design
## GeriatricCare – Cấu trúc Firestore & Security Rules

---

## 12.1 Collection Hierarchy

```
firestore/
├── users/
│   └── {userId}/                       ← Document: User info
│
├── elderly_profiles/
│   └── {elderlyId}/                    ← Document: Profile
│       ├── health_records/
│       │   └── {recordId}/
│       ├── sos_events/
│       │   └── {sosId}/
│       ├── medication_schedules/
│       │   └── {scheduleId}/
│       │       └── medication_logs/
│       │           └── {logId}/
│       ├── location_history/
│       │   └── {locationId}/
│       └── activity_logs/
│           └── {logId}/
│
├── family_links/
│   └── {linkId}/
│
├── doctor_links/
│   └── {linkId}/
│       └── doctor_notes/
│           └── {noteId}/
│
├── health_thresholds/
│   └── {elderlyId}/                    ← Document: Custom thresholds
│   └── default/                        ← Document: Global defaults
│
└── notification_settings/
    └── {userId}/
```

---

## 12.2 Sample Documents

### User Document

```json
{
  "id": "uid_abc123",
  "email": "nguyen.van.a@gmail.com",
  "fullName": "Nguyễn Văn A",
  "role": "FAMILY",
  "phoneNumber": "+84901234567",
  "fcmTokens": ["token_device1", "token_device2"],
  "isActive": true,
  "isEmailVerified": true,
  "createdAt": "2026-01-15T08:00:00Z",
  "updatedAt": "2026-07-01T14:30:00Z",
  "lastLoginAt": "2026-07-22T07:45:00Z",
  "deviceInfo": {
    "os": "Android 13",
    "model": "Samsung Galaxy A54",
    "appVersion": "1.2.0"
  }
}
```

### Elderly Profile Document

```json
{
  "id": "ep_xyz789",
  "userId": "uid_elder001",
  "fullName": "Nguyễn Thị B",
  "dateOfBirth": "1948-03-15T00:00:00Z",
  "gender": "FEMALE",
  "bloodType": "O",
  "height": 155.0,
  "weight": 52.0,
  "medicalConditions": ["Tăng huyết áp", "Đái tháo đường type 2"],
  "drugAllergies": ["Penicillin"],
  "address": "123 Nguyễn Huệ, Quận 1, TP.HCM",
  "photoUrl": "https://storage.googleapis.com/geriatric.../profiles/ep_xyz789.jpg",
  "emergencyContact": {
    "name": "Nguyễn Văn A",
    "phone": "+84901234567",
    "relationship": "Con trai"
  },
  "linkCode": "483920",
  "linkCodeExpiresAt": "2026-07-23T08:00:00Z",
  "createdAt": "2026-01-20T10:00:00Z",
  "updatedAt": "2026-07-20T09:15:00Z"
}
```

### Health Record – Blood Pressure

```json
{
  "id": "hr_bp_001",
  "elderlyId": "ep_xyz789",
  "type": "BLOOD_PRESSURE",
  "values": {
    "sys": 148,
    "dia": 92,
    "pulse": 78
  },
  "measuredAt": "2026-07-22T07:30:00Z",
  "note": "Sau khi thức dậy",
  "isAbnormal": true,
  "pendingSync": false,
  "createdBy": "uid_elder001",
  "syncedAt": "2026-07-22T07:30:05Z"
}
```

### SOS Event Document

```json
{
  "id": "sos_20260722_001",
  "elderlyId": "ep_xyz789",
  "triggerType": "BUTTON",
  "latitude": 10.7769,
  "longitude": 106.7009,
  "locationAccuracy": 12.5,
  "status": "ACKNOWLEDGED",
  "notifiedUserIds": ["uid_abc123", "uid_def456"],
  "acknowledgedBy": "uid_abc123",
  "acknowledgedAt": "2026-07-22T08:05:30Z",
  "triggeredAt": "2026-07-22T08:04:00Z",
  "sentAt": "2026-07-22T08:04:02Z"
}
```

### Medication Schedule Document

```json
{
  "id": "ms_001",
  "elderlyId": "ep_xyz789",
  "createdBy": "uid_abc123",
  "medicationName": "Amlodipine 5mg",
  "dosage": "1 viên",
  "timesPerDay": 1,
  "scheduledTimes": ["07:00"],
  "startDate": "2026-01-20T00:00:00Z",
  "endDate": null,
  "note": "Uống sau khi ăn sáng",
  "photoUrl": null,
  "color": "#4CAF50",
  "isActive": true,
  "createdAt": "2026-01-20T10:00:00Z",
  "updatedAt": "2026-01-20T10:00:00Z"
}
```

### Medication Log Document

```json
{
  "id": "ml_20260722_001",
  "scheduleId": "ms_001",
  "elderlyId": "ep_xyz789",
  "scheduledTime": "2026-07-22T07:00:00Z",
  "status": "TAKEN",
  "respondedAt": "2026-07-22T07:08:22Z",
  "familyAlertedAt": null
}
```

### Family Link Document

```json
{
  "id": "fl_001",
  "familyUserId": "uid_abc123",
  "elderlyProfileId": "ep_xyz789",
  "elderlyUserId": "uid_elder001",
  "relationship": "Con trai",
  "status": "ACTIVE",
  "permissions": {
    "viewHealth": true,
    "viewLocation": true,
    "manageMedications": true,
    "exportReports": true
  },
  "createdAt": "2026-01-20T11:00:00Z",
  "approvedAt": "2026-01-20T11:05:00Z"
}
```

---

## 12.3 Firestore Security Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // ========== HELPER FUNCTIONS ==========

    function isAuthenticated() {
      return request.auth != null;
    }

    function isOwner(userId) {
      return request.auth.uid == userId;
    }

    function getUserRole() {
      return get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role;
    }

    function isAdmin() {
      return getUserRole() == 'ADMIN';
    }

    function isElderly() {
      return getUserRole() == 'ELDERLY';
    }

    function isFamily() {
      return getUserRole() == 'FAMILY';
    }

    function isDoctor() {
      return getUserRole() == 'DOCTOR';
    }

    function isFamilyOfElderly(elderlyId) {
      return exists(/databases/$(database)/documents/family_links/$(request.auth.uid + '_' + elderlyId))
        && get(/databases/$(database)/documents/family_links/$(request.auth.uid + '_' + elderlyId)).data.status == 'ACTIVE';
    }

    function isDoctorOfElderly(elderlyId) {
      return exists(/databases/$(database)/documents/doctor_links/$(request.auth.uid + '_' + elderlyId))
        && get(/databases/$(database)/documents/doctor_links/$(request.auth.uid + '_' + elderlyId)).data.status == 'ACTIVE';
    }

    function isElderlyOwner(elderlyId) {
      return get(/databases/$(database)/documents/elderly_profiles/$(elderlyId)).data.userId == request.auth.uid;
    }

    function canReadElderlyData(elderlyId) {
      return isAdmin()
        || isElderlyOwner(elderlyId)
        || isFamilyOfElderly(elderlyId)
        || isDoctorOfElderly(elderlyId);
    }

    function canWriteElderlyData(elderlyId) {
      return isAdmin()
        || isElderlyOwner(elderlyId)
        || isFamilyOfElderly(elderlyId);
    }

    // ========== USERS ==========

    match /users/{userId} {
      allow read: if isAuthenticated() && (isOwner(userId) || isAdmin());
      allow create: if isAuthenticated() && isOwner(userId);
      allow update: if isAuthenticated() && (isOwner(userId) || isAdmin());
      allow delete: if isAdmin();
    }

    // ========== ELDERLY PROFILES ==========

    match /elderly_profiles/{elderlyId} {
      allow read: if isAuthenticated() && canReadElderlyData(elderlyId);
      allow create: if isAuthenticated() && (isElderly() || isFamily());
      allow update: if isAuthenticated() && canWriteElderlyData(elderlyId);
      allow delete: if isAdmin();

      // --- HEALTH RECORDS ---
      match /health_records/{recordId} {
        allow read: if isAuthenticated() && canReadElderlyData(elderlyId);
        allow create: if isAuthenticated() && canWriteElderlyData(elderlyId)
          && request.resource.data.measuredAt <= request.time + duration.value(5, 'm');
        allow update: if false; // Immutable - không cho sửa
        allow delete: if false; // Không cho xóa
      }

      // --- SOS EVENTS ---
      match /sos_events/{sosId} {
        allow read: if isAuthenticated() && canReadElderlyData(elderlyId);
        allow create: if isAuthenticated()
          && (isElderlyOwner(elderlyId) || isAdmin());
        allow update: if isAuthenticated()
          && (isFamilyOfElderly(elderlyId) || isElderlyOwner(elderlyId) || isAdmin())
          && !('triggeredAt' in request.resource.data.diff(resource.data).affectedKeys()); // Không đổi triggeredAt
        allow delete: if false;
      }

      // --- MEDICATION SCHEDULES ---
      match /medication_schedules/{scheduleId} {
        allow read: if isAuthenticated() && canReadElderlyData(elderlyId);
        allow create: if isAuthenticated()
          && (isFamilyOfElderly(elderlyId) || isDoctorOfElderly(elderlyId) || isAdmin());
        allow update: if isAuthenticated()
          && (isFamilyOfElderly(elderlyId) || isDoctorOfElderly(elderlyId) || isAdmin());
        allow delete: if isAuthenticated()
          && (isFamilyOfElderly(elderlyId) || isAdmin());

        // --- MEDICATION LOGS ---
        match /medication_logs/{logId} {
          allow read: if isAuthenticated() && canReadElderlyData(elderlyId);
          allow create: if isAuthenticated()
            && (isElderlyOwner(elderlyId) || isFamilyOfElderly(elderlyId));
          allow update: if false; // Immutable
          allow delete: if false;
        }
      }

      // --- LOCATION HISTORY ---
      match /location_history/{locationId} {
        allow read: if isAuthenticated()
          && (isElderlyOwner(elderlyId) || isFamilyOfElderly(elderlyId) || isAdmin());
        allow create: if isAuthenticated() && isElderlyOwner(elderlyId);
        allow update: if false;
        allow delete: if false; // Xóa qua Cloud Function (TTL)
      }

      // --- ACTIVITY LOGS ---
      match /activity_logs/{logId} {
        allow read: if isAuthenticated()
          && (isElderlyOwner(elderlyId) || isFamilyOfElderly(elderlyId) || isAdmin());
        allow create: if isAuthenticated(); // Server-side via Functions preferred
        allow update: if false;
        allow delete: if false;
      }
    }

    // ========== FAMILY LINKS ==========

    match /family_links/{linkId} {
      allow read: if isAuthenticated()
        && (resource.data.familyUserId == request.auth.uid
          || isElderlyOwner(resource.data.elderlyProfileId)
          || isAdmin());
      allow create: if isAuthenticated() && isFamily()
        && request.resource.data.familyUserId == request.auth.uid;
      allow update: if isAuthenticated()
        && (isElderlyOwner(resource.data.elderlyProfileId) // Elderly approve/reject
          || resource.data.familyUserId == request.auth.uid  // Family cancel
          || isAdmin());
      allow delete: if isAdmin();
    }

    // ========== DOCTOR LINKS ==========

    match /doctor_links/{linkId} {
      allow read: if isAuthenticated()
        && (resource.data.doctorUserId == request.auth.uid
          || isFamilyOfElderly(resource.data.elderlyProfileId)
          || isElderlyOwner(resource.data.elderlyProfileId)
          || isAdmin());
      allow create: if isAuthenticated() && isDoctor()
        && request.resource.data.doctorUserId == request.auth.uid;
      allow update: if isAuthenticated()
        && (isFamilyOfElderly(resource.data.elderlyProfileId) || isAdmin());
      allow delete: if isAdmin();

      match /doctor_notes/{noteId} {
        allow read: if isAuthenticated()
          && (resource.data.doctorUserId == request.auth.uid
            || canReadElderlyData(resource.data.elderlyId));
        allow create: if isAuthenticated() && isDoctor()
          && isDoctorOfElderly(request.resource.data.elderlyId);
        allow update: if isAuthenticated()
          && resource.data.doctorUserId == request.auth.uid;
        allow delete: if resource.data.doctorUserId == request.auth.uid || isAdmin();
      }
    }

    // ========== HEALTH THRESHOLDS ==========

    match /health_thresholds/{docId} {
      allow read: if isAuthenticated() && canReadElderlyData(docId);
      allow write: if isAuthenticated()
        && (isDoctorOfElderly(docId) || isAdmin()
          || docId == 'default' && isAdmin());
    }

    // ========== NOTIFICATION SETTINGS ==========

    match /notification_settings/{userId} {
      allow read: if isAuthenticated() && isOwner(userId);
      allow write: if isAuthenticated() && isOwner(userId)
        && request.resource.data.sosAlerts == true; // SOS không thể tắt
    }
  }
}
```

---

## 12.4 Firestore Indexes

```json
{
  "indexes": [
    {
      "collectionGroup": "health_records",
      "queryScope": "COLLECTION",
      "fields": [
        { "fieldPath": "elderlyId", "order": "ASCENDING" },
        { "fieldPath": "type", "order": "ASCENDING" },
        { "fieldPath": "measuredAt", "order": "DESCENDING" }
      ]
    },
    {
      "collectionGroup": "health_records",
      "queryScope": "COLLECTION",
      "fields": [
        { "fieldPath": "elderlyId", "order": "ASCENDING" },
        { "fieldPath": "isAbnormal", "order": "ASCENDING" },
        { "fieldPath": "measuredAt", "order": "DESCENDING" }
      ]
    },
    {
      "collectionGroup": "sos_events",
      "queryScope": "COLLECTION",
      "fields": [
        { "fieldPath": "elderlyId", "order": "ASCENDING" },
        { "fieldPath": "triggeredAt", "order": "DESCENDING" }
      ]
    },
    {
      "collectionGroup": "medication_logs",
      "queryScope": "COLLECTION_GROUP",
      "fields": [
        { "fieldPath": "elderlyId", "order": "ASCENDING" },
        { "fieldPath": "scheduledTime", "order": "DESCENDING" }
      ]
    },
    {
      "collectionGroup": "family_links",
      "queryScope": "COLLECTION",
      "fields": [
        { "fieldPath": "familyUserId", "order": "ASCENDING" },
        { "fieldPath": "status", "order": "ASCENDING" }
      ]
    },
    {
      "collectionGroup": "location_history",
      "queryScope": "COLLECTION",
      "fields": [
        { "fieldPath": "elderlyId", "order": "ASCENDING" },
        { "fieldPath": "recordedAt", "order": "DESCENDING" }
      ]
    }
  ]
}
```

---

## 12.5 Offline Persistence Strategy

```dart
// Cấu hình Firestore offline trong Flutter
FirebaseFirestore.instance.settings = const Settings(
  persistenceEnabled: true,
  cacheSizeBytes: Settings.CACHE_SIZE_UNLIMITED,
);
```

**Chiến lược đồng bộ:**

| Loại dữ liệu | Offline behavior | Sync khi online |
|---|---|---|
| Health records | Lưu Hive local, pending queue | Auto sync, giữ measuredAt gốc |
| Medication logs | Lưu local với Firestore cache | Auto sync |
| SOS | Phát âm báo động, lưu local | Retry 3 lần với exponential backoff |
| Profile | Serve từ Firestore cache | Background sync |
| Location | Không lưu khi offline | Skip, resume khi online |
