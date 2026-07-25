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

#### Module 5 – Theo dõi sức khỏe (FR-HEALTH)

| ID | Tên chức năng | Mô tả | Actor | Ưu tiên |
|----|--------------|-------|-------|---------|
| FR-HLT-01 | Nhập chỉ số sức khỏe | Huyết áp, đường huyết, nhịp tim, nhiệt độ, cân nặng, SpO2 | NCT, Người thân | Must |
| FR-HLT-02 | Validate ngưỡng bất thường | Cảnh báo khi chỉ số vượt ngưỡng bình thường | Hệ thống | Must |
| FR-HLT-03 | Lưu lịch sử đo | Mỗi lần đo lưu timestamp, giá trị, người nhập | Hệ thống | Must |
| FR-HLT-04 | Biểu đồ theo thời gian | Hiển thị biểu đồ ngày/tuần/tháng/năm (fl_chart) | NCT, Người thân, BS | Must |
| FR-HLT-05 | Cảnh báo bất thường | Gửi FCM cho người thân khi chỉ số nguy hiểm | Hệ thống | Must |
| FR-HLT-06 | Cấu hình ngưỡng cá nhân | Bác sĩ/người thân đặt ngưỡng riêng cho NCT | BS, Người thân | Should |
| FR-HLT-07 | Ghi chú theo lần đo | Thêm ghi chú văn bản cho mỗi lần đo | NCT, BS | Should |
| FR-HLT-08 | Xem bởi bác sĩ | BS được xem toàn bộ lịch sử NCT được liên kết | BS | Must |

#### Module 6 – Vị trí & Bản đồ (FR-LOC)

| ID | Tên chức năng | Mô tả | Actor | Ưu tiên |
|----|--------------|-------|-------|---------|
| FR-LOC-01 | Cập nhật vị trí định kỳ | Lấy GPS mỗi 15 phút khi app active | Hệ thống | Should |
| FR-LOC-02 | Hiển thị trên bản đồ | Google Maps với marker vị trí NCT | Người thân | Should |
| FR-LOC-03 | Lịch sử vị trí | Xem vị trí theo thời gian (24h gần nhất) | Người thân | Could |
| FR-LOC-04 | Vị trí khi SOS | Gửi tọa độ GPS chính xác khi SOS kích hoạt | Hệ thống | Must |
| FR-LOC-05 | Thời gian cập nhật cuối | Hiển thị "cập nhật lúc HH:mm" trên dashboard | Người thân | Must |

#### Module 7 – Dashboard & Thông báo (FR-DASH)

| ID | Tên chức năng | Mô tả | Actor | Ưu tiên |
|----|--------------|-------|-------|---------|
| FR-DSH-01 | Dashboard người thân | Tổng quan NCT: status, sức khỏe, thuốc, SOS, pin, GPS | Người thân | Must |
| FR-DSH-02 | Dashboard bác sĩ | Danh sách BN, biểu đồ sức khỏe, chỉ số bất thường | BS | Should |
| FR-DSH-03 | Ghi chú bác sĩ | BS ghi chú tư vấn, khám cho từng NCT | BS | Should |
| FR-DSH-04 | Trạng thái online NCT | Hiển thị online/offline, lần hoạt động cuối | Người thân | Must |
| FR-DSH-05 | Mức pin điện thoại | Hiển thị % pin, cảnh báo khi < 20% | Người thân | Should |
| FR-DSH-06 | Lịch sử thông báo | Danh sách tất cả thông báo đã nhận | Tất cả | Should |

#### Module 8 – Báo cáo (FR-RPT)

| ID | Tên chức năng | Mô tả | Actor | Ưu tiên |
|----|--------------|-------|-------|---------|
| FR-RPT-01 | Xuất báo cáo PDF | Báo cáo sức khỏe theo tuần/tháng/năm dạng PDF | Người thân, BS | Could |
| FR-RPT-02 | Xuất báo cáo Excel | Dữ liệu sức khỏe dạng bảng tính | Người thân, BS | Could |
| FR-RPT-03 | Báo cáo tổng hợp | Thống kê tuân thủ thuốc, số SOS, xu hướng sức khỏe | Người thân, BS | Could |

---

## 3. YÊU CẦU PHI CHỨC NĂNG (NON-FUNCTIONAL REQUIREMENTS)

