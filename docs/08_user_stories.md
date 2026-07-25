# Chương 8: User Stories
## GeriatricCare – User Stories theo chuẩn Agile

---

## 8.1 Quy ước

| Ký hiệu | Ý nghĩa |
|---|---|
| US-xxx | User Story ID |
| Priority | 🔴 Critical / 🟠 High / 🟡 Medium / 🟢 Low |
| SP | Story Points (Fibonacci: 1, 2, 3, 5, 8, 13) |
| Sprint | Sprint đề xuất triển khai |

**Template:**
```
As a [actor],
I want [action],
So that [benefit].
```

---

## 8.2 Epic 1: Authentication & Onboarding

---

### US-001: Đăng ký tài khoản

**As a** người dùng mới (người thân),  
**I want** đăng ký tài khoản bằng email và mật khẩu,  
**So that** tôi có thể sử dụng ứng dụng theo dõi cha/mẹ tôi.

**Acceptance Criteria:**
- [ ] AC1: Form đăng ký có đủ: Họ tên, Email, Mật khẩu, Xác nhận mật khẩu, Vai trò
- [ ] AC2: Validate email đúng định dạng
- [ ] AC3: Validate mật khẩu ≥ 8 ký tự, có chữ hoa + số
- [ ] AC4: Hiển thị lỗi cụ thể khi validation fail
- [ ] AC5: Sau đăng ký thành công → chuyển trang hoàn thiện hồ sơ
- [ ] AC6: Email xác thực được gửi tự động
- [ ] AC7: Nếu email đã tồn tại → hiển thị thông báo gợi ý đăng nhập

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🔴 Critical | 3 | Sprint 1 | Authentication |

---

### US-002: Đăng nhập

**As a** người dùng đã có tài khoản,  
**I want** đăng nhập bằng email và mật khẩu,  
**So that** tôi truy cập được vào dashboard của mình.

**Acceptance Criteria:**
- [ ] AC1: Form đăng nhập có Email + Mật khẩu + nút "Đăng nhập"
- [ ] AC2: Sau đăng nhập thành công → chuyển đến dashboard theo vai trò
- [ ] AC3: Sai thông tin ≥ 3 lần → hiển thị captcha
- [ ] AC4: Sai ≥ 5 lần → khóa 15 phút + thông báo
- [ ] AC5: Tuỳ chọn "Nhớ tôi" giữ đăng nhập 30 ngày

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🔴 Critical | 2 | Sprint 1 | Authentication |

---

### US-003: Quên mật khẩu

**As a** người dùng,  
**I want** lấy lại mật khẩu qua email,  
**So that** tôi không bị mất quyền truy cập tài khoản.

**Acceptance Criteria:**
- [ ] AC1: Nhập email → nhận link reset trong < 2 phút
- [ ] AC2: Link chỉ dùng được 1 lần, hết hạn sau 1 giờ
- [ ] AC3: Sau reset → yêu cầu đăng nhập lại

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🟠 High | 2 | Sprint 1 | Authentication |

---

## 8.3 Epic 2: Hồ sơ & Liên kết

---

### US-004: Tạo hồ sơ người cao tuổi

**As a** người thân,  
**I want** tạo hồ sơ chi tiết cho cha/mẹ tôi,  
**So that** thông tin y tế của họ được lưu trữ đầy đủ và dễ truy cập khi khẩn cấp.

**Acceptance Criteria:**
- [ ] AC1: Form đầy đủ: Họ tên, Ngày sinh, Giới tính, Nhóm máu, Chiều cao, Cân nặng
- [ ] AC2: Nhập bệnh nền: danh sách đa chọn + tuỳ chọn tự nhập
- [ ] AC3: Nhập dị ứng thuốc: tương tự bệnh nền
- [ ] AC4: Nhập người liên hệ khẩn cấp: Tên + SĐT (bắt buộc)
- [ ] AC5: Upload ảnh đại diện từ camera hoặc thư viện ảnh
- [ ] AC6: Lưu thành công → hiển thị trong danh sách theo dõi

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🔴 Critical | 5 | Sprint 1 | Profile |

---

### US-005: Liên kết bác sĩ với người cao tuổi

**As a** người thân,  
**I want** thêm bác sĩ vào danh sách theo dõi người cao tuổi,  
**So that** bác sĩ có thể theo dõi sức khỏe và đưa ra tư vấn từ xa.

**Acceptance Criteria:**
- [ ] AC1: Tìm kiếm bác sĩ theo tên hoặc mã bác sĩ
- [ ] AC2: Gửi yêu cầu liên kết đến bác sĩ
- [ ] AC3: Bác sĩ nhận thông báo và có thể chấp nhận/từ chối
- [ ] AC4: Sau chấp nhận → bác sĩ thấy bệnh nhân trong dashboard

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🟠 High | 3 | Sprint 2 | Profile |

