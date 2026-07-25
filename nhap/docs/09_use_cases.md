# Chương 9: Use Cases
## GeriatricCare – Đặc tả Use Case

---

## 9.1 Use Case Diagram tổng quan (text representation)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         GeriatricCare System                                │
│                                                                             │
│  ┌──────────────┐    ┌─────────────────────────────────────────────┐        │
│  │   ELDERLY    │    │  UC-AUTH-01: Đăng ký                        │        │
│  │   (Người     │───>│  UC-AUTH-02: Đăng nhập                      │        │
│  │   cao tuổi)  │    │  UC-AUTH-03: Đăng xuất                      │        │
│  └──────┬───────┘    │  UC-AUTH-04: Quên mật khẩu                  │        │
│         │            └─────────────────────────────────────────────┘        │
│         │                                                                   │
│         │────────────>  UC-SOS-01: Kích hoạt Panic Button                  │
│         │────────────>  UC-SOS-02: Kích hoạt Shake SOS                     │
│         │────────────>  UC-HLT-01: Nhập chỉ số sức khỏe                   │
│         │────────────>  UC-MED-01: Xác nhận uống thuốc                     │
│         │────────────>  UC-PROF-01: Xem/cập nhật hồ sơ cá nhân            │
│         │────────────>  UC-SET-01: Cài đặt ứng dụng                        │
│                                                                             │
│  ┌──────────────┐                                                           │
│  │   FAMILY     │────>  UC-FAM-01: Xem Dashboard người thân                │
│  │   (Người     │────>  UC-FAM-02: Xem vị trí GPS                          │
│  │    thân)     │────>  UC-FAM-03: Nhận và xử lý SOS                       │
│  └──────┬───────┘────>  UC-MED-02: Tạo lịch uống thuốc                    │
│         │────────────>  UC-PROF-02: Quản lý hồ sơ người cao tuổi           │
│         │────────────>  UC-RPT-01: Xuất báo cáo                            │
│         │────────────>  UC-LINK-01: Liên kết người cao tuổi                │
│                                                                             │
│  ┌──────────────┐                                                           │
│  │   DOCTOR     │────>  UC-DOC-01: Xem Dashboard bác sĩ                    │
│  │   (Bác sĩ)   │────>  UC-DOC-02: Ghi chú khám bệnh                       │
│  └──────┬───────┘────>  UC-RPT-01: Xuất báo cáo                            │
│         │────────────>  UC-LINK-02: Liên kết bệnh nhân                     │
│                                                                             │
│  ┌──────────────┐                                                           │
│  │    ADMIN     │────>  UC-ADMIN-01: Quản lý người dùng                    │
│  │              │────>  UC-ADMIN-02: Xem thống kê hệ thống                 │
│  └──────────────┘────>  UC-ADMIN-03: Gửi thông báo hàng loạt               │
│                                                                             │
│  ┌──────────────┐                                                           │
│  │   Firebase   │<────  <<include>> Xác thực người dùng                    │
│  │   (External) │<────  <<include>> Gửi Push Notification                  │
│  │              │<────  <<include>> Lưu trữ dữ liệu                        │
│  └──────────────┘                                                           │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 9.2 UC-SOS-01: Kích hoạt Panic Button

| Thuộc tính | Chi tiết |
|---|---|
| **Use Case ID** | UC-SOS-01 |
| **Tên** | Kích hoạt SOS bằng Panic Button |
| **Actor chính** | Người cao tuổi (ELDERLY) |
| **Actor phụ** | Firebase FCM, SMS Service |
| **Mức độ** | Primary |
| **Preconditions** | 1. Người dùng đã đăng nhập<br>2. Ứng dụng đang chạy (foreground/background)<br>3. Có ít nhất 1 người thân được liên kết |
| **Postconditions thành công** | SOS được ghi log; thông báo gửi đến tất cả người thân; vị trí GPS được chia sẻ |
| **Postconditions thất bại** | SOS được lưu local; âm thanh báo động phát; thử gửi lại khi có mạng |

**Main Success Scenario:**

| Bước | Actor | Hành động |
|---|---|---|
| 1 | Người cao tuổi | Nhìn thấy nút SOS đỏ lớn trên màn hình chính |
| 2 | Người cao tuổi | Bấm vào nút SOS |
| 3 | System | Chuyển toàn màn hình sang giao diện đếm ngược (nền đỏ, số đếm lớn) |
| 4 | System | Đếm từ 5 xuống 0 với âm thanh beep nhẹ mỗi giây |
| 5 | Người cao tuổi | Không bấm HỦY |
| 6 | System | Gọi Geolocator.getCurrentPosition() lấy tọa độ GPS |
| 7 | System | Tạo document SOS trong Firestore: {id, userId, lat, lng, timestamp, status: "active"} |
| 8 | System | Gọi Firebase FCM gửi HIGH priority notification đến tất cả người thân |
| 9 | System | Gửi SMS qua SMS service đến người liên hệ khẩn cấp |
| 10 | System | Phát âm thanh còi báo động (AudioPlayer) |
| 11 | System | Hiển thị màn hình xác nhận: "Đã gửi cảnh báo khẩn cấp" + danh sách đã thông báo |
| 12 | System | Ghi Activity Log |