| ID | Nhóm | Yêu cầu | Tiêu chí đo |
|----|------|---------|-------------|
| NFR-01 | Hiệu năng | Màn hình load ≤ 2 giây | Đo trên thiết bị tầm trung (RAM 3GB) |
| NFR-02 | Hiệu năng | Gửi SOS ≤ 3 giây kể từ khi xác nhận | Đo từ tap → FCM delivered |
| NFR-03 | Hiệu năng | Firestore query ≤ 1 giây | Có index, dữ liệu ≤ 10,000 docs |
| NFR-04 | Bảo mật | Mã hóa dữ liệu nhạy cảm | Hive encrypted, HTTPS only |
| NFR-05 | Bảo mật | RBAC nghiêm ngặt | Firestore Rules test pass 100% |
| NFR-06 | Bảo mật | OTP hết hạn 5 phút | Kiểm tra expiry server-side |
| NFR-07 | Khả dụng | Uptime ≥ 99.5% | Firebase SLA + monitor |
| NFR-08 | Accessibility | Font ≥ 18sp, nút ≥ 48dp | UI audit checklist |
| NFR-09 | Accessibility | Hỗ trợ TTS tiếng Việt | flutter_tts test |
| NFR-10 | Offline | Cache dữ liệu cơ bản khi offline | Hive local DB |
| NFR-11 | Offline | Sync khi có lại Internet | Firestore offline persistence |
| NFR-12 | Khả năng mở rộng | Hỗ trợ 10,000 MAU giai đoạn 1 | Firebase auto-scale |
| NFR-13 | UX | Dark Mode đầy đủ | Tất cả màn hình test Dark Mode |
| NFR-14 | UX | Responsive 5" – 7" | Test 4 kích thước màn hình |
| NFR-15 | Bảo trì | Coverage ≥ 70% | Unit test + widget test |

---

## 4. PHÂN LOẠI MoSCoW

```
┌─────────────────────────────────────────────────────────────────┐
│                      MoSCoW PRIORITY                            │
├──────────────┬──────────────┬──────────────┬────────────────────┤
│   MUST HAVE  │  SHOULD HAVE │  COULD HAVE  │   WON'T HAVE (v1)  │
├──────────────┼──────────────┼──────────────┼────────────────────┤
│ • Auth + OTP │ • BS Dashb.  │ • Báo cáo   │ • AI Fall Detect.  │
│ • Hồ sơ NCT │ • GPS track  │   PDF/Excel  │ • Smart Watch sync │
│ • SOS + GPS  │ • Ghi chú BS │ • Lịch sử vị│ • Video call       │
│ • Shake Det. │ • BS link    │   trí 24h   │ • iOS support      │
│ • Nhắc thuốc │ • Nhắc lại  │ • Export    │ • AI health pred.  │
│ + TTS        │   thuốc      │   Excel      │ • BLE devices      │
│ • Đo sức KH  │ • Ngưỡng    │ • Admin     │                    │
│ • Cảnh báo  │   cá nhân    │   dashboard  │                    │
│ • Gia đình  │ • Mức pin    │              │                    │
│   Dashboard  │ • Nhật ký HĐ│              │                    │
│ • Dark Mode  │              │              │                    │
│ • Offline   │              │              │                    │
└──────────────┴──────────────┴──────────────┴────────────────────┘
```

---

## 5. USER STORIES (AGILE)

### Sprint 1 – Authentication & Profile

**US-01: Đăng ký tài khoản**
```
As a người dùng mới,
I want to đăng ký bằng số điện thoại và mật khẩu,
So that tôi có thể bắt đầu sử dụng ứng dụng.

Acceptance Criteria:
  ✓ AC-01: SĐT hợp lệ (10 số, bắt đầu 0)
  ✓ AC-02: Mật khẩu ≥ 8 ký tự, có chữ hoa + số
  ✓ AC-03: Gửi OTP SMS trong 30 giây
  ✓ AC-04: OTP hết hạn sau 5 phút
  ✓ AC-05: Sau xác thực → màn hình chọn vai trò

Priority: Must | Story Points: 5 | Sprint: 1
```

**US-02: Đăng nhập**
```
As a người dùng đã có tài khoản,
I want to đăng nhập bằng SĐT + mật khẩu,
So that tôi có thể truy cập ứng dụng an toàn.

Acceptance Criteria:
  ✓ AC-01: Đăng nhập thành công → vào Dashboard đúng role
  ✓ AC-02: Sai mật khẩu 5 lần → khóa 15 phút
  ✓ AC-03: Duy trì đăng nhập khi mở lại app (auto-login)
  ✓ AC-04: Loading indicator khi đang xác thực

Priority: Must | Story Points: 3 | Sprint: 1
```

