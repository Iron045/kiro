# Chương 19: Future Improvements
## GeriatricCare – Đề xuất cải tiến & Lộ trình phát triển

---

## 19.1 Product Roadmap

```
2026 Q3–Q4          2027 Q1–Q2          2027 Q3–Q4          2028+
───────────         ────────────        ────────────        ──────────
   V1.0                V1.5                V2.0              V3.0+
  (MVP)             (Enhanced)           (Smart)           (AI/IoT)
────────────────────────────────────────────────────────────────────
• SOS Button        • iOS Support       • AI Fall Detect    • Smart Watch
• Medication        • Chat Text         • Smart Watch Sync  • Stroke Predict
• Health Track      • Video Call        • Wearable HR       • ML Health
• GPS               • AI Health Tips    • Appointment Mgmt  • Doctor Chat AI
• Family Dashboard  • Multi-lang        • Insurance Link    • Smart Home
• Doctor View       • Report V2         • Community         • Hospital EHR
```

---

## 19.2 V1.5 – Enhanced Features (Q1–Q2 2027)

### 19.2.1 iOS Support

**Mô tả:** Mở rộng sang nền tảng iOS để phục vụ người dùng iPhone.

**Công việc cần làm:**
```
• Flutter code đã cross-platform – không cần viết lại
• iOS-specific configurations:
  - XCode setup, Bundle ID, Provisioning Profile
  - Apple Push Notification Service (APNs) thay FCM
  - iOS permission strings trong Info.plist
  - TestFlight distribution
  - App Store Connect submission
• Kiểm thử trên iPhone 12+ và iPad
• Tương thích iOS 15+
```

**Ước tính:** 3–4 tuần (1 iOS developer)

---

### 19.2.2 Chat text 1:1

**Mô tả:** Người thân và người cao tuổi nhắn tin trực tiếp trong app.

**Kiến trúc đề xuất:**
```dart
// Sử dụng Firestore realtime cho chat đơn giản
// Collection: chats/{chatId}/messages/{messageId}
// Không cần WebSocket phức tạp

class ChatMessage {
  final String id;
  final String senderId;
  final String content;
  final MessageType type; // TEXT, VOICE, IMAGE
  final DateTime sentAt;
  final bool isRead;
}
```

**Tính năng:**
- Tin nhắn văn bản
- Tin nhắn giọng nói (ghi âm + phát lại)
- Ảnh / hình ảnh
- Emoji lớn, dễ nhìn
- Read receipts
- Push notification khi có tin nhắn mới

---

### 19.2.3 AI Health Tips

**Mô tả:** Gợi ý sức khỏe cá nhân hóa dựa trên xu hướng chỉ số.

**Ví dụ gợi ý:**
```
📊 Dựa trên dữ liệu 7 ngày qua của bạn:
• Huyết áp có xu hướng cao vào buổi sáng
  → Uống thuốc huyết áp trước 7:00 sáng
• Đường huyết ổn định sau bữa ăn
  → Duy trì chế độ ăn hiện tại
⚠️ Lưu ý: Thông tin tham khảo, không thay thế bác sĩ
```

**Công nghệ:** Firebase ML Kit + rule-based engine (giai đoạn đầu), ML model (giai đoạn sau)

---

## 19.3 V2.0 – Smart Features (Q3–Q4 2027)

### 19.3.1 AI Fall Detection

**Mô tả:** Tự động phát hiện té ngã bằng cảm biến accelerometer + gyroscope và kích hoạt SOS.

**Thuật toán đề xuất:**
```dart
class FallDetectionService {
  // Dấu hiệu té ngã:
  // 1. Gia tốc đột ngột > 30 m/s² (IMPACT)
  // 2. Tiếp theo bởi giai đoạn gia tốc gần 0 (nằm im – POST_IMPACT)
  // 3. Kéo dài > 2 giây → xác nhận té ngã

  static const double impactThreshold = 30.0;   // m/s²
  static const double stillnessThreshold = 3.0;  // m/s²
  static const Duration confirmDuration = Duration(seconds: 2);

  Stream<FallEvent> detectFalls() {
    return accelerometerEvents
        .bufferTime(const Duration(milliseconds: 100))
        .where(_hasImpact)
        .flatMap(_checkPostImpactStillness);
  }
}
```

**Accuracy target:** Sensitivity ≥ 90%, Specificity ≥ 95%

**Tích hợp với SOS:**
- Fall detected → đếm ngược 15 giây (dài hơn manual SOS để tránh false positive)
- Người cao tuổi có thể bấm "Tôi ổn" để hủy
- Hết countdown → kích hoạt SOS tự động

---

### 19.3.2 Smart Watch Integration

**Thiết bị hỗ trợ đề xuất:**
- Samsung Galaxy Watch (Wear OS)
- Xiaomi Smart Band
- Fitbit

**Dữ liệu từ Smart Watch:**

| Chỉ số | Tần suất | Độ chính xác |
|---|---|---|
| Nhịp tim (HR) | 1 phút/lần | ±3 bpm |
| SpO2 | 30 phút/lần | ±2% |
| Bước chân | Realtime | ±5% |
| Giấc ngủ | Hàng ngày | Moderate |
| Phát hiện rung bất thường | Realtime | Cao |

**Kiến trúc:**
```
Smart Watch → Bluetooth → Flutter App → Firestore → Family Dashboard
                              ↓
                     Tự động ghi health_records
                     (không cần nhập thủ công)
```

---

### 19.3.3 Stroke Prediction Alert

**Mô tả:** Cảnh báo sớm nguy cơ đột quỵ dựa trên tổ hợp chỉ số.