---

## 8.4 Epic 3: SOS & An toàn khẩn cấp

---

### US-006: Kích hoạt SOS bằng nút bấm

**As a** người cao tuổi,  
**I want** bấm một nút lớn để gọi cấp cứu,  
**So that** người thân được thông báo ngay lập tức khi tôi gặp nguy hiểm.

**Acceptance Criteria:**
- [ ] AC1: Nút SOS màu đỏ, kích thước ≥ 80dp, luôn hiện trên màn hình chính
- [ ] AC2: Bấm SOS → hiện màn hình đếm ngược 5 giây toàn màn hình
- [ ] AC3: Nút "HỦY" rõ ràng trong thời gian đếm ngược
- [ ] AC4: Sau 5 giây → tự động lấy GPS và gửi thông báo đến tất cả người thân
- [ ] AC5: Phát âm thanh còi báo động khi gửi SOS
- [ ] AC6: Thông báo SOS đến người thân trong < 5 giây
- [ ] AC7: Thông báo gồm: tên người cao tuổi + vị trí GPS (link Maps)
- [ ] AC8: Lưu sự kiện SOS vào lịch sử (kể cả bị hủy)

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🔴 Critical | 8 | Sprint 2 | SOS |

---

### US-007: Kích hoạt SOS bằng lắc điện thoại

**As a** người cao tuổi,  
**I want** lắc điện thoại mạnh để gửi SOS,  
**So that** tôi có thể cầu cứu ngay cả khi không thể nhìn thấy màn hình.

**Acceptance Criteria:**
- [ ] AC1: Lắc mạnh ≥ 3 lần trong 2 giây → kích hoạt đếm ngược SOS
- [ ] AC2: Tính năng hoạt động cả khi app ở background
- [ ] AC3: Cài đặt bật/tắt Shake Detection
- [ ] AC4: Cài đặt điều chỉnh độ nhạy (Thấp / Vừa / Cao)
- [ ] AC5: Phân biệt được lắc SOS với chuyển động bình thường

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🔴 Critical | 8 | Sprint 2 | SOS |

---

### US-008: Nhận thông báo SOS

**As a** người thân,  
**I want** nhận thông báo ngay lập tức khi cha/mẹ kích hoạt SOS,  
**So that** tôi có thể hành động kịp thời.

**Acceptance Criteria:**
- [ ] AC1: Push notification đến dù điện thoại để chế độ im lặng
- [ ] AC2: Notification chứa: tên người cao tuổi, thời gian, link Google Maps vị trí
- [ ] AC3: Mở notification → vào thẳng màn hình SOS detail
- [ ] AC4: Nút "Xác nhận đã tiếp nhận" trên màn hình SOS detail
- [ ] AC5: Xem lịch sử SOS trong 30 ngày gần nhất

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🔴 Critical | 5 | Sprint 2 | SOS |

---

## 8.5 Epic 4: Theo dõi sức khỏe

---

### US-009: Nhập chỉ số huyết áp

**As a** người cao tuổi (hoặc người thân),  
**I want** nhập kết quả đo huyết áp vào ứng dụng,  
**So that** có hồ sơ theo dõi huyết áp lâu dài và cảnh báo kịp thời khi bất thường.

**Acceptance Criteria:**
- [ ] AC1: Form nhập: SYS (mmHg), DIA (mmHg), Pulse (bpm), Ngày giờ, Ghi chú
- [ ] AC2: Bàn phím số lớn, dễ nhập cho người cao tuổi
- [ ] AC3: Lưu ngay cả khi offline
- [ ] AC4: SYS ≥ 140 hoặc DIA ≥ 90 → hiển thị cảnh báo ngay + gửi notification người thân
- [ ] AC5: Sau lưu → cập nhật biểu đồ ngay

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🔴 Critical | 5 | Sprint 3 | Health |

---

### US-010: Nhập các chỉ số sức khỏe khác

**As a** người cao tuổi,  
**I want** nhập đường huyết, nhịp tim, nhiệt độ, SpO2, cân nặng,  
**So that** có đầy đủ hồ sơ sức khỏe tổng thể.

**Acceptance Criteria:**
- [ ] AC1: Tab riêng cho từng loại chỉ số
- [ ] AC2: Mỗi loại có đơn vị rõ ràng và hướng dẫn nhập
- [ ] AC3: SpO2 < 93% → cảnh báo cấp độ cao ngay lập tức
- [ ] AC4: Đường huyết < 3.9 hoặc > 11.0 → cảnh báo khẩn cấp
- [ ] AC5: Lưu thành công → hiện tick xanh xác nhận

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🔴 Critical | 5 | Sprint 3 | Health |

---

### US-011: Xem biểu đồ sức khỏe