**US-03: Tạo hồ sơ người cao tuổi**
```
As a người thân,
I want to tạo hồ sơ đầy đủ cho cha/mẹ,
So that hệ thống có đủ thông tin để hỗ trợ hiệu quả.

Acceptance Criteria:
  ✓ AC-01: Nhập họ tên, ngày sinh, giới tính, nhóm máu
  ✓ AC-02: Nhập danh sách bệnh nền (multi-select)
  ✓ AC-03: Nhập danh sách dị ứng thuốc
  ✓ AC-04: Nhập ≥ 1 người liên hệ khẩn cấp
  ✓ AC-05: Upload ảnh đại diện (tuỳ chọn)
  ✓ AC-06: Lưu thành công → hiển thị mã chia sẻ

Priority: Must | Story Points: 5 | Sprint: 1
```

### Sprint 2 – SOS & Safety

**US-04: Kích hoạt SOS khẩn cấp**
```
As a người cao tuổi,
I want to bấm nút SOS lớn khi gặp nguy hiểm,
So that người thân được thông báo ngay lập tức cùng vị trí của tôi.

Acceptance Criteria:
  ✓ AC-01: Nút SOS hiển thị luôn trên màn hình chính
  ✓ AC-02: Nút SOS ≥ 80×80dp, màu đỏ rõ ràng
  ✓ AC-03: Bấm → hiện đếm ngược 5 giây có nút Hủy
  ✓ AC-04: Sau 5 giây → lấy GPS + gửi FCM tất cả người thân
  ✓ AC-05: Phát còi báo động trên máy NCT
  ✓ AC-06: Lưu sự kiện SOS vào Firestore
  ✓ AC-07: Notification người thân hiện tên + địa chỉ GPS

Priority: Must | Story Points: 8 | Sprint: 2
```

**US-05: Shake Detection tự động SOS**
```
As a người cao tuổi bị ngã,
I want to lắc điện thoại để tự động gửi SOS,
So that ngay cả khi không bấm được màn hình tôi vẫn có thể cầu cứu.

Acceptance Criteria:
  ✓ AC-01: Phát hiện lắc ≥ 3 lần liên tiếp trong 1 giây
  ✓ AC-02: Kích hoạt cùng luồng với bấm tay (đếm ngược 5s)
  ✓ AC-03: Độ nhạy có thể cấu hình (Thấp / Trung bình / Cao)
  ✓ AC-04: Có thể bật/tắt tính năng Shake Detection

Priority: Must | Story Points: 5 | Sprint: 2
```

### Sprint 3 – Medication Reminder

**US-06: Tạo lịch uống thuốc**
```
As a người thân,
I want to tạo lịch uống thuốc chi tiết cho cha/mẹ,
So that ứng dụng tự động nhắc đúng giờ.

Acceptance Criteria:
  ✓ AC-01: Nhập tên thuốc, liều lượng, đơn vị
  ✓ AC-02: Chọn số lần/ngày và giờ uống cụ thể
  ✓ AC-03: Chọn ngày bắt đầu và ngày kết thúc (tuỳ chọn)
  ✓ AC-04: Thêm ghi chú (uống trước/sau ăn)
  ✓ AC-05: Xem preview lịch trước khi lưu

Priority: Must | Story Points: 5 | Sprint: 3
```

**US-07: Nhắc thuốc bằng giọng nói**
```
As a người cao tuổi,
I want to nghe thông báo nhắc thuốc bằng giọng Việt,
So that tôi biết cần uống thuốc gì ngay cả khi không đọc được.

Acceptance Criteria:
  ✓ AC-01: Thông báo push đến đúng giờ đã cài
  ✓ AC-02: TTS đọc: "Đến giờ uống [tên thuốc], [liều lượng]"
  ✓ AC-03: Giọng đọc rõ ràng, tốc độ phù hợp NCT (0.8x)
  ✓ AC-04: Hiển thị nút "Đã uống" và "Nhắc lại sau 15 phút"
  ✓ AC-05: Nếu sau 30 phút không phản hồi → gửi cảnh báo NT

Priority: Must | Story Points: 5 | Sprint: 3
```

### Sprint 4 – Health Tracking

