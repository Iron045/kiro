# Chương 4: Requirement Analysis
## GeriatricCare – Phân tích yêu cầu

---

## 4.1 Phương pháp thu thập yêu cầu

| Phương pháp | Đối tượng | Mục tiêu |
|---|---|---|
| Phỏng vấn | Người cao tuổi, người thân | Hiểu pain points thực tế |
| Workshop | Product Owner, Dev Team | Xác định scope và ưu tiên |
| Focus Group | Bác sĩ, điều dưỡng | Clinical requirements |
| Phân tích cạnh tranh | - | Benchmark tính năng |
| Document Analysis | Văn bản y tế, pháp lý | Quy định và tiêu chuẩn |

---

## 4.2 Danh sách yêu cầu tổng hợp

### 4.2.1 Yêu cầu chức năng theo Module

#### Module 1: Xác thực (Authentication)

| ID | Yêu cầu | Ưu tiên | Nguồn |
|---|---|---|---|
| REQ-AUTH-01 | Đăng ký bằng email + mật khẩu | Must Have | Product Owner |
| REQ-AUTH-02 | Đăng nhập bằng email + mật khẩu | Must Have | Product Owner |
| REQ-AUTH-03 | Xác thực OTP qua SMS/Email | Must Have | Security |
| REQ-AUTH-04 | Quên mật khẩu / Reset mật khẩu | Must Have | UX |
| REQ-AUTH-05 | Đăng xuất | Must Have | Security |
| REQ-AUTH-06 | Tự động đăng nhập lại (Remember Me) | Should Have | UX |
| REQ-AUTH-07 | Phân quyền theo vai trò (RBAC) | Must Have | Security |
| REQ-AUTH-08 | Quản lý phiên đăng nhập | Must Have | Security |

#### Module 2: Hồ sơ (Profile)

| ID | Yêu cầu | Ưu tiên | Nguồn |
|---|---|---|---|
| REQ-PROF-01 | Tạo/cập nhật hồ sơ người cao tuổi | Must Have | Core |
| REQ-PROF-02 | Thêm nhiều người cao tuổi vào một tài khoản gia đình | Must Have | Family |
| REQ-PROF-03 | Tạo hồ sơ bác sĩ / điều dưỡng | Must Have | Doctor |
| REQ-PROF-04 | Liên kết bác sĩ với người cao tuổi | Must Have | Doctor |
| REQ-PROF-05 | Quản lý danh sách người chăm sóc | Must Have | Family |
| REQ-PROF-06 | Upload ảnh đại diện | Should Have | UX |
| REQ-PROF-07 | Thông tin dị ứng thuốc | Must Have | Medical |
| REQ-PROF-08 | Người liên hệ khẩn cấp | Must Have | Emergency |

#### Module 3: SOS / Panic Button

| ID | Yêu cầu | Ưu tiên | Nguồn |
|---|---|---|---|
| REQ-SOS-01 | Nút SOS lớn luôn hiển thị màn hình chính | Must Have | Safety |
| REQ-SOS-02 | Đếm ngược 5 giây trước khi gửi (có thể hủy) | Must Have | UX |
| REQ-SOS-03 | Lấy GPS tự động khi kích hoạt SOS | Must Have | Safety |
| REQ-SOS-04 | Gửi push notification đến tất cả người thân | Must Have | Safety |
| REQ-SOS-05 | Gửi SMS khẩn cấp | Should Have | Safety |
| REQ-SOS-06 | Phát âm thanh báo động | Must Have | Safety |
| REQ-SOS-07 | Lưu lịch sử SOS | Must Have | Log |
| REQ-SOS-08 | Xem lịch sử SOS | Must Have | Log |
| REQ-SOS-09 | Kích hoạt SOS bằng lắc điện thoại | Must Have | Safety |
| REQ-SOS-10 | Cấu hình độ nhạy lắc | Should Have | UX |
| REQ-SOS-11 | Tắt/bật Shake Detection | Must Have | Control |

