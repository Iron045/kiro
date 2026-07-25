# GIAI ĐOẠN 2: PHÂN TÍCH YÊU CẦU
## GeriatricCare – Hệ thống Giám sát & Chăm sóc Người cao tuổi Độc cư

**Phiên bản:** 1.0.0 | **Ngày:** 2026-07-25 | **Trạng thái:** Hoàn chỉnh

---

## 1. MỤC TIÊU GIAI ĐOẠN

Giai đoạn này xác định:
- Toàn bộ yêu cầu chức năng (FR) và phi chức năng (NFR)
- Phân loại và ưu tiên theo MoSCoW
- User Stories theo chuẩn Agile
- Use Cases đầy đủ luồng chính / phụ / ngoại lệ
- Business Rules ràng buộc nghiệp vụ
- Ma trận truy vết yêu cầu (RTM)

---

## 2. PHÂN TÍCH YÊU CẦU CHỨC NĂNG (FUNCTIONAL REQUIREMENTS)

### 2.1 Sơ đồ tổng quan chức năng

```
┌─────────────────────────────────────────────────────────────────────┐
│                    GERIATRICCARE – FUNCTION MAP                     │
├───────────────┬───────────────┬──────────────┬──────────────────────┤
│  AUTH MODULE  │ PROFILE MODULE│  SOS MODULE  │  MEDICATION MODULE   │
│               │               │              │                      │
│ • Đăng ký     │ • Hồ sơ NCT  │ • Panic Btn  │ • Tạo lịch thuốc    │
│ • Đăng nhập   │ • Hồ sơ BS   │ • Shake Det. │ • Thông báo + TTS   │
│ • OTP verify  │ • Hồ sơ NT   │ • Đếm ngược  │ • Xác nhận uống     │
│ • Quên MK     │ • Liên kết   │ • Gửi GPS    │ • Cảnh báo bỏ lỡ   │
│ • Đăng xuất   │ • Phân quyền │ • FCM Alert  │ • Lịch sử thuốc     │
├───────────────┼───────────────┼──────────────┼──────────────────────┤
│ HEALTH MODULE │  DASHBOARD    │  LOCATION    │  REPORT MODULE       │
│               │               │              │                      │
│ • Nhập chỉ số │ • Gia đình   │ • GPS track  │ • Xuất PDF          │
│ • Biểu đồ    │ • Bác sĩ     │ • Google Map │ • Xuất Excel         │
│ • Lịch sử    │ • Real-time  │ • Lịch sử vị │ • Tuần/Tháng/Năm    │
│ • Cảnh báo   │ • Cảnh báo   │   trí        │ • Thống kê sức khỏe  │
│   bất thường │ • Pin status │ • Điều hướng │                      │
├───────────────┼───────────────┼──────────────┼──────────────────────┤
│ NOTIFICATION  │ ACTIVITY LOG  │  SETTINGS    │  ADMIN MODULE        │
│               │               │              │                      │
│ • FCM push    │ • Đăng nhập  │ • Font size  │ • Quản lý user      │
│ • Local notif │ • SOS events │ • Dark mode  │ • Thống kê hệ thống │
│ • SOS alert   │ • Uống thuốc │ • Ngôn ngữ  │ • Cấu hình alert    │
│ • Thuốc alert │ • Đo sức KH  │ • Shake sens │ • Audit log         │
└───────────────┴───────────────┴──────────────┴──────────────────────┘
```

### 2.2 Danh sách yêu cầu chức năng đầy đủ

#### Module 1 – Authentication (FR-AUTH)

| ID | Tên chức năng | Mô tả | Actor | Ưu tiên |
|----|--------------|-------|-------|---------|
| FR-AUTH-01 | Đăng ký tài khoản | Người dùng tạo tài khoản bằng SĐT + mật khẩu | Tất cả | Must |
| FR-AUTH-02 | Xác thực OTP | Gửi mã OTP 6 số qua SMS, hết hạn sau 5 phút | Tất cả | Must |
| FR-AUTH-03 | Đăng nhập | Đăng nhập bằng SĐT + mật khẩu hoặc Google | Tất cả | Must |
| FR-AUTH-04 | Quên mật khẩu | Đặt lại mật khẩu qua OTP | Tất cả | Must |
| FR-AUTH-05 | Đăng xuất | Xóa session, hủy FCM token | Tất cả | Must |
| FR-AUTH-06 | Chọn vai trò | Sau đăng ký: chọn NCT / Người thân / Bác sĩ | Tất cả | Must |
| FR-AUTH-07 | Tự động đăng nhập | Duy trì session qua Firebase Auth token | Tất cả | Must |

#### Module 2 – Quản lý hồ sơ (FR-PROFILE)

