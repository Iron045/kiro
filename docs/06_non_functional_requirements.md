# Chương 6: Non-Functional Requirements
## GeriatricCare – Yêu cầu phi chức năng

---

## 6.1 NFR-PERF: Hiệu năng (Performance)

### 6.1.1 Thời gian phản hồi

| ID | Chỉ tiêu | Mục tiêu | Tối đa chấp nhận |
|---|---|---|---|
| NFR-PERF-01 | Khởi động ứng dụng lần đầu (cold start) | < 3 giây | < 5 giây |
| NFR-PERF-02 | Khởi động từ background (warm start) | < 1.5 giây | < 3 giây |
| NFR-PERF-03 | Gửi SOS từ lúc xác nhận → notification đến người thân | < 3 giây | < 5 giây |
| NFR-PERF-04 | Tải danh sách màn hình | < 1 giây | < 2 giây |
| NFR-PERF-05 | Render biểu đồ sức khỏe | < 2 giây | < 4 giây |
| NFR-PERF-06 | Lưu chỉ số sức khỏe vào Firestore | < 2 giây | < 5 giây |
| NFR-PERF-07 | Nhận push notification sau khi event xảy ra | < 5 giây | < 10 giây |
| NFR-PERF-08 | Xuất báo cáo PDF | < 5 giây | < 10 giây |

### 6.1.2 Tài nguyên thiết bị

| ID | Chỉ tiêu | Mục tiêu |
|---|---|---|
| NFR-PERF-09 | Mức sử dụng CPU khi idle | < 5% |
| NFR-PERF-10 | Mức sử dụng CPU khi active | < 30% |
| NFR-PERF-11 | RAM tối đa sử dụng | < 150 MB |
| NFR-PERF-12 | Tiêu thụ pin khi background (GPS off) | < 2%/giờ |
| NFR-PERF-13 | Tiêu thụ pin khi background (GPS on) | < 5%/giờ |
| NFR-PERF-14 | Dung lượng cài đặt ứng dụng | < 50 MB |
| NFR-PERF-15 | Cache dữ liệu offline | < 100 MB |

### 6.1.3 Tải hệ thống (Backend Firebase)

| ID | Chỉ tiêu | Mục tiêu |
|---|---|---|
| NFR-PERF-16 | Số người dùng đồng thời hỗ trợ | ≥ 1,000 |
| NFR-PERF-17 | Số SOS đồng thời xử lý | ≥ 100/phút |
| NFR-PERF-18 | Firestore reads/writes tối ưu | ≤ 50,000 reads/ngày/user |

---

## 6.2 NFR-SEC: Bảo mật (Security)

### 6.2.1 Xác thực & Phân quyền

| ID | Yêu cầu | Chi tiết |
|---|---|---|
| NFR-SEC-01 | Xác thực 2 yếu tố | Firebase Auth + Email OTP |
| NFR-SEC-02 | Phiên đăng nhập | Token hết hạn sau 30 ngày idle |
| NFR-SEC-03 | Khóa tài khoản | Sau 5 lần nhập sai trong 15 phút |
| NFR-SEC-04 | RBAC | 4 vai trò: ELDERLY / FAMILY / DOCTOR / ADMIN |
| NFR-SEC-05 | Firebase Security Rules | Mỗi user chỉ đọc/ghi dữ liệu của mình |

### 6.2.2 Mã hóa dữ liệu

| ID | Yêu cầu | Tiêu chuẩn |
|---|---|---|
| NFR-SEC-06 | Truyền dữ liệu | TLS 1.3 (HTTPS) |
| NFR-SEC-07 | Mật khẩu | Bcrypt hash (Firebase Auth tự xử lý) |
| NFR-SEC-08 | Dữ liệu nhạy cảm ở local | AES-256 |
| NFR-SEC-09 | Dữ liệu y tế trên Firestore | Mã hóa trường nhạy cảm |

### 6.2.3 Bảo vệ dữ liệu

| ID | Yêu cầu | Chi tiết |
|---|---|---|
| NFR-SEC-10 | Không lưu mật khẩu plain text | Tuyệt đối không |
| NFR-SEC-11 | Không log thông tin nhạy cảm | Email, số điện thoại không log rõ |
| NFR-SEC-12 | API Key bảo vệ | Dùng Firebase Remote Config, không hard-code |
| NFR-SEC-13 | Certificate Pinning | Chống man-in-the-middle |
| NFR-SEC-14 | Root/Jailbreak Detection | Cảnh báo khi phát hiện thiết bị rooted |
| NFR-SEC-15 | Audit Log | Ghi lại mọi hành động nhạy cảm |