**As a** người thân hoặc bác sĩ,  
**I want** xem biểu đồ xu hướng sức khỏe theo thời gian,  
**So that** tôi nhận ra sớm các dấu hiệu bất thường.

**Acceptance Criteria:**
- [ ] AC1: Biểu đồ line chart cho mỗi chỉ số
- [ ] AC2: Chuyển đổi view: Ngày / Tuần / Tháng / Năm
- [ ] AC3: Đường ngưỡng cảnh báo hiển thị trên biểu đồ
- [ ] AC4: Tap vào điểm dữ liệu → hiện giá trị + thời gian
- [ ] AC5: Giá trị bất thường hiển thị màu đỏ
- [ ] AC6: Pinch to zoom

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🟠 High | 5 | Sprint 3 | Health |

---

## 8.6 Epic 5: Nhắc uống thuốc

---

### US-012: Tạo lịch uống thuốc

**As a** người thân,  
**I want** tạo lịch nhắc uống thuốc chi tiết cho cha/mẹ,  
**So that** cha/mẹ không bỏ lỡ liều thuốc quan trọng.

**Acceptance Criteria:**
- [ ] AC1: Form: Tên thuốc, Liều lượng, Số lần/ngày, Thời gian, Ngày bắt đầu/kết thúc, Ghi chú
- [ ] AC2: Có thể thêm ảnh viên thuốc (tùy chọn)
- [ ] AC3: Xem trước lịch nhắc trước khi lưu
- [ ] AC4: Lưu thành công → notification được lên lịch ngay

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🔴 Critical | 5 | Sprint 3 | Medication |

---

### US-013: Nhận nhắc uống thuốc bằng giọng nói

**As a** người cao tuổi,  
**I want** nghe ứng dụng nhắc tôi uống thuốc bằng giọng nói tiếng Việt,  
**So that** tôi không bỏ lỡ thuốc dù đang làm việc khác hoặc thị lực kém.

**Acceptance Criteria:**
- [ ] AC1: Đúng giờ → notification popup + đọc bằng TTS: "Đã đến giờ uống thuốc [Tên thuốc], [Liều lượng]"
- [ ] AC2: Giọng đọc rõ ràng, tốc độ vừa phải
- [ ] AC3: Có thể replay giọng đọc bằng cách tap vào notification
- [ ] AC4: 2 nút hành động: [✅ Đã uống] và [⏭ Bỏ qua]
- [ ] AC5: Notification bật âm thanh dù máy để im lặng

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🔴 Critical | 5 | Sprint 3 | Medication |

---

### US-014: Cảnh báo người thân khi bỏ lỡ thuốc

**As a** người thân,  
**I want** nhận thông báo khi cha/mẹ không xác nhận uống thuốc đúng giờ,  
**So that** tôi có thể nhắc nhở hoặc kiểm tra trực tiếp.

**Acceptance Criteria:**
- [ ] AC1: Sau 30 phút không phản hồi → push notification đến người thân
- [ ] AC2: Nội dung: "[Tên] chưa xác nhận uống [Thuốc] lúc [Giờ]"
- [ ] AC3: Người thân xem lịch sử uống thuốc của ngày hôm nay
- [ ] AC4: Biểu đồ tỷ lệ tuân thủ theo tuần/tháng

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🔴 Critical | 3 | Sprint 3 | Medication |

---

## 8.7 Epic 6: Dashboard & Theo dõi từ xa

---

### US-015: Xem Dashboard tổng quan người thân

**As a** người thân,  
**I want** mở app và thấy ngay tình trạng tổng quan của cha/mẹ,  
**So that** tôi nhanh chóng biết có điều gì bất thường không.

**Acceptance Criteria:**
- [ ] AC1: Hiển thị danh sách người cao tuổi đang theo dõi
- [ ] AC2: Mỗi card gồm: ảnh, tên, trạng thái online, lần hoạt động cuối
- [ ] AC3: Chỉ số sức khỏe mới nhất (HA, nhịp tim, đường huyết)
- [ ] AC4: Trạng thái uống thuốc hôm nay (% hoàn thành)
- [ ] AC5: Badge đỏ khi có SOS hoặc chỉ số bất thường
- [ ] AC6: Mức pin điện thoại người cao tuổi
- [ ] AC7: Realtime updates không cần kéo refresh

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🔴 Critical | 8 | Sprint 4 | Dashboard |

---

### US-016: Xem vị trí GPS của người cao tuổi

**As a** người thân,  
**I want** xem vị trí hiện tại của cha/mẹ trên bản đồ,  
**So that** tôi biết họ đang ở đâu khi có sự cố.

