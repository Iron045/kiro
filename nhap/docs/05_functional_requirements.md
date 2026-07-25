# Chương 5: Functional Requirements
## GeriatricCare – Đặc tả yêu cầu chức năng

---

## 5.1 FR-AUTH: Xác thực & Phân quyền

---

### FR-AUTH-01: Đăng ký tài khoản

| Thuộc tính | Nội dung |
|---|---|
| **ID** | FR-AUTH-01 |
| **Tên** | Đăng ký tài khoản |
| **Mô tả** | Người dùng tạo tài khoản mới bằng email và mật khẩu, chọn vai trò phù hợp |
| **Actor** | Người cao tuổi, Người thân, Bác sĩ |
| **Preconditions** | Người dùng chưa có tài khoản; có kết nối Internet |

**Main Flow:**
1. Người dùng mở ứng dụng lần đầu
2. Chọn "Đăng ký"
3. Nhập: Họ tên, Email, Mật khẩu, Xác nhận mật khẩu
4. Chọn vai trò: [Người cao tuổi / Người thân / Bác sĩ]
5. Đọc và đồng ý Điều khoản sử dụng
6. Nhấn "Đăng ký"
7. Hệ thống gửi email xác thực
8. Người dùng mở email, xác thực
9. Chuyển sang màn hình hoàn thiện hồ sơ

**Alternative Flow:**
- A1: Email đã tồn tại → Hiển thị thông báo, gợi ý "Đăng nhập"
- A2: Đăng ký thành công nhưng chưa xác thực email → Cho phép dùng ứng dụng, nhắc xác thực

**Exception Flow:**
- E1: Mất mạng khi đăng ký → Lưu tạm thông tin, thông báo thử lại
- E2: Firebase Auth lỗi → Hiển thị thông báo lỗi kỹ thuật

**Validation Rules:**
- Email: định dạng hợp lệ, không trùng
- Mật khẩu: ≥ 8 ký tự, có chữ hoa, số, ký tự đặc biệt
- Họ tên: 2–100 ký tự, không chứa ký tự đặc biệt

**Business Rules:**
- BR-AUTH-01: Mỗi email chỉ tạo 1 tài khoản
- BR-AUTH-02: Mật khẩu không được lưu dạng plain text

**Postconditions:** Tài khoản được tạo, người dùng được chuyển đến màn hình hoàn thiện hồ sơ

---

### FR-AUTH-02: Đăng nhập

| Thuộc tính | Nội dung |
|---|---|
| **ID** | FR-AUTH-02 |
| **Tên** | Đăng nhập tài khoản |
| **Actor** | Tất cả người dùng |
| **Preconditions** | Đã có tài khoản; có kết nối Internet |

**Main Flow:**
1. Người dùng nhập Email + Mật khẩu
2. Nhấn "Đăng nhập"
3. Firebase Auth xác thực
4. Hệ thống lấy thông tin vai trò từ Firestore
5. Chuyển đến màn hình Dashboard tương ứng vai trò

**Alternative Flow:**
- A1: Chọn "Nhớ tài khoản" → Tự động đăng nhập lần sau

**Exception Flow:**
- E1: Sai email/mật khẩu → Thông báo "Thông tin đăng nhập không đúng"
- E2: Sai ≥ 5 lần → Khóa tài khoản 15 phút
- E3: Tài khoản bị vô hiệu hóa → Thông báo liên hệ Admin

**Validation Rules:**
- Không để trống Email hoặc Mật khẩu
- Email phải đúng định dạng

---

### FR-AUTH-03: Quên mật khẩu

**Main Flow:**
1. Nhấn "Quên mật khẩu"
2. Nhập địa chỉ email đã đăng ký
3. Hệ thống gửi link reset mật khẩu qua email (Firebase)
4. Người dùng nhấn link, đặt mật khẩu mới
5. Đăng nhập bằng mật khẩu mới

---

### FR-AUTH-04: Phân quyền RBAC