**Alternative Flows:**

*A1: Người dùng hủy trong đếm ngược*
- Bước 4a: Người cao tuổi bấm nút "HỦY"
- System: Hủy đếm ngược, trở về màn hình chính
- System: Ghi log "SOS_CANCELLED"

*A2: GPS không khả dụng*
- Bước 6a: Geolocator timeout sau 5 giây
- System: Tiếp tục gửi SOS không có tọa độ
- System: Thêm flag "location_unavailable: true" vào document SOS
- System: Notification gửi kèm ghi chú "Không xác định được vị trí"

*A3: Không có kết nối Internet*
- Bước 7a: Firestore offline → lưu vào pending queue
- System: Tiếp tục phát âm thanh báo động
- System: Hiển thị: "Đang gửi... Đã phát âm thanh báo động"
- System: Khi có mạng → tự động retry gửi SOS

**Exception Flows:**

*E1: FCM gửi thất bại*
- System: Thử lại tối đa 3 lần với interval 1s, 3s, 9s
- System: Nếu vẫn thất bại → gửi SMS dự phòng
- System: Ghi lỗi vào Crashlytics

**Business Rules áp dụng:** BR-SOS-01, BR-SOS-02, BR-SOS-03, BR-SOS-05, BR-SOS-06

---

## 9.3 UC-MED-02: Tạo lịch uống thuốc

| Thuộc tính | Chi tiết |
|---|---|
| **Use Case ID** | UC-MED-02 |
| **Tên** | Tạo lịch nhắc uống thuốc |
| **Actor chính** | Người thân (FAMILY) hoặc Bác sĩ (DOCTOR) |
| **Actor phụ** | Flutter Local Notifications |
| **Preconditions** | Đã đăng nhập; người cao tuổi đã tạo hồ sơ; Actor có quyền chỉnh sửa |

**Main Success Scenario:**

| Bước | Actor | Hành động |
|---|---|---|
| 1 | Người thân | Chọn menu "Thuốc" trong dashboard |
| 2 | Người thân | Chọn "Thêm thuốc mới" |
| 3 | System | Hiển thị form tạo lịch thuốc |
| 4 | Người thân | Điền: Tên thuốc, Liều lượng, Số lần/ngày |
| 5 | Người thân | Chọn thời điểm uống (time picker cho từng lần) |
| 6 | Người thân | Đặt ngày bắt đầu và kết thúc (tuỳ chọn) |
| 7 | Người thân | Thêm ghi chú (ví dụ: "Uống sau ăn") |
| 8 | Người thân | Nhấn "Lưu" |
| 9 | System | Validate dữ liệu |
| 10 | System | Lưu MedicationSchedule vào Firestore |
| 11 | System | Lên lịch Local Notification cho mỗi thời điểm uống |
| 12 | System | Hiển thị xác nhận thành công + preview lịch nhắc |

**Alternative Flows:**

*A1: Ngày kết thúc không điền*
- System: Tạo lịch nhắc không giới hạn thời gian (recurring indefinitely)

*A2: Chỉnh sửa lịch thuốc hiện có*
- Bước 2a: Chọn thuốc hiện có → nhấn "Chỉnh sửa"
- System: Hiện form với dữ liệu cũ đã điền sẵn
- Sau lưu: Cập nhật Firestore + reschedule notifications

**Business Rules áp dụng:** BR-MED-01, BR-MED-04, BR-MED-06

---

## 9.4 UC-HLT-01: Nhập chỉ số sức khỏe

| Thuộc tính | Chi tiết |
|---|---|
| **Use Case ID** | UC-HLT-01 |
| **Tên** | Nhập và lưu chỉ số sức khỏe |
| **Actor chính** | Người cao tuổi (ELDERLY) |
| **Actor phụ** | Firestore, FCM |
| **Preconditions** | Đã đăng nhập; có hồ sơ người cao tuổi |

**Main Success Scenario:**

