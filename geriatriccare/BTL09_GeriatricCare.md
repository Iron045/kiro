# BTL09 - GeriatricCare
## Giám sát & Chăm sóc Người cao tuổi Độc cư

**Học phần:** CSE441 - Phát triển ứng dụng di động (Flutter)  
**Nhóm lĩnh vực:** 🏥 HealthTech & Social  
**Nền tảng:** Mobile/Tablet App + Web Dashboard  

---

## I. YÊU CẦU CHỨC NĂNG (FUNCTIONAL REQUIREMENTS)

### 1.1 Phân hệ Người cao tuổi (Mobile/Tablet - UI to, chữ lớn)

#### User Story 1: Nhấn nút khẩn cấp khi gặp sự cố
> *Là người cao tuổi sống một mình, tôi muốn có nút khẩn cấp dễ tìm để nhấn khi gặp tai nạn, để con cái và bác sĩ đến kịp thời.*

**Acceptance Criteria:**
- WHEN màn hình chính của app, THEN nút SOS màu đỏ lớn chiếm ít nhất 30% diện tích màn hình, không thể bị ẩn.
- WHEN người dùng nhấn giữ nút SOS 2 giây (chống nhấn nhầm), THEN đếm ngược 5 giây với âm thanh beep; nếu không hủy → gửi alert.
- WHEN alert được gửi, THEN FCM gửi đến: tất cả liên hệ khẩn cấp đã đăng ký (con cái) + bác sĩ gia đình kèm tọa độ GPS.
- WHEN lắc điện thoại mạnh, THEN cũng kích hoạt cùng quy trình SOS (cho trường hợp bị ngã không thể nhấn nút).

#### User Story 2: Nhắc uống thuốc bằng giọng nói
> *Là người cao tuổi, tôi muốn được nhắc uống thuốc bằng giọng nói tiếng Việt, để không quên kể cả khi không đeo kính đọc màn hình.*

**Acceptance Criteria:**
- WHEN đến giờ uống thuốc, THEN app phát âm thanh + giọng đọc tiếng Việt: "Đã đến giờ uống thuốc [tên thuốc], liều [liều lượng]".
- WHEN người dùng nhấn "Đã uống", THEN ghi nhận thời gian uống và thông báo xanh "Đã ghi nhận".
- IF người dùng không phản hồi trong 30 phút, THEN app phát lại nhắc nhở và FCM gửi cho con cái "Bố/Mẹ chưa xác nhận uống thuốc lúc [giờ]".

#### User Story 3: Ghi nhận chỉ số sức khỏe
> *Là người cao tuổi, tôi muốn ghi lại huyết áp và đường huyết mỗi ngày qua app, để bác sĩ theo dõi từ xa.*

**Acceptance Criteria:**
- WHEN người dùng mở "Ghi chỉ số", THEN form có chữ to, nút số lớn để nhập huyết áp (tâm thu/tâm trương) và đường huyết.
- WHEN nhập xong, THEN dữ liệu được lưu vào Firestore với timestamp.
- IF chỉ số bất thường (huyết áp > 180/110 hoặc < 90/60), THEN app hiển thị cảnh báo lớn và FCM gửi tới con cái + bác sĩ.

---

### 1.2 Phân hệ Con cái Chăm sóc (Mobile App)

#### User Story 4: Nhận và xử lý alert khẩn cấp
> *Là con cái đang đi làm xa, tôi muốn nhận cảnh báo tức thì khi bố/mẹ có vấn đề, để có thể phản ứng ngay.*

**Acceptance Criteria:**
- WHEN nhận FCM alert SOS, THEN app hiển thị màn hình fullscreen với: ảnh bố/mẹ, tọa độ GPS, bản đồ mini, nút gọi điện ngay.
- WHEN con cái xác nhận "Đã biết - đang xử lý", THEN trạng thái alert cập nhật trên Web Dashboard của bác sĩ.

#### User Story 5: Theo dõi lịch uống thuốc và chỉ số sức khỏe
> *Là con cái, tôi muốn xem lịch sử uống thuốc và chỉ số sức khỏe của bố/mẹ trong tuần, để biết bố/mẹ có đang tuân thủ điều trị không.*