| Vai trò | Quyền |
|---|---|
| **ELDERLY** | Xem/nhập sức khỏe cá nhân, SOS, nhắc thuốc, hồ sơ cá nhân |
| **FAMILY** | Xem dashboard người cao tuổi, cấu hình nhắc thuốc, xem GPS |
| **DOCTOR** | Xem dữ liệu sức khỏe bệnh nhân, ghi chú, xuất báo cáo |
| **ADMIN** | Toàn quyền quản lý hệ thống |

---

## 5.2 FR-PROF: Quản lý hồ sơ

---

### FR-PROF-01: Hồ sơ người cao tuổi

| Thuộc tính | Nội dung |
|---|---|
| **ID** | FR-PROF-01 |
| **Tên** | Tạo và quản lý hồ sơ người cao tuổi |
| **Actor** | Người cao tuổi, Người thân |
| **Preconditions** | Đã đăng nhập; chưa có hồ sơ hoặc muốn cập nhật |

**Thông tin hồ sơ:**

| Trường | Kiểu dữ liệu | Bắt buộc | Ghi chú |
|---|---|---|---|
| Họ tên | String | ✅ | 2–100 ký tự |
| Ngày sinh | Date | ✅ | Định dạng DD/MM/YYYY |
| Giới tính | Enum | ✅ | Nam / Nữ / Khác |
| Nhóm máu | Enum | ❌ | A/B/AB/O |
| Chiều cao | Float | ❌ | cm, 50–250 |
| Cân nặng | Float | ❌ | kg, 10–300 |
| Bệnh nền | List\<String\> | ❌ | Đa chọn + tự nhập |
| Dị ứng thuốc | List\<String\> | ❌ | Đa chọn + tự nhập |
| Địa chỉ | String | ❌ | Địa chỉ thường trú |
| Ảnh đại diện | Image URL | ❌ | Firebase Storage |
| Người liên hệ khẩn cấp | Object | ✅ | Tên + SĐT |

**Main Flow:**
1. Chọn "Thêm người cao tuổi" hoặc "Cập nhật hồ sơ"
2. Điền các thông tin
3. Tải ảnh đại diện (tuỳ chọn)
4. Nhấn "Lưu"
5. Hệ thống lưu vào Firestore
6. Hiển thị hồ sơ đã cập nhật

---

### FR-PROF-02: Liên kết người chăm sóc

**Main Flow:**
1. Người thân nhập mã liên kết của người cao tuổi (hoặc quét QR)
2. Hệ thống xác nhận mã hợp lệ
3. Gửi yêu cầu kết nối đến người cao tuổi
4. Người cao tuổi chấp nhận → Liên kết thành công
5. Người thân thấy người cao tuổi trong dashboard

---

## 5.3 FR-SOS: Cảnh báo khẩn cấp

---

### FR-SOS-01: Panic Button

| Thuộc tính | Nội dung |
|---|---|
| **ID** | FR-SOS-01 |
| **Tên** | Kích hoạt SOS bằng nút bấm |
| **Actor** | Người cao tuổi |
| **Preconditions** | Ứng dụng đang chạy (foreground hoặc background) |

**Main Flow:**
1. Người dùng bấm nút SOS đỏ trên màn hình chính
2. Màn hình chuyển sang giao diện đếm ngược 5 giây (toàn màn hình, nền đỏ)
3. Hiển thị nút "HỦY" lớn
4. Sau 5 giây (không hủy):
   a. Hệ thống lấy tọa độ GPS
   b. Tạo bản ghi SOS trong Firestore
   c. Gửi FCM notification đến tất cả người thân với payload: {type: SOS, lat, lng, timestamp, elderlyName}
   d. Gửi SMS đến người liên hệ khẩn cấp
   e. Phát âm thanh còi báo động
   f. Hiển thị màn hình xác nhận "Đã gửi cảnh báo"

**Alternative Flow:**
- A1: Người dùng nhấn "HỦY" trong 5 giây → Hủy SOS, trở về màn hình chính
- A2: GPS chưa lấy được → Gửi SOS không có tọa độ, kèm thông báo "Không có GPS"