#### Module 4: Theo dõi sức khỏe (Health Tracking)

| ID | Yêu cầu | Ưu tiên | Nguồn |
|---|---|---|---|
| REQ-HLT-01 | Nhập chỉ số huyết áp (SYS/DIA/Pulse) | Must Have | Medical |
| REQ-HLT-02 | Nhập chỉ số đường huyết | Must Have | Medical |
| REQ-HLT-03 | Nhập nhịp tim | Must Have | Medical |
| REQ-HLT-04 | Nhập nhiệt độ cơ thể | Must Have | Medical |
| REQ-HLT-05 | Nhập cân nặng | Must Have | Medical |
| REQ-HLT-06 | Nhập SpO2 | Must Have | Medical |
| REQ-HLT-07 | Lưu lịch sử tất cả chỉ số | Must Have | Medical |
| REQ-HLT-08 | Biểu đồ theo ngày/tuần/tháng/năm | Must Have | Analytics |
| REQ-HLT-09 | Cảnh báo khi chỉ số vượt ngưỡng | Must Have | Safety |
| REQ-HLT-10 | Cho phép bác sĩ xem lịch sử | Must Have | Doctor |
| REQ-HLT-11 | Ngưỡng cảnh báo cấu hình được | Should Have | Medical |

#### Module 5: Nhắc uống thuốc (Medication Reminder)

| ID | Yêu cầu | Ưu tiên | Nguồn |
|---|---|---|---|
| REQ-MED-01 | Tạo lịch uống thuốc với thông tin đầy đủ | Must Have | Medical |
| REQ-MED-02 | Cấu hình thời gian nhắc nhở | Must Have | Medical |
| REQ-MED-03 | Push notification đến giờ uống thuốc | Must Have | Medical |
| REQ-MED-04 | Đọc tên thuốc bằng giọng nói tiếng Việt (TTS) | Must Have | Accessibility |
| REQ-MED-05 | Xác nhận "Đã uống" hoặc "Bỏ qua" | Must Have | Tracking |
| REQ-MED-06 | Cảnh báo người thân nếu quá giờ không xác nhận | Must Have | Safety |
| REQ-MED-07 | Lịch sử tuân thủ uống thuốc | Must Have | Analytics |
| REQ-MED-08 | Tỷ lệ tuân thủ theo ngày/tuần/tháng | Should Have | Analytics |
| REQ-MED-09 | Tắt/bật từng lịch nhắc | Must Have | Control |
| REQ-MED-10 | Nhắc thuốc lặp lại nếu không phản hồi | Should Have | Safety |

#### Module 6: Dashboard người thân (Family Dashboard)

| ID | Yêu cầu | Ưu tiên | Nguồn |
|---|---|---|---|
| REQ-FAM-01 | Danh sách người cao tuổi đang theo dõi | Must Have | Core |
| REQ-FAM-02 | Trạng thái online/offline | Must Have | Monitoring |
| REQ-FAM-03 | Lần cập nhật dữ liệu gần nhất | Must Have | Monitoring |
| REQ-FAM-04 | Chỉ số sức khỏe mới nhất | Must Have | Health |
| REQ-FAM-05 | Trạng thái uống thuốc hôm nay | Must Have | Medication |
| REQ-FAM-06 | Cảnh báo SOS gần đây | Must Have | Safety |
| REQ-FAM-07 | Mức pin điện thoại của người cao tuổi | Should Have | Monitoring |
| REQ-FAM-08 | Vị trí GPS trên bản đồ | Must Have | Location |
| REQ-FAM-09 | Nhận push notification khi có sự kiện | Must Have | Alert |
| REQ-FAM-10 | Lịch sử hoạt động của người cao tuổi | Should Have | Log |

#### Module 7: Dashboard bác sĩ (Doctor Dashboard)

