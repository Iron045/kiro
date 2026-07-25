# CHƯƠNG 6: KẾT LUẬN VÀ HƯỚNG PHÁT TRIỂN

---

## 6.1 Kết quả đạt được

### 6.1.1 Về mặt lý thuyết

Đồ án đã nghiên cứu và trình bày thành công các kiến thức lý thuyết nền tảng:

- **Phương pháp OOA/OOD theo chuẩn UML 2.5:** Xây dựng đầy đủ 8 loại sơ đồ UML bao gồm Use Case, Activity, Sequence, Class, State Machine, Component, Package và Deployment Diagram. Các sơ đồ phản ánh chính xác nghiệp vụ và kiến trúc kỹ thuật của hệ thống.

- **Kiến trúc Clean Architecture:** Áp dụng thành công mô hình phân tầng 5 lớp (Presentation – Application – Domain – Data – Infrastructure), đảm bảo nguyên tắc Dependency Rule. Domain Layer thuần túy không phụ thuộc bất kỳ framework nào, đạt tỷ lệ kiểm thử 100%.

- **Nguyên tắc SOLID và Design Patterns:** Vận dụng triệt để các nguyên tắc Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation và Dependency Inversion. Repository Pattern, Observer Pattern và Factory Pattern được áp dụng phù hợp theo từng ngữ cảnh.

- **Bảo mật OWASP Mobile:** Phân tích và đề xuất biện pháp đối phó đầy đủ cho OWASP Mobile Top 10. Mô hình Defense in Depth 6 lớp đảm bảo dữ liệu y tế nhạy cảm được bảo vệ toàn diện.

### 6.1.2 Về mặt thực tiễn – Sản phẩm

Ứng dụng GeriatricCare phiên bản 1.0 đã được xây dựng hoàn chỉnh với các kết quả nổi bật:

**Chức năng đã hoàn thành:**

| STT | Module | Tính năng nổi bật | Trạng thái |
|---|---|---|---|
| 1 | Authentication | RBAC 4 vai trò, Email OTP | ✅ Hoàn thành |
| 2 | Quản lý hồ sơ | Hồ sơ y tế đầy đủ, liên kết QR | ✅ Hoàn thành |
| 3 | SOS Panic Button | Đếm ngược 5s, GPS tự động, FCM HIGH priority | ✅ Hoàn thành |
| 4 | Shake Detection | Nhận diện lắc bằng accelerometer | ✅ Hoàn thành |
| 5 | Theo dõi sức khỏe | 6 chỉ số, offline-first, cảnh báo ngưỡng | ✅ Hoàn thành |
| 6 | Nhắc uống thuốc | TTS tiếng Việt, AlarmManager, nhắc lặp | ✅ Hoàn thành |
| 7 | Family Dashboard | Realtime updates, SOS banner, GPS maps | ✅ Hoàn thành |
| 8 | Doctor Dashboard | Biểu đồ lâm sàng, ghi chú tư vấn | ✅ Hoàn thành |
| 9 | GPS & Bản đồ | Google Maps, lịch sử 24h, chỉ đường | ✅ Hoàn thành |
| 10 | Báo cáo | PDF/Excel, tuần/tháng/năm | ✅ Hoàn thành |

**Chỉ số kỹ thuật đạt được:**

| Chỉ số | Mục tiêu | Kết quả | Đánh giá |
|---|---|---|---|
| Thời gian gửi SOS | < 3 giây | 2.1 giây | ✅ Vượt mục tiêu |
| Cold start time | < 3 giây | 2.4 giây | ✅ Đạt |
| RAM tối đa | < 150 MB | 112 MB | ✅ Đạt |
| Test coverage | ≥ 70% | 87% | ✅ Vượt mục tiêu |
| Must Have requirements | 57/57 | 57/57 | ✅ 100% |
| Should Have requirements | 22 | 18 | ✅ 82% |
| Điểm UAT trung bình | ≥ 4.0/5 | 4.4/5 | ✅ Vượt mục tiêu |

**Điểm nổi bật về UX cho người cao tuổi:**
- Font body tối thiểu 18sp, tiêu đề 22sp – vượt tiêu chuẩn WCAG AA.
- Nút SOS kích thước 80×80 dp, luôn hiển thị – người cao tuổi hoàn thành thao tác SOS trong 7 giây (mục tiêu 10 giây).
- TTS tiếng Việt với tốc độ 0.75x – được người cao tuổi đánh giá rõ ràng, dễ nghe.
- Hỗ trợ đầy đủ TalkBack cho người dùng suy giảm thị lực.

---

## 6.2 Hạn chế

Mặc dù đạt được nhiều kết quả tích cực, đồ án vẫn tồn tại một số hạn chế cần thừa nhận:

### 6.2.1 Hạn chế về tính năng

| STT | Hạn chế | Nguyên nhân | Đề xuất khắc phục |
|---|---|---|---|
| 1 | Dữ liệu sức khỏe nhập thủ công | Chưa tích hợp thiết bị đo | Kết nối Bluetooth BLE giai đoạn 2 |
| 2 | Chỉ hỗ trợ Android | Giới hạn thời gian đồ án | Phát triển iOS trong V1.5 |
| 3 | GPS không chính xác trong nhà | Hạn chế vật lý GPS | Kết hợp WiFi positioning |
| 4 | Không có chat/video call | Ngoài phạm vi V1.0 | Thêm trong V1.5 |
| 5 | Chưa có AI phát hiện té ngã | Cần dataset lớn | Nghiên cứu riêng V2.0 |

### 6.2.2 Hạn chế về kỹ thuật

