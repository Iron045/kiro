---
title: "BÁO CÁO ĐỒ ÁN TỐT NGHIỆP"
geometry: margin=3cm
fontsize: 13pt
linestretch: 1.5
---

# TRƯỜNG ĐẠI HỌC [TÊN TRƯỜNG]
# KHOA CÔNG NGHỆ THÔNG TIN

---

&nbsp;

&nbsp;

# BÁO CÁO ĐỒ ÁN TỐT NGHIỆP

&nbsp;

## ĐỀ TÀI

# GERIATRICCARE – XÂY DỰNG HỆ THỐNG GIÁM SÁT VÀ CHĂM SÓC NGƯỜI CAO TUỔI ĐỘC CƯ SỬ DỤNG CÔNG NGHỆ FLUTTER VÀ FIREBASE

&nbsp;

&nbsp;

| | |
|---|---|
| **Sinh viên thực hiện** | Nguyễn Văn A – MSSV: [MSSV] |
| **Lớp** | [Tên lớp] |
| **Giảng viên hướng dẫn** | TS./ThS. [Tên GVHD] |

&nbsp;

&nbsp;

**[Thành phố], tháng [XX] năm [XXXX]**

---

&nbsp;

# LỜI CAM ĐOAN

Tôi xin cam đoan đây là công trình nghiên cứu của riêng tôi dưới sự hướng dẫn của giảng viên hướng dẫn. Các số liệu và kết quả trong báo cáo là trung thực và chưa được công bố trong bất kỳ công trình nào khác.

Tất cả các tài liệu tham khảo đều được trích dẫn đầy đủ theo quy định.

&nbsp;

*[Thành phố], ngày ... tháng ... năm ...*

**Sinh viên thực hiện**

*(Ký và ghi rõ họ tên)*

---

&nbsp;

# LỜI CẢM ƠN

Lời đầu tiên, tôi xin bày tỏ lòng biết ơn sâu sắc đến **TS./ThS. [Tên GVHD]** – giảng viên hướng dẫn đồ án tốt nghiệp, người đã tận tình chỉ bảo, định hướng và đồng hành cùng tôi trong suốt quá trình thực hiện đề tài.

Tôi xin chân thành cảm ơn **Quý thầy cô Khoa Công nghệ Thông tin, Trường Đại học [Tên trường]** đã truyền đạt kiến thức nền tảng và kỹ năng chuyên môn trong những năm học vừa qua, tạo điều kiện để tôi có thể hoàn thành đồ án này.

Tôi cũng xin gửi lời cảm ơn đến **gia đình và bạn bè** đã luôn động viên, hỗ trợ tinh thần trong suốt quá trình học tập và nghiên cứu.

Mặc dù đã cố gắng hết sức, báo cáo này không thể tránh khỏi những thiếu sót. Tôi rất mong nhận được sự góp ý từ Quý thầy cô và các bạn để hoàn thiện hơn.

Xin trân trọng cảm ơn!

&nbsp;

*[Thành phố], ngày ... tháng ... năm ...*

**Sinh viên thực hiện**

*(Ký và ghi rõ họ tên)*

---

&nbsp;

# TÓM TẮT ĐỒ ÁN

**Tên đề tài:** GeriatricCare – Xây dựng hệ thống giám sát và chăm sóc người cao tuổi độc cư sử dụng công nghệ Flutter và Firebase

**Từ khóa:** Flutter, Firebase, người cao tuổi, SOS, theo dõi sức khỏe, nhắc uống thuốc, GPS, Clean Architecture, Riverpod

---

## Tóm tắt tiếng Việt

Việt Nam đang bước vào giai đoạn già hóa dân số với tốc độ nhanh nhất khu vực Đông Nam Á. Ngày càng nhiều người cao tuổi sống một mình, đối mặt với những rủi ro sức khỏe nghiêm trọng như té ngã, đột quỵ, quên uống thuốc mà không có ai phát hiện kịp thời.

Đồ án này trình bày quá trình phân tích, thiết kế và xây dựng ứng dụng di động **GeriatricCare** trên nền tảng Android sử dụng framework **Flutter** và backend **Firebase**. Hệ thống cung cấp các chức năng cốt lõi: nút SOS khẩn cấp với phát hiện lắc điện thoại, theo dõi chỉ số sức khỏe (huyết áp, đường huyết, nhịp tim, SpO2), nhắc uống thuốc bằng giọng nói tiếng Việt, theo dõi vị trí GPS, dashboard thời gian thực cho người thân và bác sĩ.

Ứng dụng được thiết kế theo nguyên tắc **Senior-first UX** với giao diện tối giản, font chữ lớn, màu sắc tương phản cao, đáp ứng tiêu chuẩn WCAG AA về khả năng tiếp cận. Kiến trúc phần mềm áp dụng **Clean Architecture** kết hợp **SOLID Principles** và quản lý trạng thái bằng **Riverpod**, đảm bảo khả năng bảo trì và mở rộng.