**Acceptance Criteria:**
- [ ] AC1: Google Maps hiển thị marker vị trí người cao tuổi
- [ ] AC2: Hiển thị thời gian cập nhật vị trí gần nhất
- [ ] AC3: Marker xanh = vị trí cập nhật < 30 phút, xám = cũ hơn
- [ ] AC4: Nút điều hướng: mở Google Maps app để chỉ đường
- [ ] AC5: Lịch sử vị trí trong 24 giờ gần nhất

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🟠 High | 5 | Sprint 4 | GPS |

---

### US-017: Dashboard bác sĩ

**As a** bác sĩ,  
**I want** xem toàn bộ lịch sử sức khỏe và biểu đồ của bệnh nhân,  
**So that** tôi có thể theo dõi và tư vấn điều trị phù hợp.

**Acceptance Criteria:**
- [ ] AC1: Danh sách bệnh nhân đang theo dõi
- [ ] AC2: Chọn bệnh nhân → xem biểu đồ tất cả chỉ số
- [ ] AC3: Filter theo loại chỉ số và khoảng thời gian
- [ ] AC4: Highlight giá trị bất thường màu đỏ
- [ ] AC5: Thêm ghi chú tư vấn cho bệnh nhân
- [ ] AC6: Xuất báo cáo PDF cho bệnh nhân

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🟠 High | 8 | Sprint 4 | Dashboard |

---

## 8.8 Epic 7: Báo cáo & Xuất dữ liệu

---

### US-018: Xuất báo cáo sức khỏe PDF

**As a** người thân hoặc bác sĩ,  
**I want** xuất báo cáo sức khỏe định kỳ dưới dạng PDF,  
**So that** tôi có tài liệu mang đến gặp bác sĩ hoặc lưu trữ.

**Acceptance Criteria:**
- [ ] AC1: Chọn kỳ: Tuần / Tháng / Năm
- [ ] AC2: PDF gồm: thông tin hồ sơ, biểu đồ, bảng lịch sử, tỷ lệ uống thuốc
- [ ] AC3: Disclaimer y tế trên PDF
- [ ] AC4: Preview PDF trước khi lưu/chia sẻ
- [ ] AC5: Chia sẻ qua email, Zalo, Drive

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🟡 Medium | 5 | Sprint 5 | Report |

---

## 8.9 Epic 8: Cài đặt & Cá nhân hóa

---

### US-019: Cài đặt cỡ chữ và theme

**As a** người cao tuổi,  
**I want** điều chỉnh cỡ chữ và màu sắc ứng dụng,  
**So that** tôi đọc được nội dung dễ dàng hơn.

**Acceptance Criteria:**
- [ ] AC1: 4 mức cỡ chữ: Nhỏ / Vừa / Lớn / Rất lớn
- [ ] AC2: Dark Mode / Light Mode
- [ ] AC3: High Contrast Mode
- [ ] AC4: Thay đổi có hiệu lực ngay, không cần khởi động lại

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🟠 High | 3 | Sprint 2 | Settings |

---

### US-020: Cài đặt thông báo

**As a** người thân,  
**I want** tuỳ chỉnh loại thông báo muốn nhận,  
**So that** tôi không bị làm phiền bởi các thông báo không quan trọng.

**Acceptance Criteria:**
- [ ] AC1: Bật/tắt: Cảnh báo sức khỏe, Nhắc thuốc, Pin thấp, Báo cáo ngày
- [ ] AC2: **Không thể tắt** SOS notification (hiển thị khoá, giải thích lý do)
- [ ] AC3: Cài giờ im lặng (Do Not Disturb) cho thông báo thấp
- [ ] AC4: Thay đổi có hiệu lực ngay

| Priority | SP | Sprint | Epic |
|---|---|---|---|
| 🟡 Medium | 3 | Sprint 4 | Settings |

---

## 8.10 Sprint Planning Summary

| Sprint | US | Tổng SP | Mục tiêu |
|---|---|---|---|
| Sprint 1 | US-001, 002, 003, 004 | 12 | Auth + Hồ sơ cơ bản |
| Sprint 2 | US-005, 006, 007, 008, 019 | 27 | SOS + Settings |
| Sprint 3 | US-009, 010, 011, 012, 013, 014 | 28 | Health + Medication |
| Sprint 4 | US-015, 016, 017, 020 | 24 | Dashboard + GPS |
| Sprint 5 | US-018 + Refine | 10 | Report + Polish |

**Tổng: ~101 Story Points cho MVP**

---

## 8.11 Backlog Refinement – User Stories chưa ưu tiên (V2)

| ID | User Story | SP |
|---|---|---|
| US-021 | Người cao tuổi chat text với người thân | 8 |
| US-022 | Gọi video khẩn cấp 1 chạm | 13 |
| US-023 | Đồng bộ dữ liệu từ vòng đeo tay sức khỏe | 13 |
| US-024 | AI phân tích xu hướng sức khỏe | 21 |
| US-025 | Đặt lịch khám qua ứng dụng | 13 |