| Bước | Actor | Hành động |
|---|---|---|
| 1 | Người cao tuổi | Chọn "Sức khỏe" → "Nhập chỉ số mới" |
| 2 | System | Hiển thị màn hình chọn loại chỉ số |
| 3 | Người cao tuổi | Chọn loại chỉ số (ví dụ: Huyết áp) |
| 4 | System | Hiển thị form nhập với bàn phím số lớn |
| 5 | Người cao tuổi | Nhập giá trị |
| 6 | System | Tự động điền ngày giờ hiện tại |
| 7 | Người cao tuổi | Thêm ghi chú (tuỳ chọn) |
| 8 | Người cao tuổi | Nhấn "Lưu" |
| 9 | System | Validate giá trị trong khoảng hợp lệ |
| 10 | System | Lưu vào Firestore (offline-first) |
| 11 | System | Kiểm tra ngưỡng cảnh báo |
| 12 | System (nếu bất thường) | Gửi FCM Health Alert đến người thân và bác sĩ |
| 13 | System | Cập nhật biểu đồ |
| 14 | System | Hiển thị xác nhận + nhận xét (Bình thường / Cần theo dõi / Cảnh báo) |

**Alternative Flows:**

*A1: Giá trị vượt ngưỡng cảnh báo*
- Bước 11a: System phát hiện giá trị vượt ngưỡng
- System: Hiển thị cảnh báo trực tiếp trên màn hình: "⚠️ Huyết áp cao. Nghỉ ngơi và theo dõi"
- System: Gửi Health Alert đến người thân

*A2: Đang offline*
- Bước 10a: Lưu vào Hive local database với flag "pending_sync"
- System: Hiển thị icon sync pending
- System: Khi có mạng → tự động sync với timestamp gốc

**Business Rules áp dụng:** BR-HLT-01, BR-HLT-02, BR-HLT-05, BR-HLT-06, BR-HLT-07

---

## 9.5 UC-FAM-01: Xem Dashboard người thân

| Thuộc tính | Chi tiết |
|---|---|
| **Use Case ID** | UC-FAM-01 |
| **Tên** | Xem Dashboard tổng quan người thân |
| **Actor chính** | Người thân (FAMILY) |
| **Actor phụ** | Firestore Realtime Listener |
| **Preconditions** | Đã đăng nhập; đã liên kết ít nhất 1 người cao tuổi |

**Main Success Scenario:**

| Bước | Actor | Hành động |
|---|---|---|
| 1 | Người thân | Mở ứng dụng |
| 2 | System | Tự động chuyển đến Family Dashboard (do vai trò FAMILY) |
| 3 | System | Khởi động Firestore realtime listeners cho tất cả người cao tuổi liên kết |
| 4 | System | Hiển thị danh sách card người cao tuổi |
| 5 | System | Mỗi card cập nhật realtime: online status, chỉ số sức khỏe, uống thuốc, pin |
| 6 | Người thân | Tap vào card một người cao tuổi |
| 7 | System | Chuyển sang màn hình chi tiết người cao tuổi đó |

**Alternative Flows:**

*A1: Chưa liên kết người cao tuổi nào*
- System: Hiển thị màn hình empty state với nút "Thêm người thân"

*A2: Có SOS đang active*
- System: Hiển thị banner SOS màu đỏ nổi bật ở đầu danh sách
- System: Rung thiết bị liên tục cho đến khi xác nhận

---

## 9.6 UC-DOC-01: Xem Dashboard bác sĩ

| Thuộc tính | Chi tiết |
|---|---|
| **Use Case ID** | UC-DOC-01 |
| **Tên** | Xem và phân tích dữ liệu sức khỏe bệnh nhân |
| **Actor chính** | Bác sĩ (DOCTOR) |
| **Preconditions** | Đã đăng nhập; đã liên kết ít nhất 1 bệnh nhân |

**Main Success Scenario:**

| Bước | Actor | Hành động |
|---|---|---|
| 1 | Bác sĩ | Mở ứng dụng → Doctor Dashboard |
| 2 | System | Hiển thị danh sách bệnh nhân đang theo dõi |
| 3 | Bác sĩ | Chọn bệnh nhân |
| 4 | System | Hiển thị tóm tắt sức khỏe: chỉ số mới nhất, tuân thủ thuốc |
| 5 | Bác sĩ | Chọn tab "Biểu đồ" |
| 6 | System | Render biểu đồ line chart theo thời gian |
| 7 | Bác sĩ | Chọn khoảng thời gian (Tuần/Tháng/3 tháng) |
| 8 | System | Cập nhật biểu đồ với dữ liệu trong khoảng chọn |
| 9 | Bác sĩ | Tap vào điểm bất thường (màu đỏ) |
| 10 | System | Hiển thị chi tiết: giá trị, thời gian, ghi chú |
| 11 | Bác sĩ | Chọn tab "Ghi chú" → thêm nhận xét |
| 12 | System | Lưu ghi chú kèm timestamp bác sĩ |

---

## 9.7 UC-LINK-01: Liên kết người cao tuổi với người thân

| Thuộc tính | Chi tiết |
|---|---|
| **Use Case ID** | UC-LINK-01 |
| **Tên** | Liên kết tài khoản người thân với người cao tuổi |
| **Actor chính** | Người thân (FAMILY) |
| **Actor phụ** | Người cao tuổi (ELDERLY) |
| **Preconditions** | Cả hai đều đã có tài khoản |