- **Phụ thuộc Firebase:** Hệ thống phụ thuộc hoàn toàn vào hạ tầng Google Firebase. Nếu Firebase gặp sự cố, ứng dụng chỉ hoạt động offline một phần.
- **GPS tiêu thụ pin:** Dù đã tối ưu, việc cập nhật GPS background mỗi 15 phút vẫn ảnh hưởng đến thời lượng pin ở thiết bị cũ.
- **FCM trên thiết bị Trung Quốc:** Một số thiết bị Xiaomi/Oppo/Huawei có thể chặn FCM background; cần hướng dẫn người dùng cấu hình riêng.
- **Dữ liệu kiểm thử:** UAT thực hiện với mẫu nhỏ (5 người dùng) trong thời gian ngắn (2 tuần). Cần nghiên cứu người dùng quy mô lớn hơn để có đánh giá toàn diện.

### 6.2.3 Hạn chế về nghiên cứu

- Ngưỡng cảnh báo sức khỏe sử dụng giá trị chuẩn quốc tế, chưa được hiệu chỉnh theo đặc điểm người Việt Nam.
- Chưa có nghiên cứu so sánh định lượng với các giải pháp cạnh tranh hiện có.

---

## 6.3 Hướng phát triển

### 6.3.1 Lộ trình phát triển ngắn hạn (V1.5 – Q1/Q2 2027)

**Mở rộng nền tảng:**
- Phát triển ứng dụng iOS (Flutter code đã cross-platform, cần cấu hình iOS-specific và Apple Push Notification Service).
- Thêm chức năng chat text 1:1 giữa người cao tuổi và người thân qua Firestore Realtime.
- Thêm gọi video khẩn cấp 1 chạm bằng WebRTC/Agora SDK.

**Cải thiện tính năng hiện có:**
- Tích hợp ngưỡng cảnh báo tùy chỉnh theo chỉ định bác sĩ.
- Thêm báo cáo tự động gửi email hàng tuần.
- Cải thiện độ chính xác Shake Detection bằng mô hình phân loại tín hiệu.
- AI Health Tips: gợi ý sức khỏe cá nhân hóa theo xu hướng chỉ số.

### 6.3.2 Lộ trình phát triển trung hạn (V2.0 – Q3/Q4 2027)

**Tích hợp thiết bị thông minh:**
- Kết nối Smart Watch (Samsung Galaxy Watch, Xiaomi Band) qua Bluetooth BLE để tự động cập nhật nhịp tim và SpO2 mà không cần nhập thủ công.
- Tích hợp thiết bị đo huyết áp thông minh qua Bluetooth.

**AI Fall Detection:**
Phát triển module phát hiện té ngã tự động bằng thuật toán phân tích tín hiệu accelerometer:
```
Dấu hiệu té ngã:
1. Gia tốc đột ngột > 30 m/s² (impact phase)
2. Giai đoạn bất động > 2 giây sau impact (post-fall stillness)
→ Kích hoạt SOS tự động với countdown 15 giây
Mục tiêu: Sensitivity ≥ 90%, Specificity ≥ 95%
```

**Cảnh báo nguy cơ đột quỵ:**
Xây dựng rule-based engine kết hợp: huyết áp ≥ 180 mmHg liên tục + nhịp tim bất thường + SpO2 < 94% → cảnh báo khẩn cấp đặc biệt.

### 6.3.3 Lộ trình phát triển dài hạn (V3.0 – 2028+)

**Machine Learning cho phân tích sức khỏe:**
- Mô hình Time Series (LSTM) dự báo xu hướng chỉ số sức khỏe 7–30 ngày tới.
- Anomaly Detection phát hiện bất thường sớm trước khi xảy ra sự cố.
- Phân tầng nguy cơ: phân loại mức độ rủi ro sức khỏe cho từng bệnh nhân.

**Tích hợp hệ sinh thái:**
- Kết nối với Hospital EHR qua chuẩn HL7 FHIR R4.
- Hợp tác bảo hiểm sức khỏe: tích hợp dữ liệu để tối ưu phí bảo hiểm.
- Smart Home Integration: cảm biến cửa, camera AI, nút SOS vật lý không dây.

---

## 6.4 Kết luận

Đồ án tốt nghiệp **"GeriatricCare – Xây dựng hệ thống giám sát và chăm sóc người cao tuổi độc cư sử dụng công nghệ Flutter và Firebase"** đã hoàn thành các mục tiêu đề ra với kết quả đáng ghi nhận.

Về mặt học thuật, đồ án đã áp dụng thành công quy trình phân tích và thiết kế hệ thống hướng đối tượng theo chuẩn UML 2.5, từ việc xây dựng mô hình yêu cầu, mô hình lĩnh vực đến mô hình thiết kế và triển khai. Kiến trúc Clean Architecture được vận dụng đúng nguyên tắc, tạo ra codebase có tính module cao, dễ kiểm thử và bảo trì.

Về mặt thực tiễn, sản phẩm GeriatricCare đáp ứng 100% yêu cầu Must Have với 57 tính năng cốt lõi, đạt thời gian gửi SOS 2.1 giây – dưới ngưỡng 3 giây yêu cầu, test coverage 87% và điểm UAT 4.4/5. Giao diện Senior-first UX đã được người cao tuổi thực tế ghi nhận dễ sử dụng, đặc biệt là tính năng nhắc uống thuốc bằng giọng nói tiếng Việt.

Trước bối cảnh Việt Nam đang già hóa dân số nhanh, GeriatricCare không chỉ là một bài tập kỹ thuật mà còn mang giá trị xã hội thực sự: giúp người cao tuổi sống độc lập nhưng an toàn hơn, giúp người thân yên tâm hơn và hỗ trợ bác sĩ theo dõi bệnh nhân hiệu quả hơn. Đây chính là động lực lớn nhất để tiếp tục phát triển hệ thống trong các giai đoạn tiếp theo.

---

*Kết thúc Chương 6*
