# Chương 1: Business Analysis
## GeriatricCare – Hệ thống Giám sát & Chăm sóc Người cao tuổi Độc cư

---

## 1.1 Tổng quan dự án

| Thông tin | Chi tiết |
|---|---|
| Tên dự án | GeriatricCare |
| Loại sản phẩm | Mobile Application (Flutter) |
| Backend | Firebase (Firestore, Auth, FCM, Storage) |
| Phiên bản tài liệu | 1.0.0 |
| Ngày lập | 2026-07-22 |
| Trạng thái | Draft |

---

## 1.2 Bối cảnh kinh doanh (Business Context)

### 1.2.1 Xu hướng nhân khẩu học Việt Nam

Việt Nam đang bước vào giai đoạn "già hóa dân số nhanh". Theo thống kê:

- Tỷ lệ người từ 60 tuổi trở lên chiếm hơn 11% dân số (khoảng 11 triệu người).
- Dự kiến đến 2038, Việt Nam trở thành "dân số già" với tỷ lệ trên 20%.
- Tốc độ già hóa nhanh nhất khu vực Đông Nam Á.

### 1.2.2 Vấn đề thực tiễn

Người cao tuổi sống độc lập đối mặt với nhiều nguy cơ:

| Rủi ro | Hậu quả nếu phát hiện muộn |
|---|---|
| Té ngã | Gãy xương, xuất huyết não |
| Đột quỵ | Tàn phế, tử vong trong 4.5 giờ "vàng" |
| Hạ đường huyết | Hôn mê, tổn thương não |
| Quên uống thuốc | Bệnh mãn tính trở nặng |
| Không liên lạc được | Không kịp can thiệp y tế |

> **Phát hiện chậm 15–30 phút có thể gây hậu quả không thể phục hồi.**

### 1.2.3 Khoảng trống thị trường

| Giải pháp hiện tại | Hạn chế |
|---|---|
| Camera giám sát | Vi phạm quyền riêng tư, không có cảnh báo thông minh |
| Điện thoại thông thường | Không có cơ chế tự động, người già khó dùng |
| Vòng đeo tay y tế | Chi phí cao, hạn chế tính năng |
| Viện dưỡng lão | Không phù hợp văn hóa Việt Nam |

**GeriatricCare** lấp đầy khoảng trống này bằng giải pháp mobile app toàn diện, dễ dùng, chi phí thấp.

---

## 1.3 Mô hình kinh doanh (Business Model)

### 1.3.1 Đề xuất giá trị (Value Proposition)

```
┌─────────────────────────────────────────────────────────────────┐
│                     VALUE PROPOSITION                           │
├────────────────────┬────────────────────┬───────────────────────┤
│   Người cao tuổi   │    Người thân       │     Bác sĩ            │
├────────────────────┼────────────────────┼───────────────────────┤
│ • An toàn hơn      │ • Yên tâm hơn      │ • Theo dõi từ xa      │
│ • Nhắc uống thuốc  │ • Cảnh báo kịp thời│ • Lịch sử đầy đủ      │
│ • Gọi cứu giúp     │ • Biết vị trí      │ • Ra quyết định tốt   │
│   nhanh chóng      │ • Theo dõi 24/7    │   hơn                 │
└────────────────────┴────────────────────┴───────────────────────┘
```

### 1.3.2 Mô hình doanh thu

| Mô hình | Mô tả |
|---|---|
| Freemium | Miễn phí tính năng cơ bản, trả phí premium |
| B2C Subscription | Người thân trả phí theo tháng/năm |
| B2B | Bán giải pháp cho bệnh viện, trung tâm y tế |
| Partnership | Hợp tác bảo hiểm sức khỏe |

### 1.3.3 Phân tích cạnh tranh

| Tiêu chí | GeriatricCare | Ứng dụng theo dõi GPS | Camera | Thiết bị y tế |
|---|---|---|---|---|
| Chi phí | Thấp | Thấp | Trung bình | Cao |
| Dễ dùng | ★★★★★ | ★★★ | ★★★★ | ★★ |
| SOS | ✅ | ❌ | ❌ | ✅ |
| Nhắc thuốc | ✅ | ❌ | ❌ | ❌ |
| Sức khỏe | ✅ | ❌ | ❌ | ✅ |
| Riêng tư | ✅ | ✅ | ❌ | ✅ |

---

## 1.4 Mục tiêu kinh doanh (Business Objectives)

| ID | Mục tiêu | KPI | Thời hạn |
|---|---|---|---|
| BO-01 | Giảm thiểu rủi ro sức khỏe người cao tuổi | Thời gian phản hồi SOS < 2 phút | Q1/2027 |
| BO-02 | Nâng cao tỷ lệ tuân thủ uống thuốc | > 90% nhắc nhở được xác nhận | Q2/2027 |
| BO-03 | Kết nối người thân và người cao tuổi | DAU/MAU > 60% | Q2/2027 |
| BO-04 | Hỗ trợ bác sĩ theo dõi từ xa | > 80% bác sĩ hài lòng | Q3/2027 |
| BO-05 | Mở rộng người dùng | 10,000 MAU trong 6 tháng đầu | Q4/2027 |

---

## 1.5 Phạm vi dự án (Project Scope)

### 1.5.1 Trong phạm vi (In Scope)