| ID | Tên chức năng | Mô tả | Actor | Ưu tiên |
|----|--------------|-------|-------|---------|
| FR-PRF-01 | Tạo hồ sơ NCT | Nhập đầy đủ thông tin cá nhân, bệnh nền, dị ứng | NCT, Người thân | Must |
| FR-PRF-02 | Cập nhật hồ sơ NCT | Chỉnh sửa thông tin hồ sơ | NCT, Người thân | Must |
| FR-PRF-03 | Tạo hồ sơ bác sĩ | Nhập chuyên khoa, bệnh viện, SĐT | Bác sĩ | Must |
| FR-PRF-04 | Liên kết Người thân ↔ NCT | Tạo mối quan hệ qua mã chia sẻ | Người thân | Must |
| FR-PRF-05 | Liên kết Bác sĩ ↔ NCT | Bác sĩ theo dõi bệnh nhân sau khi được duyệt | Bác sĩ | Should |
| FR-PRF-06 | Xem danh sách NCT quản lý | Người thân/BS xem danh sách NCT được liên kết | Người thân, BS | Must |
| FR-PRF-07 | Upload ảnh đại diện | Tải ảnh lên Firebase Storage | Tất cả | Should |
| FR-PRF-08 | Quản lý liên hệ khẩn cấp | Thêm/sửa/xóa danh sách người liên hệ khi SOS | NCT, Người thân | Must |

#### Module 3 – SOS & Panic Button (FR-SOS)

| ID | Tên chức năng | Mô tả | Actor | Ưu tiên |
|----|--------------|-------|-------|---------|
| FR-SOS-01 | Nút SOS luôn hiển thị | Nút SOS lớn, màu đỏ, trên màn hình chính NCT | NCT | Must |
| FR-SOS-02 | Kích hoạt SOS thủ công | Bấm nút → hiện đếm ngược 5 giây có thể hủy | NCT | Must |
| FR-SOS-03 | Shake Detection | Lắc điện thoại mạnh → tự động kích hoạt SOS | NCT | Must |
| FR-SOS-04 | Đếm ngược và hủy | 5 giây đếm ngược, NCT có thể hủy để tránh nhầm | NCT | Must |
| FR-SOS-05 | Lấy GPS location | Lấy tọa độ GPS hiện tại khi SOS kích hoạt | Hệ thống | Must |
| FR-SOS-06 | Gửi FCM notification | Gửi push notification đến tất cả người thân liên kết | Hệ thống | Must |
| FR-SOS-07 | Phát còi báo động | Kích hoạt âm thanh cảnh báo trên thiết bị NCT | Hệ thống | Must |
| FR-SOS-08 | Lưu lịch sử SOS | Ghi lại thời gian, vị trí, trạng thái của mỗi SOS | Hệ thống | Must |
| FR-SOS-09 | Cấu hình độ nhạy Shake | Cho phép điều chỉnh ngưỡng gia tốc kích hoạt | NCT, Người thân | Should |
| FR-SOS-10 | Xem lịch sử SOS | Danh sách SOS theo thời gian | Người thân, BS | Must |

#### Module 4 – Nhắc uống thuốc (FR-MED)

| ID | Tên chức năng | Mô tả | Actor | Ưu tiên |
|----|--------------|-------|-------|---------|
| FR-MED-01 | Tạo lịch thuốc | Nhập tên, liều lượng, số lần, giờ uống, ghi chú | Người thân, NCT | Must |
| FR-MED-02 | Thông báo đến giờ uống | Push notification + local notification đúng giờ | Hệ thống | Must |
| FR-MED-03 | Đọc giọng nói tiếng Việt | TTS đọc "Đến giờ uống thuốc [tên thuốc]" | Hệ thống | Must |
| FR-MED-04 | Xác nhận đã uống | NCT bấm "Đã uống" → ghi log, thông báo người thân | NCT | Must |
| FR-MED-05 | Bỏ qua / Nhắc lại | NCT bỏ qua hoặc yêu cầu nhắc lại sau N phút | NCT | Should |
| FR-MED-06 | Cảnh báo bỏ lỡ | Nếu sau 30 phút không xác nhận → gửi cảnh báo NT | Hệ thống | Must |
| FR-MED-07 | Lịch sử uống thuốc | Xem tuân thủ theo ngày/tuần, tỷ lệ % | NCT, Người thân, BS | Must |
| FR-MED-08 | Chỉnh sửa lịch thuốc | Cập nhật hoặc tạm dừng lịch thuốc | Người thân, NCT | Must |
| FR-MED-09 | Nhắc trước N phút | Cho phép cấu hình thời gian nhắc trước | Người thân | Should |