### 6.2.4 OWASP Mobile Top 10 Compliance

| OWASP Risk | Biện pháp |
|---|---|
| M1: Improper Platform Usage | Dùng đúng Permission, không yêu cầu quyền thừa |
| M2: Insecure Data Storage | Mã hóa SharedPreferences, không lưu token dạng plain |
| M3: Insecure Communication | TLS 1.3, Certificate Pinning |
| M4: Insecure Authentication | Firebase Auth, token refresh |
| M5: Insufficient Cryptography | AES-256, không tự implement crypto |
| M6: Insecure Authorization | Firebase Security Rules nghiêm ngặt |
| M7: Client Code Quality | Lint, static analysis, code review |
| M8: Code Tampering | Play Integrity API |
| M9: Reverse Engineering | ProGuard/R8 obfuscation |
| M10: Extraneous Functionality | Remove debug code trước production |

---

## 6.3 NFR-AVAIL: Độ sẵn sàng (Availability)

| ID | Yêu cầu | Mục tiêu |
|---|---|---|
| NFR-AVAIL-01 | Uptime hệ thống | ≥ 99.9% (< 8.7 giờ downtime/năm) |
| NFR-AVAIL-02 | Uptime tính năng SOS | ≥ 99.99% |
| NFR-AVAIL-03 | Thời gian khôi phục sự cố (RTO) | < 1 giờ |
| NFR-AVAIL-04 | Điểm khôi phục dữ liệu (RPO) | < 15 phút |
| NFR-AVAIL-05 | Hoạt động offline | Nhập sức khỏe, nhắc thuốc hoạt động không cần mạng |
| NFR-AVAIL-06 | Đồng bộ khi có mạng lại | Tự động sync dữ liệu offline |

---

## 6.4 NFR-SCALE: Khả năng mở rộng (Scalability)

| ID | Yêu cầu |
|---|---|
| NFR-SCALE-01 | Kiến trúc Firebase tự động scale theo lượng người dùng |
| NFR-SCALE-02 | Firestore có thể xử lý hàng triệu documents |
| NFR-SCALE-03 | Code base Flutter dễ thêm tính năng mới (Clean Architecture) |
| NFR-SCALE-04 | Hỗ trợ mở rộng sang iOS trong giai đoạn 2 mà không cần viết lại |
| NFR-SCALE-05 | Module hoá để thêm AI features về sau |

---

## 6.5 NFR-ACCESS: Khả năng tiếp cận (Accessibility)

### 6.5.1 Yêu cầu giao diện người cao tuổi

| ID | Yêu cầu | Tiêu chuẩn |
|---|---|---|
| NFR-ACCESS-01 | Kích thước font tối thiểu | ≥ 18sp (body text) |
| NFR-ACCESS-02 | Kích thước font tiêu đề | ≥ 22sp |
| NFR-ACCESS-03 | Kích thước nút tối thiểu | 48×48 dp |
| NFR-ACCESS-04 | Nút SOS | ≥ 80×80 dp |
| NFR-ACCESS-05 | Contrast ratio | ≥ 4.5:1 (WCAG AA) |
| NFR-ACCESS-06 | Khoảng cách giữa các phần tử tương tác | ≥ 8dp |

### 6.5.2 Hỗ trợ giọng nói

| ID | Yêu cầu |
|---|---|
| NFR-ACCESS-07 | TTS tiếng Việt cho nhắc uống thuốc (flutter_tts) |
| NFR-ACCESS-08 | TTS giọng đọc nội dung màn hình (TalkBack support) |
| NFR-ACCESS-09 | Semantic labels cho tất cả widget tương tác |

### 6.5.3 Dark Mode & Theme

| ID | Yêu cầu |
|---|---|
| NFR-ACCESS-10 | Hỗ trợ Dark Mode |
| NFR-ACCESS-11 | Tùy chỉnh cỡ chữ (Small / Normal / Large / Extra Large) |
| NFR-ACCESS-12 | Tùy chỉnh contrast (Normal / High Contrast) |

---

## 6.6 NFR-REL: Độ tin cậy (Reliability)