**US-08: Nhập và theo dõi chỉ số sức khỏe**
```
As a người cao tuổi / người thân,
I want to nhập và xem chỉ số sức khỏe hàng ngày,
So that tôi và bác sĩ có thể theo dõi xu hướng sức khỏe.

Acceptance Criteria:
  ✓ AC-01: Nhập được 6 loại chỉ số: HA, đường huyết, nhịp tim, nhiệt độ, cân nặng, SpO2
  ✓ AC-02: Validate ngưỡng hợp lệ cho từng chỉ số
  ✓ AC-03: Hiển thị biểu đồ đường theo ngày/tuần/tháng
  ✓ AC-04: Đánh dấu màu đỏ khi giá trị bất thường
  ✓ AC-05: Gửi cảnh báo FCM khi vượt ngưỡng nguy hiểm

Priority: Must | Story Points: 8 | Sprint: 4
```

---

## 6. USE CASES ĐẦY ĐỦ

### UC-01: Kích hoạt SOS

| Thuộc tính | Chi tiết |
|------------|---------|
| **ID** | UC-01 |
| **Tên** | Kích hoạt Panic Button SOS |
| **Actor chính** | Người cao tuổi |
| **Actor phụ** | Hệ thống Firebase, Người thân |
| **Điều kiện tiên quyết** | NCT đã đăng nhập; đã có ≥ 1 người thân liên kết |
| **Điều kiện hậu** | SOS đã gửi, lưu Firestore, người thân nhận FCM |

**Luồng chính:**
```
1. NCT bấm nút SOS đỏ trên màn hình chính
2. Hệ thống hiển thị đếm ngược 5 giây + nút Hủy
3. Hệ thống lấy tọa độ GPS hiện tại
4. Đếm ngược kết thúc (không hủy)
5. Hệ thống kích hoạt còi báo động trên thiết bị
6. Hệ thống tạo document SOS trong Firestore
7. Cloud Function gửi FCM đến tất cả người thân
8. Notification hiển thị: "[Tên NCT] cần giúp đỡ! Vị trí: [địa chỉ]"
9. Người thân mở app → xem bản đồ vị trí NCT
```

**Luồng thay thế:**
```
A1 (Bước 3): GPS không khả dụng
  → Gửi SOS với vị trí "Không xác định"
  → Hiển thị cảnh báo "Không lấy được GPS"

A2 (Bước 2): NCT bấm Hủy trong 5 giây
  → Hủy SOS, quay về màn hình chính
  → Không gửi notification
```

**Luồng ngoại lệ:**
```
E1: Mất kết nối Internet
  → Lưu SOS vào queue offline (Hive)
  → Gửi SMS fallback (nếu có cấu hình)
  → Sync khi có lại Internet

E2: GPS timeout > 10 giây
  → Tiếp tục gửi SOS với tọa độ null
  → Ghi log lỗi GPS
```

### UC-02: Nhắc uống thuốc

| Thuộc tính | Chi tiết |
|------------|---------|
| **ID** | UC-02 |
| **Tên** | Nhắc uống thuốc và xác nhận |
| **Actor chính** | Hệ thống (tự động), Người cao tuổi |
| **Điều kiện tiên quyết** | Lịch thuốc đã được tạo; đến giờ uống |

**Luồng chính:**
```
1. Đến giờ uống thuốc đã cài
2. Hệ thống gửi local notification + push notification
3. Màn hình nhắc thuốc hiện ra tự động
4. flutter_tts đọc: "Đến giờ uống [tên thuốc], [liều lượng]"
5. NCT bấm "Đã uống"
6. Hệ thống lưu log xác nhận + timestamp
7. Dashboard người thân cập nhật trạng thái "✅ Đã uống"
```

**Luồng thay thế:**
```
A1 (Bước 5): NCT bấm "Nhắc lại sau 15 phút"
  → Hủy notification hiện tại
  → Lên lịch notification sau 15 phút
  → Giới hạn nhắc lại tối đa 3 lần

A2 (Bước 5): NCT bấm "Bỏ qua"
  → Ghi log bỏ qua + lý do (tuỳ chọn)
  → Không gửi cảnh báo ngay
```

**Luồng ngoại lệ:**
```
E1: Không xác nhận sau 30 phút
  → Cloud Function gửi FCM cảnh báo cho người thân
  → Notification: "[Tên NCT] chưa uống [thuốc] lúc [giờ]"
```

---

## 7. BUSINESS RULES