**Exception Flow:**
- E1: Không có Internet → Ghi SOS vào local, phát âm báo động, thử gửi lại khi có mạng
- E2: FCM lỗi → Thử gửi SMS dự phòng

**Business Rules:**
- BR-SOS-01: SOS phải được gửi trong vòng 3 giây sau khi đếm ngược xong
- BR-SOS-02: Tối thiểu gửi đến 1 người thân hoặc người liên hệ khẩn cấp
- BR-SOS-03: Lưu log mỗi lần SOS kể cả đã hủy

---

### FR-SOS-02: Shake Detection SOS

| Thuộc tính | Nội dung |
|---|---|
| **ID** | FR-SOS-02 |
| **Tên** | Kích hoạt SOS bằng lắc điện thoại |
| **Actor** | Người cao tuổi |
| **Preconditions** | Tính năng Shake Detection được bật trong cài đặt |

**Main Flow:**
1. Cảm biến accelerometer phát hiện gia tốc > ngưỡng cấu hình
2. Kiểm tra: phát hiện lắc ≥ số lần cấu hình trong 2 giây
3. Kích hoạt đếm ngược SOS 5 giây (tương tự FR-SOS-01)
4. Tiếp tục Main Flow của FR-SOS-01

**Configuration:**
- Ngưỡng gia tốc: 15–25 m/s² (mặc định: 20)
- Số lần lắc: 2–5 (mặc định: 3)
- Có thể bật/tắt từ cài đặt

---

## 5.4 FR-HLT: Theo dõi sức khỏe

---

### FR-HLT-01: Nhập chỉ số sức khỏe

| Thuộc tính | Nội dung |
|---|---|
| **ID** | FR-HLT-01 |
| **Tên** | Nhập và lưu chỉ số sức khỏe |
| **Actor** | Người cao tuổi, Người thân |
| **Preconditions** | Đã đăng nhập; có hồ sơ người cao tuổi |

**Các chỉ số và ngưỡng cảnh báo:**

| Chỉ số | Đơn vị | Ngưỡng bình thường | Ngưỡng cảnh báo |
|---|---|---|---|
| Huyết áp tâm thu (SYS) | mmHg | 90–139 | < 90 hoặc ≥ 140 |
| Huyết áp tâm trương (DIA) | mmHg | 60–89 | < 60 hoặc ≥ 90 |
| Nhịp tim | bpm | 60–100 | < 60 hoặc > 100 |
| Đường huyết (đói) | mmol/L | 3.9–6.9 | < 3.9 hoặc > 7.0 |
| Nhiệt độ | °C | 36.1–37.2 | < 35.5 hoặc > 38.0 |
| SpO2 | % | 95–100 | < 93 |
| Cân nặng | kg | Theo BMI | BMI < 18.5 hoặc > 30 |

**Main Flow:**
1. Chọn loại chỉ số cần nhập
2. Nhập giá trị (bàn phím số lớn)
3. Hệ thống tự động điền thời gian hiện tại
4. Có thể thêm ghi chú
5. Nhấn "Lưu"
6. Hệ thống kiểm tra ngưỡng cảnh báo
7. Nếu vượt ngưỡng: gửi cảnh báo đến người thân và bác sĩ
8. Lưu vào Firestore (có hỗ trợ offline)
9. Cập nhật biểu đồ

---

### FR-HLT-02: Biểu đồ sức khỏe

**Các loại biểu đồ:**
- Line chart: Xu hướng theo thời gian
- Bar chart: So sánh theo ngày/tuần
- Khoảng thời gian: Ngày / Tuần / Tháng / Năm

**Tính năng:**
- Pinch to zoom
- Tap để xem giá trị cụ thể
- Đường ngưỡng cảnh báo (reference lines)
- Export biểu đồ dưới dạng ảnh

---

## 5.5 FR-MED: Nhắc uống thuốc

---

### FR-MED-01: Tạo lịch uống thuốc