| ID | Yêu cầu | Ưu tiên | Nguồn |
|---|---|---|---|
| REQ-DOC-01 | Danh sách bệnh nhân đang theo dõi | Must Have | Core |
| REQ-DOC-02 | Biểu đồ sức khỏe chi tiết | Must Have | Clinical |
| REQ-DOC-03 | Lịch sử đo tất cả chỉ số | Must Have | Clinical |
| REQ-DOC-04 | Cảnh báo chỉ số bất thường | Must Have | Clinical |
| REQ-DOC-05 | Ghi chú khám bệnh / tư vấn | Must Have | Clinical |
| REQ-DOC-06 | Xuất báo cáo PDF | Must Have | Reporting |

#### Module 8: GPS & Bản đồ

| ID | Yêu cầu | Ưu tiên | Nguồn |
|---|---|---|---|
| REQ-GPS-01 | Hiển thị vị trí hiện tại trên Google Maps | Must Have | Location |
| REQ-GPS-02 | Lưu lịch sử vị trí | Should Have | Location |
| REQ-GPS-03 | Chia sẻ vị trí với người thân | Must Have | Safety |
| REQ-GPS-04 | Điều hướng đến vị trí người cao tuổi | Should Have | Safety |
| REQ-GPS-05 | Cập nhật vị trí định kỳ khi background | Should Have | Monitoring |

#### Module 9: Thông báo (Notification)

| ID | Yêu cầu | Ưu tiên | Nguồn |
|---|---|---|---|
| REQ-NOT-01 | Push notification qua FCM | Must Have | Core |
| REQ-NOT-02 | Local notification cho nhắc thuốc | Must Have | Medication |
| REQ-NOT-03 | Notification SOS ưu tiên cao | Must Have | Safety |
| REQ-NOT-04 | Cấu hình loại thông báo muốn nhận | Should Have | UX |
| REQ-NOT-05 | Badge số lượng thông báo chưa đọc | Should Have | UX |

#### Module 10: Báo cáo (Report)

| ID | Yêu cầu | Ưu tiên | Nguồn |
|---|---|---|---|
| REQ-RPT-01 | Xuất báo cáo sức khỏe PDF | Must Have | Reporting |
| REQ-RPT-02 | Xuất báo cáo Excel | Should Have | Reporting |
| REQ-RPT-03 | Báo cáo theo tuần/tháng/năm | Must Have | Reporting |
| REQ-RPT-04 | Báo cáo tuân thủ uống thuốc | Must Have | Reporting |
| REQ-RPT-05 | Chia sẻ báo cáo qua email | Should Have | Reporting |

---

## 4.3 Phân loại yêu cầu theo MoSCoW

| Loại | Số lượng | Mô tả |
|---|---|---|
| Must Have | 52 | Bắt buộc có trong v1.0 |
| Should Have | 18 | Nên có, ưu tiên cao sau Must Have |
| Could Have | 8 | Có thể có nếu còn thời gian |
| Won't Have | 5 | Không làm trong v1.0 |

---

## 4.4 Dependency Map

```
Authentication ──→ Profile Management
     │
     └──→ Role-Based Access Control ──→ All Features

SOS Module ──→ GPS Module
     │
     └──→ Notification Module
     │
     └──→ Activity Log

Medication Module ──→ Notification Module
     │
     └──→ Activity Log

Health Tracking ──→ Charts/Analytics
     │
     └──→ Doctor Dashboard
     │
     └──→ Report Module

Family Dashboard ──→ GPS Module
     │
     └──→ Notification Module
     │
     └──→ Health Tracking
```

---

## 4.5 Acceptance Criteria tổng quan

| Tiêu chí | Mô tả |
|---|---|
| Performance | App khởi động < 3 giây |
| SOS Response | SOS gửi trong < 3 giây |
| Notification Delivery | Push notification đến trong < 5 giây |
| Offline Capability | Nhập sức khỏe hoạt động offline |
| Accessibility | Font ≥ 18sp, contrast ratio ≥ 4.5:1 |
| Security | Mã hóa dữ liệu nhạy cảm, RBAC |
| Usability | Người cao tuổi hoàn thành SOS trong < 10 giây |