| ID | Rule | Mô tả | Module |
|----|------|-------|--------|
| BR-01 | SOS đếm ngược | Phải có 5 giây đếm ngược trước khi gửi SOS | SOS |
| BR-02 | Hủy SOS trong 5s | NCT có thể hủy nếu kích hoạt nhầm | SOS |
| BR-03 | Tần suất SOS | Không giới hạn số lần SOS (an toàn ưu tiên) | SOS |
| BR-04 | OTP hết hạn | OTP hết hạn sau 5 phút, tối đa 3 lần gửi lại | Auth |
| BR-05 | Khóa tài khoản | Sai mật khẩu 5 lần → khóa 15 phút | Auth |
| BR-06 | Nhắc lại thuốc | Tối đa nhắc lại 3 lần, mỗi lần cách 15 phút | Medication |
| BR-07 | Cảnh báo bỏ lỡ thuốc | Sau 30 phút không xác nhận → cảnh báo người thân | Medication |
| BR-08 | Ngưỡng sức khỏe | Huyết áp > 180/120 → cảnh báo đỏ; > 140/90 → cảnh báo vàng | Health |
| BR-09 | Cảnh báo pin | Pin < 20% → hiển thị cảnh báo dashboard | Dashboard |
| BR-10 | Quyền liên kết | NCT phải xác nhận trước khi bác sĩ có quyền xem dữ liệu | Profile |
| BR-11 | Giới hạn thuốc | Tối đa 20 loại thuốc active cùng lúc | Medication |
| BR-12 | Shake cooldown | Sau khi SOS gửi, Shake Detection tạm dừng 5 phút | SOS |

---

## 8. NGƯỠNG CHỈ SỐ SỨC KHỎE (HEALTH THRESHOLDS)

| Chỉ số | Đơn vị | Bình thường | Cảnh báo vàng | Cảnh báo đỏ |
|--------|--------|-------------|----------------|--------------|
| Huyết áp tâm thu | mmHg | 90–139 | 140–179 | ≥ 180 hoặc < 90 |
| Huyết áp tâm trương | mmHg | 60–89 | 90–119 | ≥ 120 hoặc < 60 |
| Đường huyết (lúc đói) | mmol/L | 3.9–6.1 | 6.2–10.0 | > 10.0 hoặc < 3.9 |
| Nhịp tim | bpm | 60–100 | 101–120 hoặc 50–59 | > 120 hoặc < 50 |
| Nhiệt độ cơ thể | °C | 36.1–37.2 | 37.3–38.5 | > 38.5 hoặc < 35.5 |
| SpO2 | % | 95–100 | 90–94 | < 90 |
| Cân nặng | kg | Theo BMI | BMI 25–29.9 | BMI ≥ 30 hoặc < 18.5 |

---

## 9. MA TRẬN TRUY VẾT YÊU CẦU (RTM)

| US ID | FR ID | UC ID | BR ID | Màn hình |
|-------|-------|-------|-------|----------|
| US-01 | FR-AUTH-01, FR-AUTH-02 | UC-10 | BR-04 | RegisterScreen |
| US-02 | FR-AUTH-03 | UC-11 | BR-05 | LoginScreen |
| US-03 | FR-PRF-01 | UC-12 | — | ElderlyProfileForm |
| US-04 | FR-SOS-01..07 | UC-01 | BR-01, BR-02, BR-03 | HomeScreen (SOS) |
| US-05 | FR-SOS-03, FR-SOS-09 | UC-01 | BR-12 | SettingsScreen |
| US-06 | FR-MED-01 | UC-02 | BR-11 | MedicationFormScreen |
| US-07 | FR-MED-02..06 | UC-02 | BR-06, BR-07 | MedicationReminderScreen |
| US-08 | FR-HLT-01..05 | UC-03 | BR-08 | HealthInputScreen |

---

## 10. KẾT LUẬN GIAI ĐOẠN 2

**Tổng yêu cầu xác định:**
- Yêu cầu chức năng (FR): **47 FRs** phân theo 8 module
- Yêu cầu phi chức năng (NFR): **15 NFRs** phân theo 6 nhóm
- User Stories: **8 US** cho Sprint 1–4
- Use Cases: **2 UC** chi tiết, tổng 12+ UC trong hệ thống
- Business Rules: **12 BR** ràng buộc nghiệp vụ

**Đầu ra chuyển sang Giai đoạn 3:** Danh sách yêu cầu đầy đủ, độ ưu tiên MoSCoW, business rules → làm cơ sở thiết kế hệ thống.