**Main Success Scenario:**

| Bước | Actor | Hành động |
|---|---|---|
| 1 | Người thân | Chọn "Thêm người thân" |
| 2 | System | Hiển thị 2 phương thức: "Nhập mã" hoặc "Quét QR" |
| 3 | Người cao tuổi | Mở cài đặt → "Chia sẻ mã liên kết" |
| 4 | System | Tạo mã 6 chữ số + QR code (hết hạn 24h) |
| 5 | Người thân | Nhập mã hoặc quét QR |
| 6 | System | Validate mã hợp lệ và chưa hết hạn |
| 7 | System | Gửi yêu cầu liên kết notification đến người cao tuổi |
| 8 | Người cao tuổi | Nhận notification → xem yêu cầu → Chấp nhận |
| 9 | System | Tạo link document trong Firestore |
| 10 | System | Người thân thấy người cao tuổi xuất hiện trong dashboard |

**Alternative Flows:**

*A1: Người cao tuổi từ chối*
- Bước 8a: Người cao tuổi nhấn "Từ chối"
- System: Thông báo cho người thân: "Yêu cầu liên kết bị từ chối"

**Business Rules áp dụng:** BR-LINK-01, BR-LINK-02, BR-LINK-04

---

## 9.8 UC-ADMIN-01: Quản lý người dùng

| Thuộc tính | Chi tiết |
|---|---|
| **Use Case ID** | UC-ADMIN-01 |
| **Tên** | Quản lý tài khoản người dùng |
| **Actor chính** | Admin |
| **Preconditions** | Đã đăng nhập với vai trò ADMIN |

**Main Success Scenario:**

| Bước | Actor | Hành động |
|---|---|---|
| 1 | Admin | Vào Admin Panel → Users |
| 2 | System | Hiển thị danh sách người dùng với filter: Vai trò / Trạng thái |
| 3 | Admin | Tìm kiếm theo tên hoặc email |
| 4 | System | Hiển thị kết quả lọc |
| 5 | Admin | Chọn người dùng → xem chi tiết |
| 6 | System | Hiển thị: thông tin, hoạt động gần nhất, số liên kết |
| 7 | Admin | Vô hiệu hóa tài khoản |
| 8 | System | Đặt trạng thái disabled trong Firestore + Firebase Auth |
| 9 | System | Ghi Audit Log |

---

## 9.9 Tóm tắt tất cả Use Cases

| ID | Tên | Actor | Priority |
|---|---|---|---|
| UC-AUTH-01 | Đăng ký tài khoản | All | Critical |
| UC-AUTH-02 | Đăng nhập | All | Critical |
| UC-AUTH-03 | Đăng xuất | All | High |
| UC-AUTH-04 | Quên mật khẩu | All | High |
| UC-PROF-01 | Xem/cập nhật hồ sơ cá nhân | Elderly | High |
| UC-PROF-02 | Quản lý hồ sơ người cao tuổi | Family | Critical |
| UC-SOS-01 | Kích hoạt Panic Button | Elderly | Critical |
| UC-SOS-02 | Kích hoạt Shake SOS | Elderly | Critical |
| UC-SOS-03 | Nhận và xử lý SOS | Family | Critical |
| UC-HLT-01 | Nhập chỉ số sức khỏe | Elderly/Family | Critical |
| UC-HLT-02 | Xem biểu đồ sức khỏe | Family/Doctor | High |
| UC-MED-01 | Xác nhận uống thuốc | Elderly | Critical |
| UC-MED-02 | Tạo lịch uống thuốc | Family/Doctor | Critical |
| UC-MED-03 | Xem lịch sử tuân thủ thuốc | Family | High |
| UC-FAM-01 | Xem Dashboard người thân | Family | Critical |
| UC-FAM-02 | Xem vị trí GPS | Family | High |
| UC-DOC-01 | Xem Dashboard bác sĩ | Doctor | High |
| UC-DOC-02 | Ghi chú khám bệnh | Doctor | High |
| UC-LINK-01 | Liên kết người cao tuổi – người thân | Family | Critical |
| UC-LINK-02 | Liên kết bác sĩ – bệnh nhân | Doctor | High |
| UC-RPT-01 | Xuất báo cáo PDF | Family/Doctor | Medium |
| UC-RPT-02 | Xuất báo cáo Excel | Family/Doctor | Low |
| UC-SET-01 | Cài đặt ứng dụng | All | High |
| UC-ADMIN-01 | Quản lý người dùng | Admin | High |
| UC-ADMIN-02 | Xem thống kê hệ thống | Admin | Medium |
| UC-ADMIN-03 | Gửi thông báo hàng loạt | Admin | Low |
