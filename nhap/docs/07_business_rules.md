# Chương 7: Business Rules
## GeriatricCare – Quy tắc nghiệp vụ

---

## 7.1 Tổng quan

Business Rules là các ràng buộc nghiệp vụ bất biến mà hệ thống phải tuân thủ bất kể context. Chúng khác với Validation Rules (kiểm tra đầu vào) và không phụ thuộc vào công nghệ triển khai.

---

## 7.2 Nhóm BR-AUTH: Xác thực & Phân quyền

| ID | Quy tắc | Mức độ | Lý do |
|---|---|---|---|
| BR-AUTH-01 | Mỗi địa chỉ email chỉ được đăng ký một tài khoản duy nhất | Bắt buộc | Tránh trùng lặp, bảo mật |
| BR-AUTH-02 | Mật khẩu không bao giờ được lưu hoặc hiển thị dạng plain text | Bắt buộc | Bảo mật |
| BR-AUTH-03 | Phiên đăng nhập hết hạn sau 30 ngày không hoạt động | Bắt buộc | Bảo mật |
| BR-AUTH-04 | Tài khoản bị khóa sau 5 lần đăng nhập sai liên tiếp trong 15 phút | Bắt buộc | Chống brute force |
| BR-AUTH-05 | Mỗi người dùng chỉ có một vai trò chính (ELDERLY / FAMILY / DOCTOR / ADMIN) | Bắt buộc | RBAC rõ ràng |
| BR-AUTH-06 | Người dùng chỉ được truy cập dữ liệu trong phạm vi vai trò của mình | Bắt buộc | Bảo mật & Riêng tư |
| BR-AUTH-07 | Admin có thể xem tất cả dữ liệu nhưng không được thay đổi dữ liệu y tế | Bắt buộc | Toàn vẹn dữ liệu |
| BR-AUTH-08 | Tài khoản chưa xác thực email vẫn có thể dùng ứng dụng nhưng bị giới hạn tính năng | Tuỳ chọn | UX |

---

## 7.3 Nhóm BR-LINK: Liên kết người dùng

| ID | Quy tắc | Mức độ | Lý do |
|---|---|---|---|
| BR-LINK-01 | Một người cao tuổi có thể được liên kết với tối đa **10 người thân** | Bắt buộc | Giới hạn notification spam |
| BR-LINK-02 | Một người thân có thể theo dõi tối đa **10 người cao tuổi** | Bắt buộc | UX & performance |
| BR-LINK-03 | Liên kết bác sĩ – người cao tuổi phải được người thân (hoặc người cao tuổi) chấp thuận | Bắt buộc | Quyền riêng tư |
| BR-LINK-04 | Mã liên kết có hiệu lực trong **24 giờ** và chỉ dùng được **1 lần** | Bắt buộc | Bảo mật |
| BR-LINK-05 | Người thân có thể huỷ liên kết bất kỳ lúc nào | Bắt buộc | Quyền kiểm soát |
| BR-LINK-06 | Khi huỷ liên kết, lịch sử dữ liệu đã chia sẻ vẫn được giữ lại | Bắt buộc | Toàn vẹn dữ liệu |

---

## 7.4 Nhóm BR-SOS: Cảnh báo khẩn cấp

| ID | Quy tắc | Mức độ | Lý do |
|---|---|---|---|
| BR-SOS-01 | SOS phải được gửi trong **vòng 3 giây** sau khi đếm ngược kết thúc | Bắt buộc | An toàn tính mạng |
| BR-SOS-02 | Khi gửi SOS, thông báo phải đến **ít nhất 1 người thân hoặc người liên hệ khẩn cấp** | Bắt buộc | An toàn tính mạng |
| BR-SOS-03 | Mỗi lần SOS (kể cả bị hủy) đều phải được ghi vào Activity Log | Bắt buộc | Audit |
| BR-SOS-04 | SOS không thể bị vô hiệu hóa hoàn toàn bởi người dùng thông thường (chỉ Admin) | Bắt buộc | An toàn tính mạng |
| BR-SOS-05 | Nếu không có GPS, SOS vẫn được gửi kèm thông báo "Không xác định được vị trí" | Bắt buộc | An toàn tính mạng |
| BR-SOS-06 | Âm thanh báo động SOS không thể bị tắt bởi nút volume vật lý | Bắt buộc | An toàn tính mạng |
| BR-SOS-07 | Khi pin < 15%, hệ thống gửi cảnh báo pin thấp đến người thân **trước** khi tắt GPS | Bắt buộc | Phòng ngừa |
| BR-SOS-08 | Người thân nhận thông báo SOS phải xác nhận "Đã tiếp nhận" để tắt chuông nhắc lặp | Nên có | UX |