- ✅ Ứng dụng Flutter cho Android
- ✅ Backend Firebase (Firestore, Auth, FCM, Storage)
- ✅ Quản lý hồ sơ người cao tuổi, bác sĩ, người chăm sóc
- ✅ Panic Button / SOS với GPS
- ✅ Shake Detection tự động kích hoạt SOS
- ✅ Theo dõi chỉ số sức khỏe (HA, đường huyết, nhịp tim, SpO2...)
- ✅ Nhắc uống thuốc với giọng nói tiếng Việt
- ✅ Dashboard người thân
- ✅ Dashboard bác sĩ
- ✅ Bản đồ Google Maps + GPS tracking
- ✅ Báo cáo PDF/Excel
- ✅ Nhật ký hoạt động
- ✅ Dark Mode, Accessibility, Font lớn
- ✅ Role-Based Access Control

### 1.5.2 Ngoài phạm vi (Out of Scope)

- ❌ Ứng dụng Web / Website
- ❌ Hệ thống ERP
- ❌ Quản lý bệnh viện / phòng khám
- ❌ iOS (giai đoạn 1)
- ❌ AI Fall Detection (đề xuất giai đoạn sau)
- ❌ Smart Watch integration (đề xuất giai đoạn sau)
- ❌ Video call (đề xuất giai đoạn sau)

---

## 1.6 Giả định và ràng buộc (Assumptions & Constraints)

### 1.6.1 Giả định (Assumptions)

| ID | Giả định |
|---|---|
| A-01 | Người cao tuổi sử dụng Android smartphone |
| A-02 | Có kết nối Internet (3G/4G/WiFi) khi sử dụng tính năng real-time |
| A-03 | Người thân cài đặt ứng dụng và cấu hình ban đầu thay người cao tuổi |
| A-04 | Firebase đáp ứng được SLA 99.9% uptime |
| A-05 | Google Maps API khả dụng tại Việt Nam |
| A-06 | Người dùng đồng ý cấp quyền vị trí, microphone, thông báo |
| A-07 | Dữ liệu sức khỏe nhập thủ công (không kết nối thiết bị đo tự động giai đoạn 1) |

### 1.6.2 Ràng buộc (Constraints)

| ID | Ràng buộc | Loại |
|---|---|---|
| C-01 | Ngân sách giới hạn → dùng Firebase thay backend tự build | Kinh tế |
| C-02 | Dữ liệu y tế phải tuân thủ quy định bảo mật | Pháp lý |
| C-03 | Giao diện phải tối ưu cho người cao tuổi (font ≥ 18sp) | Kỹ thuật |
| C-04 | Ứng dụng phải hoạt động offline một phần | Kỹ thuật |
| C-05 | Thời gian gửi SOS không quá 3 giây | Kỹ thuật |

---

## 1.7 Rủi ro kinh doanh (Business Risks)

| ID | Rủi ro | Xác suất | Tác động | Chiến lược giảm thiểu |
|---|---|---|---|---|
| R-01 | Người cao tuổi không chịu dùng app | Cao | Cao | UX cực đơn giản, hỗ trợ giọng nói |
| R-02 | GPS không chính xác trong nhà | Trung bình | Trung bình | Kết hợp WiFi positioning |
| R-03 | Pin điện thoại cạn → không gửi SOS | Cao | Rất cao | Cảnh báo khi pin < 20% |
| R-04 | Thông báo bị tắt bởi hệ thống | Trung bình | Cao | Foreground service, hướng dẫn cấu hình |
| R-05 | Dữ liệu y tế bị rò rỉ | Thấp | Rất cao | Mã hóa, Firebase Rules nghiêm ngặt |
| R-06 | Chi phí Firebase vượt ngân sách | Trung bình | Trung bình | Tối ưu reads/writes, caching |

---

## 1.8 Các bên liên quan chính (Key Stakeholders)

| Vai trò | Mô tả | Mức độ ảnh hưởng |
|---|---|---|
| Product Owner | Người định hướng sản phẩm | Rất cao |
| Flutter Developer Team | Nhóm xây dựng ứng dụng | Cao |
| Người cao tuổi | Người dùng cuối chính | Rất cao |
| Người thân / Người chăm sóc | Người dùng cuối | Rất cao |
| Bác sĩ / Điều dưỡng | Người dùng chuyên nghiệp | Cao |
| Quản trị viên hệ thống | Vận hành hệ thống | Trung bình |
| Firebase (Google) | Nhà cung cấp hạ tầng | Cao |

---

## 1.9 Định nghĩa thuật ngữ (Glossary)

| Thuật ngữ | Định nghĩa |
|---|---|
| SOS / Panic Button | Nút khẩn cấp gửi cảnh báo tức thời |
| Shake Detection | Phát hiện lắc điện thoại để kích hoạt SOS |
| SpO2 | Độ bão hòa oxy trong máu |
| FCM | Firebase Cloud Messaging – dịch vụ push notification |
| TTS | Text-to-Speech – chuyển văn bản thành giọng nói |
| RBAC | Role-Based Access Control – phân quyền theo vai trò |
| DAU | Daily Active Users |
| MAU | Monthly Active Users |
| Flutter | Framework phát triển cross-platform của Google |
| Riverpod | Thư viện quản lý state cho Flutter |
| Clean Architecture | Kiến trúc phân tầng tách biệt domain/data/presentation |