**Rule-based model giai đoạn 1:**
```
Nguy cơ CAO khi:
  HA SYS > 180 mmHg (liên tục 2 lần đo trong 1 giờ) AND
  (Nhịp tim bất thường) AND
  (SpO2 < 94%)
→ Alert: "Cảnh báo nguy cơ đột quỵ cao – Liên hệ y tế ngay"
```

**ML model giai đoạn 2:**
- Input: 30 ngày lịch sử chỉ số sức khỏe
- Output: Risk score 0–100
- Model: Random Forest hoặc LSTM
- Training data: Dữ liệu ẩn danh hóa từ hệ thống (với consent)

---

### 19.3.4 Video Call khẩn cấp

**Mô tả:** Gọi video 1 chạm đến người thân khi cần hỗ trợ.

**Công nghệ đề xuất:**
```yaml
# Flutter WebRTC / Agora / ZEGOCLOUD
agora_rtc_engine: ^6.x.x
# hoặc
zego_uikit_prebuilt_call: ^4.x.x
```

**UX flow:**
```
Người cao tuổi: Bấm nút 📹 → Chọn người thân → Gọi ngay
Người thân: Nhận call notification → Chấp nhận → Video call
```

**Tính năng:**
- Giao diện đơn giản, nút TO
- Tự động bật camera người thân
- Chất lượng video adaptive theo băng thông

---

## 19.4 V3.0 – AI & IoT (2028+)

### 19.4.1 Machine Learning Health Analysis

```
Mục tiêu:
• Dự đoán xu hướng chỉ số sức khỏe 7–30 ngày tới
• Phát hiện pattern bất thường trước khi xảy ra
• Personalized medication optimization suggestions
• Anomaly detection: "Hôm nay HA của bà bất thường so với lịch sử"

Architecture:
┌──────────────────────────────────────────────┐
│  Firestore → BigQuery → Vertex AI → Model    │
│  (raw data)  (warehouse)  (training)  (serve) │
│                                              │
│  Model API → Cloud Function → FCM alert      │
└──────────────────────────────────────────────┘

Model types:
• Time Series (ARIMA / LSTM): Trend prediction
• Anomaly Detection: Isolation Forest
• Classification: Risk stratification
```

---

### 19.4.2 Smart Home Integration

**Thiết bị hỗ trợ:**

| Thiết bị | Tích hợp | Mục đích |
|---|---|---|
| Smart sensor cửa/cửa sổ | MQTT / Zigbee | Phát hiện không ra khỏi phòng > 12h |
| Smart camera (trong nhà) | Edge AI | Fall detection camera-based (opt-in) |
| Smart doorbell | WebRTC | Người thân thấy ai đến cửa |
| Smart plug | IoT | Theo dõi thiết bị điện (bếp gas) |
| Panic button vật lý | BLE | Nút SOS độc lập không cần điện thoại |

---

### 19.4.3 Hospital EHR Integration

**Mô tả:** Tích hợp với hệ thống hồ sơ bệnh án điện tử (EMR/EHR) của bệnh viện Việt Nam.

**Tiêu chuẩn:** HL7 FHIR R4

```
GeriatricCare ←→ FHIR API ←→ Bệnh viện EMR
    │                              │
    └── Đồng bộ lịch sử khám      │
    └── Nhận đơn thuốc mới        │
    └── Gửi monitoring data ───────┘
```

---

## 19.5 Technical Debt & Improvements

| Hạng mục | Mô tả | Ưu tiên |
|---|---|---|
| Performance | Lazy loading cho health history dài | Medium |
| Caching | Redis/Memcache cho report generation | Low |
| Testing | Tăng E2E test coverage lên 80% | High |
| Accessibility | WCAG 2.2 Level AAA compliance | Medium |
| Localization | Thêm ngôn ngữ: Khmer, Lào, Anh | Low |
| Analytics | Custom funnel tracking | Medium |
| A/B Testing | Firebase Remote Config experiments | Low |
| Offline Mode | Full offline cho Doctor Dashboard | Medium |

---

## 19.6 Community & Ecosystem

### Mô hình mở rộng

```
GeriatricCare Platform
├── Mobile App (Flutter) ← Hiện tại
├── Web Dashboard (Admin) ← V2
├── API (Public) ← V3 (cho đối tác)
├── SDK (Third-party integrations)
└── Marketplace (Plugin của bên thứ ba)
```

### Partnership Opportunities

| Đối tác | Lợi ích tích hợp |
|---|---|
| Bảo hiểm sức khỏe | Giảm phí bảo hiểm cho người dùng tích cực |
| Bệnh viện | Chia sẻ dữ liệu sức khỏe với consent |
| Nhà sản xuất thiết bị đo | SDK kết nối trực tiếp |
| Dược phẩm | Nhắc nhở tái mua thuốc |
| Taxi / dịch vụ đưa đón | Gọi xe đến bệnh viện từ app |

---

## 19.7 Tổng kết đề xuất theo độ ưu tiên

| Tính năng | Impact | Effort | Priority | Version |
|---|---|---|---|---|
| iOS Support | Cao | Trung bình | 🔴 High | V1.5 |
| Chat text | Cao | Trung bình | 🔴 High | V1.5 |
| Video Call khẩn cấp | Rất cao | Cao | 🟠 Medium | V2.0 |
| AI Fall Detection | Rất cao | Cao | 🟠 Medium | V2.0 |
| Smart Watch Sync | Cao | Rất cao | 🟡 Medium | V2.0 |
| AI Health Tips | Trung bình | Trung bình | 🟡 Medium | V1.5 |
| Stroke Prediction | Rất cao | Rất cao | 🟠 Medium | V2.0 |
| ML Health Analysis | Cao | Rất cao | 🟢 Low | V3.0 |
| Smart Home IoT | Trung bình | Rất cao | 🟢 Low | V3.0 |
| Hospital EHR | Cao | Cực cao | 🟢 Low | V3.0 |
