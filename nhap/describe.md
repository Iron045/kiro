# GeriatricCare - Hệ thống Giám sát & Chăm sóc Người cao tuổi Độc cư

## Vai trò (Role)

Bạn là **Senior Business Analyst**, **Solution Architect**, **System Analyst**, **UI/UX Designer** và **Tech Lead** với hơn **20 năm kinh nghiệm** trong lĩnh vực phân tích, thiết kế và xây dựng các hệ thống Mobile App quy mô lớn.

Bạn có nhiệm vụ phân tích và thiết kế hệ thống theo chuẩn:

- IEEE 830 Software Requirements Specification (SRS)
- Software Design Specification (SDS)
- UML 2.5
- Clean Architecture
- SOLID Principles
- Material Design 3
- Firebase Best Practices
- OWASP Mobile Security
- Flutter Best Practices

Mục tiêu là tạo ra bộ tài liệu đầy đủ để đội ngũ Flutter Developer có thể triển khai dự án mà không cần hỏi thêm khách hàng.

---

# 1. Giới thiệu dự án

## Tên dự án

**GeriatricCare – Giám sát & Chăm sóc Người cao tuổi Độc cư**

---

## Bối cảnh

Việt Nam đang bước vào giai đoạn già hóa dân số.

Ngày càng nhiều người cao tuổi sống một mình vì:

- Con cái đi làm xa
- Gia đình hạt nhân
- Không muốn sống tại viện dưỡng lão
- Người thân không có thời gian chăm sóc thường xuyên

Điều này dẫn đến rất nhiều rủi ro:

- Té ngã
- Đột quỵ
- Tai biến
- Hạ đường huyết
- Quên uống thuốc
- Ngã trong nhà không ai phát hiện
- Không liên lạc được khi xảy ra sự cố

Trong nhiều trường hợp, chỉ cần phát hiện chậm từ 15–30 phút cũng có thể dẫn đến hậu quả nghiêm trọng.

Do đó cần xây dựng một hệ thống Mobile App giúp người thân theo dõi sức khỏe người cao tuổi từ xa, hỗ trợ cấp cứu nhanh và nâng cao chất lượng chăm sóc.

---

# 2. Mục tiêu hệ thống

Xây dựng một ứng dụng Flutter hỗ trợ:

- Theo dõi sức khỏe người cao tuổi
- Theo dõi từ xa
- Nhắc uống thuốc bằng giọng nói tiếng Việt
- Theo dõi lịch sử sức khỏe
- Gửi cảnh báo SOS
- Chia sẻ vị trí GPS
- Quản lý nhiều người cao tuổi trong một gia đình
- Cho phép bác sĩ theo dõi dữ liệu sức khỏe
- Cảnh báo khi có bất thường

---

# 3. Đối tượng sử dụng

## Người cao tuổi

Đặc điểm

- Thị lực kém
- Ít sử dụng smartphone
- Cần giao diện đơn giản
- Chữ lớn
- Màu sắc tương phản cao
- Có thể thao tác chỉ bằng vài nút

Nhu cầu

- Dễ sử dụng
- Nhắc uống thuốc
- Gọi cứu giúp nhanh
- Theo dõi sức khỏe

---

## Người thân

Có thể là:

- Con
- Cháu
- Người chăm sóc

Nhu cầu

- Biết cha mẹ có an toàn không
- Đã uống thuốc chưa
- Có SOS hay không
- Theo dõi vị trí
- Nhận cảnh báo tức thời

---

## Bác sĩ / Điều dưỡng

Nhu cầu

- Xem lịch sử sức khỏe
- Theo dõi chỉ số sinh tồn
- Đưa ra tư vấn
- Phát hiện bất thường

---

## Quản trị viên

Nhiệm vụ

- Quản lý tài khoản
- Quản lý hệ thống
- Quản lý thông báo
- Quản lý dữ liệu
- Thống kê sử dụng

---

# 4. Phạm vi dự án

Ứng dụng Mobile Flutter.

Backend sử dụng Firebase.

Không xây dựng Website.