---

## 7.5 Nhóm BR-MED: Nhắc uống thuốc

| ID | Quy tắc | Mức độ | Lý do |
|---|---|---|---|
| BR-MED-01 | Lịch nhắc thuốc do người thân hoặc bác sĩ tạo; người cao tuổi chỉ xem và xác nhận | Bắt buộc | Tránh tự ý thay đổi |
| BR-MED-02 | Nếu người cao tuổi không phản hồi trong **30 phút** sau giờ uống thuốc, gửi cảnh báo cho người thân | Bắt buộc | An toàn sức khỏe |
| BR-MED-03 | Lịch sử uống thuốc không thể xóa hoặc chỉnh sửa sau khi đã xác nhận | Bắt buộc | Toàn vẹn dữ liệu |
| BR-MED-04 | Lịch nhắc không hoạt động vào các ngày nằm ngoài khoảng ngày bắt đầu – kết thúc | Bắt buộc | Logic nghiệp vụ |
| BR-MED-05 | Thông báo nhắc thuốc phải bật kèm âm thanh dù điện thoại để chế độ im lặng | Bắt buộc | An toàn sức khỏe |
| BR-MED-06 | Mỗi lịch nhắc phải có ít nhất 1 thời điểm trong ngày | Bắt buộc | Validation |
| BR-MED-07 | Tỷ lệ tuân thủ = (Số lần xác nhận "Đã uống") / (Tổng số lần nhắc) × 100% | Bắt buộc | Tính toán |

---

## 7.6 Nhóm BR-HLT: Sức khỏe

| ID | Quy tắc | Mức độ | Lý do |
|---|---|---|---|
| BR-HLT-01 | Chỉ số sức khỏe sau khi lưu không được xóa, chỉ được đánh dấu "không hợp lệ" | Bắt buộc | Toàn vẹn dữ liệu y tế |
| BR-HLT-02 | Khi chỉ số vượt ngưỡng cảnh báo, hệ thống tự động gửi alert trong vòng **5 giây** | Bắt buộc | An toàn sức khỏe |
| BR-HLT-03 | Bác sĩ chỉ xem dữ liệu sức khỏe của bệnh nhân đã được liên kết và chấp thuận | Bắt buộc | Quyền riêng tư |
| BR-HLT-04 | Ngưỡng cảnh báo có thể được bác sĩ điều chỉnh cho từng bệnh nhân | Nên có | Clinical flexibility |
| BR-HLT-05 | Dữ liệu sức khỏe nhập offline phải được đồng bộ với timestamp gốc (lúc nhập), không phải timestamp sync | Bắt buộc | Chính xác dữ liệu |
| BR-HLT-06 | Chỉ số SpO2 < 90% phải kích hoạt cảnh báo **ngay lập tức** không cần chờ confirm | Bắt buộc | Emergency |
| BR-HLT-07 | Không cho phép nhập chỉ số tương lai (timestamp > now + 5 phút) | Bắt buộc | Validation |

---

## 7.7 Nhóm BR-GPS: Định vị

