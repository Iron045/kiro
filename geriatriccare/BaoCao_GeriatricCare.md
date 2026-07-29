# BÁO CÁO DỰ ÁN
# BTL09 – GERIATRICCARE
## Hệ thống Giám sát & Chăm sóc Người cao tuổi Độc cư

---

| Thông tin | Nội dung |
|---|---|
| **Học phần** | CSE441 – Phát triển ứng dụng di động (Flutter) |
| **Nhóm lĩnh vực** | HealthTech & Social |
| **Nền tảng** | Mobile/Tablet App + Web Dashboard |
| **Công nghệ chính** | Flutter, Firebase (Firestore, FCM, Auth) |
| **Ngày báo cáo** | 25/07/2026 |

---

## MỤC LỤC

1. [Tổng quan dự án](#1-tổng-quan-dự-án)
2. [Mục tiêu và Phạm vi](#2-mục-tiêu-và-phạm-vi)
3. [Yêu cầu chức năng](#3-yêu-cầu-chức-năng)
4. [Yêu cầu phi chức năng](#4-yêu-cầu-phi-chức-năng)
5. [Kiến trúc hệ thống](#5-kiến-trúc-hệ-thống)
6. [Mô hình dữ liệu](#6-mô-hình-dữ-liệu)
7. [Luồng giao diện](#7-luồng-giao-diện)
8. [Công nghệ sử dụng](#8-công-nghệ-sử-dụng)
9. [Các điểm kỹ thuật quan trọng](#9-các-điểm-kỹ-thuật-quan-trọng)
10. [Kết luận](#10-kết-luận)

---

## 1. TỔNG QUAN DỰ ÁN

**GeriatricCare** là ứng dụng di động và web dashboard hỗ trợ giám sát, chăm sóc người cao tuổi sống một mình.
Hệ thống kết nối ba nhóm người dùng: **người cao tuổi**, **con cái/người chăm sóc**, và **bác sĩ gia đình**,
tạo thành vòng theo dõi sức khỏe liên tục và tức thời từ xa.

### Bối cảnh và vấn đề

Người cao tuổi ngày càng sống độc lập trong khi con cái bận công việc và bác sĩ không thể theo dõi thường xuyên.
Các nguy cơ chính gồm: té ngã không ai biết, bỏ sót liều thuốc, và chỉ số sức khỏe bất thường không được phát hiện kịp thời.

GeriatricCare giải quyết ba bài toán này thông qua:
- Cơ chế SOS khẩn cấp kích hoạt cả bằng nút và bằng lắc điện thoại.
- Nhắc uống thuốc bằng giọng nói tiếng Việt, có fallback offline.
- Theo dõi chỉ số sức khỏe hằng ngày với cảnh báo ngưỡng tự động.

---

## 2. MỤC TIÊU VÀ PHẠM VI

### 2.1 Mục tiêu dự án

- **Cảnh báo khẩn cấp đáng tin cậy**: Nút SOS + shake detection, gửi tức thì đến con cái và bác sĩ kèm GPS.
- **Tuân thủ điều trị**: Nhắc uống thuốc bằng TTS tiếng Việt, theo dõi xác nhận, gửi thông báo nếu bỏ sót.
- **Giám sát sức khỏe từ xa**: Con cái xem biểu đồ huyết áp, đường huyết theo thời gian thực.
- **Hỗ trợ bác sĩ**: Dashboard giám sát nhiều bệnh nhân, phát hiện bất thường sớm, ghi chú khuyến nghị.

### 2.2 Phạm vi hệ thống

Hệ thống bao gồm 3 phân hệ độc lập, kết nối qua Firebase:

| Phân hệ | Nền tảng | Đối tượng sử dụng | Chức năng chính |
|---|---|---|---|
| **Elder App** | Mobile/Tablet (Android/iOS) | Người cao tuổi | SOS, Nhắc thuốc, Ghi chỉ số sức khỏe |
| **Child App** | Mobile (Android/iOS) | Con cái, người chăm sóc | Nhận alert, Theo dõi sức khỏe, Quản lý lịch thuốc |
| **Doctor Dashboard** | Web Browser | Bác sĩ gia đình, phòng khám | Giám sát nhiều bệnh nhân, Ghi chú, Cảnh báo |

### 2.3 Giới hạn phạm vi (Out of Scope)

- Không tích hợp thiết bị đo sức khỏe tự động (smartwatch, blood pressure monitor).
- Không hỗ trợ video call trực tiếp trong app (chỉ có shortcut gọi điện).
- Không có AI phân tích xu hướng sức khỏe ở phiên bản này.

---

## 3. YÊU CẦU CHỨC NĂNG

### 3.1 Phân hệ Người cao tuổi (Elder App)

Giao diện được thiết kế tối giản với font chữ lớn, nút bấm rộng, phù hợp với người cao tuổi có thị lực kém.

#### US-01: Nhấn nút khẩn cấp SOS

**Mô tả**: Người cao tuổi cần nút khẩn cấp dễ tìm, dễ nhấn để kêu cứu khi gặp tai nạn hoặc sự cố sức khỏe.

**Acceptance Criteria**:

| # | WHEN (Điều kiện) | THEN (Kết quả mong đợi) |
|---|---|---|
| 1 | Màn hình chính mở | Nút SOS màu đỏ chiếm ≥ 30% diện tích, không thể ẩn |
| 2 | Nhấn giữ nút SOS 2 giây | Đếm ngược 5 giây kèm âm thanh beep, cho phép hủy |
| 3 | Không hủy trong 5 giây | Gửi FCM high-priority đến tất cả liên hệ khẩn + bác sĩ kèm GPS |
| 4 | Lắc điện thoại mạnh | Kích hoạt toàn bộ quy trình SOS tương đương nhấn nút |

**Luồng xử lý**:
```
Nhấn giữ SOS 2s → Đếm ngược 5s → [Hủy?] → No → Lấy GPS → Firestore: SosAlert 
→ Cloud Function → FCM → Con cái (fullscreen alert) + Bác sĩ
```