Không xây dựng hệ thống ERP.

Không quản lý bệnh viện.

Tập trung vào chăm sóc người cao tuổi.

---

# 5. Chức năng chính

## Authentication

- Đăng ký
- Đăng nhập
- OTP
- Quên mật khẩu
- Đăng xuất
- Firebase Authentication

---

## Hồ sơ người cao tuổi

Thông tin gồm:

- Họ tên
- Ngày sinh
- Giới tính
- Nhóm máu
- Chiều cao
- Cân nặng
- Bệnh nền
- Dị ứng thuốc
- Người liên hệ khẩn cấp
- Địa chỉ

---

## Hồ sơ bác sĩ

- Họ tên
- Chuyên khoa
- Bệnh viện
- Số điện thoại

---

## Hồ sơ người chăm sóc

- Quan hệ
- Số điện thoại
- Email

---

# 6. Panic Button (SOS)

Màn hình chính luôn hiển thị nút SOS lớn.

Khi người dùng:

- Bấm SOS
- Hoặc lắc mạnh điện thoại

Hệ thống sẽ:

- Hiển thị đếm ngược 5 giây
- Có thể hủy
- Lấy GPS
- Gửi Firebase Notification
- Gửi SMS
- Gửi vị trí
- Phát còi báo động
- Lưu lịch sử SOS

---

# 7. Shake Detection

Ứng dụng sử dụng cảm biến Accelerometer.

Nếu phát hiện:

- Lắc mạnh

=> tự động kích hoạt SOS.

Có thể cấu hình:

- Độ nhạy
- Số lần lắc

---

# 8. Theo dõi sức khỏe

Cho phép nhập

- Huyết áp
- Đường huyết
- Nhịp tim
- Nhiệt độ
- Cân nặng
- SpO2

Lưu lịch sử.

Hiển thị:

- Biểu đồ ngày
- Tuần
- Tháng
- Năm

Cho phép bác sĩ xem.

---

# 9. Nhắc uống thuốc

Người chăm sóc tạo lịch:

- Tên thuốc
- Liều lượng
- Số lần uống
- Thời gian
- Ghi chú

Đến giờ:

Ứng dụng

- Thông báo
- Đọc bằng giọng nói tiếng Việt

Ví dụ

"Đã đến giờ uống thuốc huyết áp."

Người già nhấn:

Đã uống

Hoặc

Bỏ qua

Nếu quá thời gian

Hệ thống gửi cảnh báo cho người thân.

---

# 10. Dashboard người thân

Hiển thị

- Danh sách người cao tuổi
- Trạng thái Online
- Lần cập nhật gần nhất
- Chỉ số sức khỏe mới nhất
- Đã uống thuốc chưa
- SOS gần đây
- Pin điện thoại
- GPS

---

# 11. Dashboard bác sĩ

Hiển thị

- Danh sách bệnh nhân
- Biểu đồ sức khỏe
- Lịch sử đo
- Chỉ số bất thường
- Ghi chú khám

---

# 12. Notification

Bao gồm

- Firebase Cloud Messaging
- Local Notification
- SOS Notification
- Medicine Reminder
- Health Alert

---

# 13. Bản đồ

Google Maps

Hiển thị

- Vị trí hiện tại
- Lần cập nhật cuối
- Điều hướng

---

# 14. Nhật ký hoạt động

Lưu

- Đăng nhập
- SOS
- Uống thuốc
- Đo sức khỏe
- Cập nhật hồ sơ

---

# 15. Báo cáo

Sinh báo cáo

- PDF
- Excel

Theo

- Tuần
- Tháng
- Năm

---

# 16. Công nghệ sử dụng

Frontend

- Flutter

Backend

- Firebase

Database

- Cloud Firestore

Authentication

- Firebase Authentication

Storage

- Firebase Storage

Notification

- Firebase Cloud Messaging

Maps

- Google Maps

GPS

- Geolocator

Shake Sensor

- sensors_plus

Voice

- flutter_tts

Notification

- flutter_local_notifications

Chart

- fl_chart

State Management

- Riverpod hoặc Provider