| Thuộc tính | Nội dung |
|---|---|
| **ID** | FR-MED-01 |
| **Tên** | Tạo lịch nhắc uống thuốc |
| **Actor** | Người thân, Bác sĩ |
| **Preconditions** | Đã đăng nhập; có hồ sơ người cao tuổi |

**Thông tin lịch thuốc:**

| Trường | Kiểu | Bắt buộc | Ghi chú |
|---|---|---|---|
| Tên thuốc | String | ✅ | |
| Liều lượng | String | ✅ | Ví dụ: "1 viên", "5ml" |
| Số lần/ngày | Int | ✅ | 1–6 lần |
| Thời gian uống | List\<Time\> | ✅ | Ví dụ: [07:00, 12:00, 19:00] |
| Ngày bắt đầu | Date | ✅ | |
| Ngày kết thúc | Date | ❌ | Nếu trống = uống mãi mãi |
| Ghi chú | String | ❌ | Ví dụ: "Uống sau khi ăn" |
| Màu sắc nhận dạng | Color | ❌ | |
| Ảnh viên thuốc | Image | ❌ | |

**Main Flow:**
1. Chọn "Thêm thuốc"
2. Điền thông tin thuốc
3. Chọn thời gian nhắc
4. Nhấn "Lưu"
5. Hệ thống tạo local notification schedule
6. Hiển thị trong danh sách lịch thuốc

---

### FR-MED-02: Kích hoạt nhắc uống thuốc

**Main Flow:**
1. Đến giờ uống thuốc đã cấu hình
2. Hệ thống hiển thị Local Notification với thông báo: "⏰ Đến giờ uống [Tên thuốc] - [Liều lượng]"
3. Đồng thời: TTS đọc bằng giọng nói tiếng Việt: "Đã đến giờ uống thuốc [Tên thuốc]"
4. Notification có 2 action button: [✅ Đã uống] [❌ Bỏ qua]
5. Người dùng xác nhận
6. Hệ thống lưu log và cập nhật trạng thái

**Exception Flow:**
- E1: Sau 30 phút không phản hồi → Gửi FCM đến người thân: "[Tên người cao tuổi] chưa xác nhận uống [Tên thuốc]"
- E2: Ứng dụng bị kill → Local notification vẫn hiển thị nhờ WorkManager/AlarmManager

---

## 5.6 FR-FAM: Dashboard người thân

---

### FR-FAM-01: Màn hình tổng quan

**Thông tin hiển thị cho mỗi người cao tuổi:**

| Thông tin | Nguồn dữ liệu | Tần suất cập nhật |
|---|---|---|
| Ảnh + Tên | Firestore Profile | Realtime |
| Trạng thái online | Presence System | Realtime |
| Lần hoạt động cuối | Firestore | Realtime |
| Huyết áp mới nhất | Firestore Health | Realtime |
| Nhịp tim mới nhất | Firestore Health | Realtime |
| Đường huyết mới nhất | Firestore Health | Realtime |
| Tỷ lệ uống thuốc hôm nay | Firestore Medication | Realtime |
| SOS gần nhất | Firestore SOS | Realtime |
| Pin điện thoại | FCM Data Message | 15 phút/lần |
| Vị trí trên bản đồ | Geolocator | 15 phút/lần |

---

## 5.7 FR-DOC: Dashboard bác sĩ

---

### FR-DOC-01: Màn hình bệnh nhân

**Main Flow:**
1. Bác sĩ chọn bệnh nhân từ danh sách
2. Xem tổng quan sức khỏe
3. Chọn loại chỉ số muốn xem chi tiết
4. Xem biểu đồ + lịch sử đo
5. Hệ thống highlight giá trị bất thường (màu đỏ)
6. Bác sĩ có thể thêm ghi chú tư vấn
7. Export báo cáo PDF

---

## 5.8 FR-GPS: Định vị và bản đồ

---

### FR-GPS-01: Theo dõi vị trí

| Thuộc tính | Nội dung |
|---|---|
| **ID** | FR-GPS-01 |
| **Actor** | Người cao tuổi (gửi), Người thân (nhận) |