**Acceptance Criteria:**
- WHEN con cái mở app, THEN thấy tổng quan: Uống thuốc hôm nay (bao nhiêu mũi đã uống/tổng), Chỉ số gần nhất (huyết áp, đường huyết).
- WHEN xem chi tiết, THEN biểu đồ huyết áp và đường huyết theo thời gian (7 ngày, 30 ngày).

#### User Story 6: Quản lý lịch nhắc thuốc cho bố/mẹ
> *Là con cái, tôi muốn thiết lập lịch nhắc uống thuốc cho bố/mẹ từ xa qua app, để không phải điện thoại nhắc mỗi ngày.*

**Acceptance Criteria:**
- WHEN con cái tạo lịch thuốc, THEN nhập: tên thuốc, liều lượng, số lần/ngày, giờ cụ thể, ngày bắt đầu/kết thúc.
- WHEN lưu lịch, THEN thiết bị của bố/mẹ nhận cập nhật và lịch nhắc tự động hoạt động.

---

### 1.3 Phân hệ Phòng khám / Bác sĩ Gia đình (Web Dashboard/Mobile Dashboard)

#### User Story 7: Giám sát chỉ số sức khỏe nhiều bệnh nhân
> *Là bác sĩ gia đình, tôi muốn xem dashboard chỉ số sức khỏe của tất cả bệnh nhân cao tuổi đang theo dõi, để phát hiện sớm bất thường mà không cần gặp trực tiếp.*

**Acceptance Criteria:**
- WHEN bác sĩ mở Web/Mobile, THEN thấy danh sách bệnh nhân với màu trạng thái: xanh (bình thường), vàng (cần chú ý), đỏ (cần can thiệp ngay).
- WHEN click vào bệnh nhân, THEN xem biểu đồ đầy đủ huyết áp + đường huyết theo thời gian, lịch sử SOS alerts.
- WHEN bác sĩ ghi chú khuyến nghị, THEN con cái nhận FCM thông báo "Bác sĩ có ghi chú mới".

---

## II. YÊU CẦU PHI CHỨC NĂNG

| Thuộc tính | Yêu cầu |
|---|---|
| **Accessibility** | Font size tối thiểu 18sp, contrast ratio ≥ 4.5:1, hỗ trợ TTS |
| **SOS Reliability** | SOS phải gửi được kể cả khi app ở background |
| **Battery** | Shake detection không tốn quá 3% pin/giờ khi màn hình tắt |
| **Alert Latency** | FCM SOS nhận ở thiết bị con cái trong ≤ 15 giây |

---

## III. KIẾN TRÚC HỆ THỐNG

```
┌──────────────────────────────────────────────────────────────┐
│                    GERIATRICCARE SYSTEM                      │
├──────────────┬───────────────┬───────────────────────────────┤
│ Elder Tablet │ Child Mobile  │ Doctor Web/Mobile + Firebase  │
│              │               │                               │
│ SOS Button   │ Alert Screen  │ Firestore:                    │
│ Shake Detect │ Health Charts │ - patients                    │
│ Pill Reminders│ Pill Schedule │ - health_records             │
│ Health Input │  (CRUD)       │ - pill_schedules              │
│              │               │ - sos_alerts                  │
│ Background   │               │                               │
│ Service:     │               │ FCM (High priority):          │
│ - Shake det. │               │ - SOS channel                 │
│ - Pill timer │               │ - Pill miss channel           │
│ - Health mon.│               │ - Health alert channel        │
└──────────────┴───────────────┴───────────────────────────────┘
```

---

## IV. MÔ HÌNH DỮ LIỆU

### Entity: Patient (Bệnh nhân cao tuổi)
```dart
class Patient {
  final String id;
  final String elderUserId;      // Firebase UID thiết bị của người già
  final String fullName;
  final int age;
  final String bloodType;
  final String medicalConditions;
  final List<String> caretakerIds; // UIDs con cái
  final String? doctorId;
  final double? homeLat;
  final double? homeLng;
}
```