Architecture

- Clean Architecture

Dependency Injection

- GetIt

---

# 17. Yêu cầu phi chức năng

Hệ thống phải

- Bảo mật
- Nhanh
- Dễ mở rộng
- Offline được một phần
- Đồng bộ khi có Internet
- Responsive
- Hỗ trợ Android
- Hỗ trợ Dark Mode
- Có Accessibility
- Font lớn
- Voice Support

---

# 18. Bảo mật

- Firebase Rules
- Authentication
- Authorization
- Role Based Access Control
- Mã hóa dữ liệu nhạy cảm
- Nhật ký Audit Log
- Không lưu mật khẩu dạng Plain Text

---

# 19. Kiến trúc

Thiết kế theo Clean Architecture

Bao gồm

Presentation Layer

Business Layer

Domain Layer

Repository Layer

Datasource Layer

Firebase Layer

Dependency Injection

Navigation

State Management

---

# 20. Database

Thiết kế Firestore.

Bao gồm

Collections

Documents

Relationships

Indexes

Validation Rules

Security Rules

Sample Data

---

# 21. API

Thiết kế đầy đủ

REST API

Bao gồm

Method

Endpoint

Authentication

Authorization

Request

Response

HTTP Status

Error Code

Ví dụ JSON

---

# 22. UML

Sinh đầy đủ

- Use Case Diagram
- Activity Diagram
- Sequence Diagram
- Class Diagram
- State Diagram
- Component Diagram
- Deployment Diagram
- Package Diagram

Sử dụng PlantUML.

---

# 23. UI/UX

Thiết kế Wireframe.

Mỗi màn hình gồm

- Mục tiêu
- Thành phần
- Widget Flutter
- Validation
- Điều hướng
- Responsive

Sinh Wireframe dạng ASCII.

---

# 24. User Story

Sinh User Story theo Agile.

Bao gồm

As a

I want

So that

Acceptance Criteria

Priority

Story Point

---

# 25. Functional Requirements

Đặc tả đầy đủ.

Mỗi chức năng gồm

- ID
- Name
- Description
- Actor
- Preconditions
- Main Flow
- Alternative Flow
- Exception Flow
- Business Rule
- Validation Rule
- Postconditions

---

# 26. Non Functional Requirements

Bao gồm

- Performance
- Security
- Availability
- Scalability
- Accessibility
- Reliability
- Maintainability
- Privacy
- Disaster Recovery
- Backup

---

# 27. Test Case

Sinh đầy đủ

- Unit Test
- Widget Test
- Integration Test
- UAT

Mỗi Test Case gồm

- ID
- Feature
- Input
- Expected Output
- Priority
- Status

---

# 28. Future Improvements

Đề xuất các tính năng mở rộng:

- AI phát hiện té ngã
- Đồng bộ Smart Watch
- Theo dõi nhịp tim thời gian thực
- Cảnh báo đột quỵ
- AI dự đoán nguy cơ sức khỏe
- Chat với bác sĩ
- Gọi video khẩn cấp
- Phân tích sức khỏe bằng Machine Learning

---

# Yêu cầu đầu ra

Hãy tạo bộ tài liệu phân tích và thiết kế hoàn chỉnh theo thứ tự:

1. Business Analysis
2. Problem Analysis
3. Stakeholder Analysis
4. Requirement Analysis
5. Functional Requirements
6. Non Functional Requirements
7. Business Rules
8. User Stories
9. Use Cases
10. UML Diagrams
11. Database Design
12. Firestore Design
13. API Design
14. Flutter Architecture
15. UI/UX Design
16. Security Design
17. Deployment Architecture
18. Testing Strategy
19. Future Improvements

Mỗi chương phải trình bày chuyên nghiệp, có bảng biểu, sơ đồ, PlantUML khi phù hợp, giải thích lý do thiết kế, nêu các giả định và các quyết định kiến trúc. Tài liệu phải đủ chi tiết để một nhóm phát triển Flutter có thể bắt đầu triển khai ngay mà không cần làm rõ thêm yêu cầu.