Kết quả đồ án là một ứng dụng hoàn chỉnh với 10 module chức năng, đáp ứng 52 yêu cầu chức năng Must-Have, thời gian gửi SOS dưới 3 giây, hỗ trợ hoạt động offline và đồng bộ tự động.

---

## Abstract (English)

Vietnam is experiencing one of the fastest population aging rates in Southeast Asia. An increasing number of elderly people living alone face serious health risks such as falls, stroke, and medication non-compliance without timely detection.

This thesis presents the analysis, design, and development of the **GeriatricCare** mobile application for Android, built with the **Flutter** framework and **Firebase** backend. The system provides core features: emergency SOS button with shake detection, health monitoring (blood pressure, glucose, heart rate, SpO2), Vietnamese voice medication reminders, GPS location tracking, and real-time dashboards for family members and doctors.

The application is designed with **Senior-first UX** principles featuring a minimalist interface, large fonts, high-contrast colors, and WCAG AA accessibility compliance. The software architecture follows **Clean Architecture** with **SOLID Principles** and **Riverpod** state management, ensuring maintainability and scalability.

The deliverable is a complete application with 10 functional modules, meeting 52 Must-Have functional requirements, SOS delivery time under 3 seconds, with offline support and automatic synchronization.

---

&nbsp;

# MỤC LỤC