### Entity: HealthRecord (Chỉ số sức khỏe)
```dart
class HealthRecord {
  final String id;
  final String patientId;
  final int systolicBp;          // Huyết áp tâm thu
  final int diastolicBp;         // Huyết áp tâm trương
  final double bloodSugar;       // Đường huyết (mmol/L)
  final int? heartRate;
  final DateTime recordedAt;
  final bool isAlert;            // true nếu nằm ngoài ngưỡng bình thường
}
```

### Entity: PillSchedule (Lịch uống thuốc)
```dart
class PillSchedule {
  final String id;
  final String patientId;
  final String medicationName;
  final String dosage;           // "1 viên"
  final List<TimeOfDay> times;   // Giờ nhắc trong ngày
  final DateTime startDate;
  final DateTime? endDate;
  final bool isActive;
}
```

### Entity: PillLog (Lịch sử uống thuốc)
```dart
class PillLog {
  final String id;
  final String scheduleId;
  final String patientId;
  final DateTime scheduledTime;
  final DateTime? takenAt;       // null = chưa uống
  final bool isMissed;           // true nếu quá 30 phút không xác nhận
}
```

### Entity: SosAlert (Cảnh báo khẩn)
```dart
class SosAlert {
  final String id;
  final String patientId;
  final String triggerMethod;    // 'button' | 'shake'
  final double lat;
  final double lng;
  final String status;           // 'active' | 'acknowledged' | 'resolved'
  final List<String> notifiedIds;
  final DateTime triggeredAt;
  final DateTime? acknowledgedAt;
}
```

---

## V. LUỒNG GIAO DIỆN (UI FLOW)

### Elder Tablet App (Đơn giản, chữ to):
```
Login
    └──Home Screen (luôn mở)
          ├── [SOS Button - Màu đỏ, to, ở giữa hoặc trên cùng]
          ├── Đồng hồ + Ngày/Tháng (chữ to)
          ├── Thuốc sắp uống (next reminder card)
          ├── [Nút] Ghi chỉ số hôm nay
          │       ├── Nhập huyết áp (keypad to)
          │       └── Nhập đường huyết
          └── [Nút] Gọi con cái (shortcut 1 chạm)
```

### Child Mobile App:
```
Login
    └── Dashboard
            ├── Alert Feed (SOS + pill missed)
            ├── Sức khỏe Bố/Mẹ (today snapshot)
            │       └── Chi tiết → Charts
            ├── Lịch thuốc
            │       ├── Xem lịch hiện tại
            │       └── Thêm/Sửa thuốc
            └── Cài đặt liên hệ khẩn
```

### Doctor Web/Mobile Dashboard:
```
Login
    └── Clinic Overview
            ├── Patient List (color status)
            │       └── Patient Detail
            │               ├── Health Charts (BP, Sugar 30 days)
            │               ├── SOS History
            │               └── Add Doctor Note
            └── Alerts Feed (unread alerts)
```

---

## VI. CÔNG NGHỆ SỬ DỤNG

| Layer | Package/Technology |
|---|---|
| **State Management** | `riverpod ^2.x` |
| **TTS (Voice Reminder)** | `flutter_tts ^3.x` |
| **Shake Detection** | `shake ^2.x` |
| **Background Service** | `flutter_background_service ^5.x` |
| **GPS** | `geolocator ^10.x` |
| **Firebase** | Auth, Firestore, FCM (high priority channel) |
| **Charts** | `fl_chart ^0.66.x` |
| **Local Notifications** | `flutter_local_notifications ^16.x` |

---

## VII. CÁC ĐIỂM KỸ THUẬT QUAN TRỌNG

1. **SOS High-Priority FCM**: Dùng `priority: high` và `content_available: true` trong FCM payload để wake device kể cả khi tắt tiếng.
2. **Background Shake Detection**: Android Foreground Service với accelerometer listener; iOS sử dụng `CMMotionManager` qua Flutter plugin.
3. **Pill Reminder Reliability**: Kết hợp FCM + `flutter_local_notifications` — nếu FCM không đến (offline), local notification vẫn báo.
4. **Accessibility Design**: Dùng `semanticsLabel` cho tất cả widget, font scale responsive, nút tối thiểu 48x48dp theo Material Guidelines.
5. **Health Threshold Alerts**: Cloud Function watch Firestore `health_records` on write → check thresholds → nếu alert cần thiết → dispatch FCM ngay lập tức.