**Main Flow:**
1. App của người cao tuổi lấy GPS mỗi 15 phút khi background
2. Lưu vào Firestore: {lat, lng, accuracy, timestamp, batteryLevel}
3. App người thân đọc từ Firestore, hiển thị trên Google Maps
4. Marker màu xanh = online (< 30 phút), màu xám = offline

**Điều kiện:**
- GPS chỉ cập nhật khi pin > 15%
- Có thể tắt GPS tracking từ cài đặt
- Người cao tuổi biết vị trí đang được chia sẻ (icon GPS hiển thị)

---

## 5.9 FR-RPT: Báo cáo

---

### FR-RPT-01: Xuất báo cáo PDF

**Nội dung báo cáo:**
- Thông tin cơ bản hồ sơ
- Tóm tắt sức khỏe theo kỳ
- Biểu đồ xu hướng các chỉ số
- Lịch sử đo chi tiết (bảng)
- Tỷ lệ tuân thủ uống thuốc
- Danh sách sự kiện SOS
- Ghi chú của bác sĩ (nếu có)

**Main Flow:**
1. Chọn kỳ báo cáo (Tuần / Tháng / Năm)
2. Chọn người cao tuổi (nếu quản lý nhiều người)
3. Nhấn "Xuất PDF"
4. Hệ thống tạo file PDF
5. Hiển thị preview
6. Lưu vào thiết bị hoặc chia sẻ qua email

---

## 5.10 FR-NOT: Hệ thống thông báo

### Bảng phân loại thông báo

| Loại | Kênh | Ưu tiên | Nội dung |
|---|---|---|---|
| SOS_ALERT | FCM + SMS | 🔴 Critical | "[Tên] đang cần giúp đỡ! Vị trí: [GPS Link]" |
| MEDICINE_REMINDER | Local Notification | 🟡 High | "Đến giờ uống [Thuốc]" |
| MEDICINE_MISSED | FCM | 🟡 High | "[Tên] chưa uống [Thuốc] lúc [Giờ]" |
| HEALTH_ALERT | FCM | 🟠 Medium | "[Tên]: [Chỉ số] = [Giá trị] (Bất thường)" |
| BATTERY_LOW | FCM | 🟠 Medium | "Pin điện thoại [Tên] còn [X]%" |
| DAILY_SUMMARY | FCM | 🟢 Low | "Tóm tắt sức khỏe hôm nay của [Tên]" |

---

## 5.11 FR-ADMIN: Quản trị hệ thống

### FR-ADMIN-01: Quản lý người dùng

**Chức năng Admin:**
- Xem danh sách tất cả tài khoản
- Tìm kiếm, lọc theo vai trò / trạng thái
- Vô hiệu hóa / kích hoạt tài khoản
- Xem lịch sử đăng nhập
- Reset mật khẩu cho người dùng
- Gửi thông báo hàng loạt

### FR-ADMIN-02: Thống kê hệ thống

**Dashboard Admin hiển thị:**
- Tổng số người dùng theo vai trò
- DAU / MAU
- Số SOS theo ngày/tuần
- Tỷ lệ tuân thủ uống thuốc toàn hệ thống
- Top các chỉ số sức khỏe bất thường
- Lỗi hệ thống và performance metrics

---

## 5.12 FR-LOG: Nhật ký hoạt động

### Các sự kiện được ghi log

| Sự kiện | Dữ liệu lưu |
|---|---|
| Đăng nhập | userId, timestamp, IP, device |
| Đăng xuất | userId, timestamp |
| Kích hoạt SOS | userId, lat, lng, timestamp, status |
| Xác nhận uống thuốc | userId, medicationId, action, timestamp |
| Nhập chỉ số sức khỏe | userId, type, value, timestamp |
| Cập nhật hồ sơ | userId, fields changed, timestamp |
| Xuất báo cáo | userId, type, period, timestamp |

**Chính sách lưu trữ:**
- Log được lưu 12 tháng
- Admin có thể xem toàn bộ log
- Người dùng chỉ xem log của mình