- [LỜI CAM ĐOAN](#lời-cam-đoan)
- [LỜI CẢM ƠN](#lời-cảm-ơn)
- [TÓM TẮT ĐỒ ÁN](#tóm-tắt-đồ-án)
- [DANH MỤC BẢNG BIỂU](#danh-mục-bảng-biểu)
- [DANH MỤC HÌNH VẼ VÀ SƠ ĐỒ](#danh-mục-hình-vẽ-và-sơ-đồ)
- [DANH MỤC TỪ VIẾT TẮT](#danh-mục-từ-viết-tắt)
- **[CHƯƠNG 1: TỔNG QUAN ĐỀ TÀI](#chương-1)**
  - 1.1 Đặt vấn đề
  - 1.2 Mục tiêu đề tài
  - 1.3 Đối tượng và phạm vi nghiên cứu
  - 1.4 Phương pháp nghiên cứu
  - 1.5 Ý nghĩa khoa học và thực tiễn
  - 1.6 Cấu trúc báo cáo
- **[CHƯƠNG 2: CƠ SỞ LÝ THUYẾT VÀ CÔNG NGHỆ](#chương-2)**
  - 2.1 Lập trình hướng đối tượng
  - 2.2 Các mô hình kiến trúc phần mềm
  - 2.3 Flutter và Dart
  - 2.4 Firebase Platform
  - 2.5 Các công nghệ và thư viện bổ trợ
- **[CHƯƠNG 3: PHÂN TÍCH HỆ THỐNG](#chương-3)**
  - 3.1 Khảo sát hiện trạng
  - 3.2 Phân tích tác nhân và use case
  - 3.3 Yêu cầu chức năng
  - 3.4 Yêu cầu phi chức năng
  - 3.5 Đặc tả use case chi tiết
- **[CHƯƠNG 4: THIẾT KẾ HỆ THỐNG](#chương-4)**
  - 4.1 Kiến trúc tổng thể
  - 4.2 Thiết kế lớp (Class Diagram)
  - 4.3 Thiết kế cơ sở dữ liệu
  - 4.4 Thiết kế giao diện người dùng
  - 4.5 Thiết kế bảo mật
- **[CHƯƠNG 5: CÀI ĐẶT VÀ THỬ NGHIỆM](#chương-5)**
  - 5.1 Môi trường cài đặt
  - 5.2 Cài đặt các chức năng chính
  - 5.3 Kiểm thử hệ thống
  - 5.4 Đánh giá kết quả
- **[CHƯƠNG 6: KẾT LUẬN VÀ HƯỚNG PHÁT TRIỂN](#chương-6)**
  - 6.1 Kết quả đạt được
  - 6.2 Hạn chế
  - 6.3 Hướng phát triển
- [TÀI LIỆU THAM KHẢO](#tài-liệu-tham-khảo)
- [PHỤ LỤC](#phụ-lục)

---

&nbsp;

# DANH MỤC BẢNG BIỂU

| STT | Tên bảng | Trang |
|---|---|---|
| Bảng 1.1 | So sánh các giải pháp hiện có trên thị trường | Chương 1 |
| Bảng 1.2 | Các rủi ro khi người cao tuổi sống một mình | Chương 1 |
| Bảng 2.1 | So sánh Flutter với React Native và Kotlin Native | Chương 2 |
| Bảng 2.2 | Các dịch vụ Firebase sử dụng trong hệ thống | Chương 2 |
| Bảng 2.3 | Danh sách thư viện Flutter sử dụng | Chương 2 |
| Bảng 3.1 | Danh sách tác nhân hệ thống | Chương 3 |
| Bảng 3.2 | Danh sách yêu cầu chức năng theo module | Chương 3 |
| Bảng 3.3 | Phân loại yêu cầu theo MoSCoW | Chương 3 |
| Bảng 3.4 | Các chỉ số sức khỏe và ngưỡng cảnh báo | Chương 3 |
| Bảng 3.5 | Yêu cầu phi chức năng về hiệu năng | Chương 3 |
| Bảng 3.6 | Yêu cầu về bảo mật (OWASP Mobile Top 10) | Chương 3 |
| Bảng 4.1 | Thiết kế collection `users` | Chương 4 |
| Bảng 4.2 | Thiết kế collection `elderly_profiles` | Chương 4 |
| Bảng 4.3 | Thiết kế collection `health_records` | Chương 4 |
| Bảng 4.4 | Thiết kế collection `sos_events` | Chương 4 |
| Bảng 4.5 | Thiết kế collection `medication_schedules` | Chương 4 |
| Bảng 4.6 | Ma trận phân quyền RBAC | Chương 4 |
| Bảng 5.1 | Môi trường phát triển và kiểm thử | Chương 5 |
| Bảng 5.2 | Kết quả kiểm thử chức năng SOS | Chương 5 |
| Bảng 5.3 | Kết quả kiểm thử hiệu năng | Chương 5 |
| Bảng 5.4 | Kết quả UAT với người dùng thực | Chương 5 |

---

# DANH MỤC HÌNH VẼ VÀ SƠ ĐỒ

| STT | Tên hình | Trang |
|---|---|---|
| Hình 2.1 | Kiến trúc Clean Architecture tổng quan | Chương 2 |
| Hình 2.2 | Mô hình MVVM và Riverpod trong Flutter | Chương 2 |
| Hình 2.3 | Kiến trúc Firebase Platform | Chương 2 |
| Hình 3.1 | Sơ đồ use case tổng thể hệ thống | Chương 3 |
| Hình 3.2 | Sơ đồ hoạt động luồng kích hoạt SOS | Chương 3 |
| Hình 3.3 | Sơ đồ hoạt động luồng nhắc uống thuốc | Chương 3 |
| Hình 3.4 | Sơ đồ tuần tự SOS end-to-end | Chương 3 |
| Hình 3.5 | Sơ đồ tuần tự nhập chỉ số sức khỏe | Chương 3 |
| Hình 4.1 | Kiến trúc tổng thể ứng dụng GeriatricCare | Chương 4 |
| Hình 4.2 | Cấu trúc thư mục dự án Flutter (Feature-first) | Chương 4 |
| Hình 4.3 | Sơ đồ lớp Domain Layer | Chương 4 |
| Hình 4.4 | Sơ đồ lớp Repository Pattern | Chương 4 |
| Hình 4.5 | Sơ đồ quan hệ các collection Firestore | Chương 4 |
| Hình 4.6 | Sơ đồ trạng thái SOS Event | Chương 4 |
| Hình 4.7 | Sơ đồ trạng thái Medication Log | Chương 4 |
| Hình 4.8 | Wireframe màn hình chính (Elderly Home) | Chương 4 |
| Hình 4.9 | Wireframe màn hình đếm ngược SOS | Chương 4 |
| Hình 4.10 | Wireframe Family Dashboard | Chương 4 |
| Hình 4.11 | Mô hình bảo mật Defense in Depth | Chương 4 |
| Hình 5.1 | Sơ đồ triển khai ứng dụng | Chương 5 |
| Hình 5.2 | Giao diện màn hình đăng nhập | Chương 5 |
| Hình 5.3 | Giao diện màn hình chính người cao tuổi | Chương 5 |
| Hình 5.4 | Giao diện màn hình SOS đang đếm ngược | Chương 5 |
| Hình 5.5 | Giao diện nhập chỉ số sức khỏe | Chương 5 |
| Hình 5.6 | Giao diện Family Dashboard | Chương 5 |
| Hình 5.7 | Biểu đồ kết quả kiểm thử hiệu năng | Chương 5 |

---

# DANH MỤC TỪ VIẾT TẮT

| Từ viết tắt | Nghĩa đầy đủ |
|---|---|
| API | Application Programming Interface |
| CSDL | Cơ sở dữ liệu |
| FCM | Firebase Cloud Messaging |
| GPS | Global Positioning System |
| IDE | Integrated Development Environment |
| JSON | JavaScript Object Notation |
| JWT | JSON Web Token |
| MVVM | Model-View-ViewModel |
| NFR | Non-Functional Requirement |
| OOP | Object-Oriented Programming |
| OTP | One-Time Password |
| RBAC | Role-Based Access Control |
| SDK | Software Development Kit |
| SMS | Short Message Service |
| SOS | Save Our Souls (tín hiệu cầu cứu) |
| SpO2 | Saturation of Peripheral Oxygen |
| TLS | Transport Layer Security |
| TTS | Text-to-Speech |
| UAT | User Acceptance Testing |
| UI/UX | User Interface / User Experience |
| WCAG | Web Content Accessibility Guidelines |