| ID | Quy tắc | Mức độ | Lý do |
|---|---|---|---|
| BR-GPS-01 | Vị trí GPS chỉ được chia sẻ với người thân đã được liên kết | Bắt buộc | Quyền riêng tư |
| BR-GPS-02 | Người cao tuổi luôn được thông báo khi GPS tracking đang bật | Bắt buộc | Transparency |
| BR-GPS-03 | GPS tracking tự động dừng khi pin < 10% (ưu tiên SOS) | Bắt buộc | Bảo tồn pin |
| BR-GPS-04 | Vị trí lưu trữ tối đa **30 ngày**, sau đó tự động xóa | Bắt buộc | Privacy & Storage |
| BR-GPS-05 | Người cao tuổi có thể tạm dừng GPS tracking từ cài đặt | Bắt buộc | Quyền kiểm soát |

---

## 7.8 Nhóm BR-DATA: Dữ liệu & Lưu trữ

| ID | Quy tắc | Mức độ | Lý do |
|---|---|---|---|
| BR-DATA-01 | Dữ liệu sức khỏe được lưu trữ tối thiểu **5 năm** | Bắt buộc | Y tế |
| BR-DATA-02 | Activity Log được lưu trữ tối thiểu **12 tháng** | Bắt buộc | Audit |
| BR-DATA-03 | Khi người dùng xóa tài khoản, dữ liệu cá nhân bị ẩn danh hóa trong **30 ngày** trước khi xóa vĩnh viễn | Bắt buộc | GDPR-like |
| BR-DATA-04 | Báo cáo xuất ra không được chứa thông tin định danh của người thứ ba (ngoài người cao tuổi) | Bắt buộc | Privacy |
| BR-DATA-05 | Mỗi lần đồng bộ Firestore phải có timestamp và device ID | Bắt buộc | Conflict resolution |

---

## 7.9 Nhóm BR-NOT: Thông báo

| ID | Quy tắc | Mức độ | Lý do |
|---|---|---|---|
| BR-NOT-01 | Thông báo SOS có độ ưu tiên cao nhất (HIGH priority FCM), không thể bị batching | Bắt buộc | An toàn tính mạng |
| BR-NOT-02 | Người dùng có thể tắt các loại thông báo không quan trọng nhưng **không thể tắt SOS notification** | Bắt buộc | An toàn |
| BR-NOT-03 | Thông báo nhắc thuốc phải được gửi ngay đúng giờ (±1 phút) | Bắt buộc | Hiệu quả điều trị |
| BR-NOT-04 | Tối đa **5 thông báo Health Alert** mỗi giờ cho mỗi người thân (chống spam) | Bắt buộc | UX |

---

## 7.10 Nhóm BR-RPT: Báo cáo

| ID | Quy tắc | Mức độ | Lý do |
|---|---|---|---|
| BR-RPT-01 | Báo cáo PDF phải có watermark "GeriatricCare – Dữ liệu tham khảo, không thay thế chỉ định y tế" | Bắt buộc | Legal disclaimer |
| BR-RPT-02 | Báo cáo chỉ có thể xuất cho khoảng thời gian có dữ liệu | Bắt buộc | Logic |
| BR-RPT-03 | Chỉ người thân và bác sĩ liên kết mới được xuất báo cáo của người cao tuổi | Bắt buộc | Privacy |

---

## 7.11 Ma trận tác động Business Rules

```
Business Rule         │ Auth │ SOS │ Med │ Health │ GPS │ Data
──────────────────────┼──────┼─────┼─────┼────────┼─────┼──────
RBAC (BR-AUTH-05/06)  │  ✅  │  ✅ │  ✅ │   ✅   │  ✅ │  ✅
Link Approval         │      │  ✅ │  ✅ │   ✅   │  ✅ │  ✅
SOS Always On         │      │  ✅ │     │        │     │
Immutable Health Data │      │     │  ✅ │   ✅   │     │  ✅
Offline + Sync        │      │  ✅ │  ✅ │   ✅   │  ✅ │  ✅
Privacy Consent       │  ✅  │     │     │   ✅   │  ✅ │  ✅
Data Retention        │      │  ✅ │  ✅ │   ✅   │  ✅ │  ✅
```