| ID | Yêu cầu | Chi tiết |
|---|---|---|
| NFR-REL-01 | Crash rate | < 0.1% sessions |
| NFR-REL-02 | ANR rate | < 0.05% |
| NFR-REL-03 | SOS không bao giờ thất bại im lặng | Phải có fallback (âm báo + SMS) |
| NFR-REL-04 | Nhắc thuốc không mất khi restart máy | Dùng AlarmManager persistent |
| NFR-REL-05 | Dữ liệu sức khỏe không mất khi offline | Local cache + sync queue |
| NFR-REL-06 | Firebase offline persistence | Bật Firestore offline |

---

## 6.7 NFR-MAINT: Khả năng bảo trì (Maintainability)

| ID | Yêu cầu | Chi tiết |
|---|---|---|
| NFR-MAINT-01 | Kiến trúc Clean Architecture | Domain / Data / Presentation tách biệt |
| NFR-MAINT-02 | SOLID Principles | Áp dụng toàn bộ codebase |
| NFR-MAINT-03 | Code coverage test | ≥ 70% (unit test) |
| NFR-MAINT-04 | Tài liệu code | Dartdoc cho tất cả public API |
| NFR-MAINT-05 | Linting | flutter_lints, custom analysis_options.yaml |
| NFR-MAINT-06 | Dependency Injection | GetIt + Injectable |
| NFR-MAINT-07 | Error monitoring | Firebase Crashlytics |
| NFR-MAINT-08 | Performance monitoring | Firebase Performance |
| NFR-MAINT-09 | Remote Config | Firebase Remote Config cho feature flags |

---

## 6.8 NFR-PRIV: Quyền riêng tư (Privacy)

| ID | Yêu cầu | Chi tiết |
|---|---|---|
| NFR-PRIV-01 | Chính sách bảo mật rõ ràng | Hiển thị khi đăng ký |
| NFR-PRIV-02 | Người dùng đồng ý trước khi thu thập GPS | Explicit consent |
| NFR-PRIV-03 | Người cao tuổi biết khi vị trí đang được chia sẻ | Icon GPS trên status bar |
| NFR-PRIV-04 | Người dùng có thể xóa dữ liệu cá nhân | Right to be forgotten |
| NFR-PRIV-05 | Dữ liệu y tế chỉ chia sẻ với người được ủy quyền | RBAC + consent |
| NFR-PRIV-06 | Không chia sẻ dữ liệu với bên thứ ba | Trừ Firebase (Google) |

---

## 6.9 NFR-COMPAT: Tương thích (Compatibility)

| ID | Yêu cầu | Chi tiết |
|---|---|---|
| NFR-COMPAT-01 | Android version | Android 8.0+ (API 26+) |
| NFR-COMPAT-02 | Màn hình | 4.7" đến 7" |
| NFR-COMPAT-03 | Độ phân giải | hdpi, xhdpi, xxhdpi, xxxhdpi |
| NFR-COMPAT-04 | RAM tối thiểu | 2 GB |
| NFR-COMPAT-05 | Bộ nhớ trống tối thiểu | 100 MB |
| NFR-COMPAT-06 | Ngôn ngữ | Tiếng Việt (chính), Tiếng Anh (phụ) |

---

## 6.10 NFR-DR: Phục hồi thảm họa (Disaster Recovery)

| ID | Yêu cầu | Chi tiết |
|---|---|---|
| NFR-DR-01 | Backup Firestore | Firebase tự động backup hàng ngày |
| NFR-DR-02 | Firebase Storage backup | Cross-region replication |
| NFR-DR-03 | Quy trình khôi phục dữ liệu | Tài liệu hóa, test 3 tháng/lần |
| NFR-DR-04 | Fallback khi Firebase lỗi | Hiển thị dữ liệu cache local |
| NFR-DR-05 | SOS fallback | SMS khi FCM không khả dụng |

---

## 6.11 Bảng tổng hợp NFR theo độ ưu tiên

| Nhóm | Tầm quan trọng | Lý do |
|---|---|---|
| Performance (SOS) | 🔴 Critical | Ảnh hưởng tính mạng |
| Security | 🔴 Critical | Dữ liệu y tế nhạy cảm |
| Reliability (SOS, Medication) | 🔴 Critical | Ảnh hưởng tính mạng |
| Accessibility | 🔴 High | Người dùng chính là người cao tuổi |
| Availability | 🔴 High | Cần 24/7 |
| Privacy | 🟡 High | Quy định pháp lý |
| Maintainability | 🟡 Medium | Phát triển lâu dài |
| Scalability | 🟡 Medium | Tăng trưởng người dùng |
| Compatibility | 🟡 Medium | Android đa dạng |
| Disaster Recovery | 🟠 Medium | Rủi ro mất dữ liệu |
