# CHƯƠNG 1: TỔNG QUAN ĐỀ TÀI

---

## 1.1 Đặt vấn đề

### 1.1.1 Bối cảnh xã hội và nhân khẩu học

Việt Nam đang đối mặt với một trong những thách thức nhân khẩu học lớn nhất trong lịch sử: tốc độ già hóa dân số nhanh nhất khu vực Đông Nam Á. Theo Tổng cục Thống kê, tỷ lệ người từ 60 tuổi trở lên hiện chiếm hơn 11% tổng dân số, tương đương khoảng 11 triệu người. Dự báo đến năm 2038, Việt Nam sẽ chính thức bước vào giai đoạn "dân số già" với tỷ lệ người cao tuổi vượt 20%.

Điều đáng lo ngại hơn là cùng với xu hướng đô thị hóa và hạt nhân hóa gia đình, ngày càng nhiều người cao tuổi phải sống một mình hoặc không có người thân chăm sóc thường xuyên. Các nguyên nhân chính bao gồm:

- **Con cái đi làm xa**: Lực lượng lao động trẻ dịch chuyển từ nông thôn lên thành thị, từ thành thị nhỏ lên các đô thị lớn hoặc ra nước ngoài.
- **Gia đình hạt nhân**: Mô hình gia đình truyền thống nhiều thế hệ dần được thay thế bởi gia đình hạt nhân 2–3 thành viên.
- **Tâm lý tự lập**: Nhiều người cao tuổi không muốn làm phiền con cháu, chủ động chọn sống độc lập.
- **Thiếu viện dưỡng lão chất lượng cao**: Văn hóa Việt Nam còn nhiều định kiến với viện dưỡng lão, trong khi các cơ sở hiện có chưa đáp ứng được nhu cầu.

### 1.1.2 Các nguy cơ sức khỏe của người cao tuổi sống độc cư

Người cao tuổi sống một mình đối mặt với hàng loạt nguy cơ sức khỏe nguy hiểm, đặc biệt trong bối cảnh không có người chăm sóc thường trực:

| STT | Nguy cơ | Hậu quả nếu phát hiện muộn | Thời gian vàng |
|---|---|---|---|
| 1 | Té ngã | Gãy xương, xuất huyết não, tàn phế | < 1 giờ |
| 2 | Đột quỵ não | Tử vong hoặc tàn phế vĩnh viễn | < 4,5 giờ |
| 3 | Nhồi máu cơ tim | Tử vong | < 6 giờ |
| 4 | Hạ đường huyết | Hôn mê, tổn thương não | < 30 phút |
| 5 | Quên uống thuốc | Bệnh mãn tính mất kiểm soát | Tích lũy theo ngày |
| 6 | Tăng huyết áp đột ngột | Xuất huyết não | < 2 giờ |

> **Nghiên cứu y tế cho thấy: chỉ cần chậm phát hiện 15–30 phút trong nhiều tình huống khẩn cấp có thể dẫn đến hậu quả không thể phục hồi.**

### 1.1.3 Hạn chế của các giải pháp hiện có

Thị trường hiện có một số giải pháp giám sát người cao tuổi, tuy nhiên đều tồn tại những hạn chế đáng kể:

| Giải pháp | Ưu điểm | Hạn chế |
|---|---|---|
| Camera giám sát tại nhà | Giám sát liên tục, hình ảnh rõ | Vi phạm quyền riêng tư; không có cảnh báo thông minh; chi phí lắp đặt cao |
| Vòng đeo tay y tế thông minh | Đo sinh hiệu tự động | Giá thành cao (2–5 triệu đồng); pin yếu; tính năng hạn chế |
| Điện thoại thông thường | Quen thuộc, dễ dùng | Không tự động; người già khó gọi khi khẩn cấp; không theo dõi sức khỏe |
| Viện dưỡng lão | Chăm sóc toàn diện | Chi phí rất cao; không phù hợp văn hóa; hạn chế tự do cá nhân |
| Ứng dụng sức khỏe phổ thông | Phong phú tính năng | Giao diện phức tạp; không tối ưu cho người cao tuổi; thiếu SOS |

Khoảng trống thị trường rõ ràng: **chưa có giải pháp toàn diện, chi phí thấp, dễ sử dụng, được thiết kế riêng cho người cao tuổi Việt Nam**.

### 1.1.4 Sự cần thiết của đề tài

Xuất phát từ thực tiễn trên, đề tài **"GeriatricCare – Xây dựng hệ thống giám sát và chăm sóc người cao tuổi độc cư"** được đề xuất nhằm giải quyết bài toán cấp bách này thông qua một ứng dụng di động toàn diện, dễ sử dụng và chi phí thấp.

---

## 1.2 Mục tiêu đề tài

### 1.2.1 Mục tiêu tổng quát

Nghiên cứu, phân tích, thiết kế và xây dựng ứng dụng di động **GeriatricCare** trên nền tảng Android, sử dụng framework Flutter và backend Firebase, nhằm hỗ trợ giám sát sức khỏe và đảm bảo an toàn cho người cao tuổi sống độc cư tại Việt Nam.

### 1.2.2 Mục tiêu cụ thể

**Về nghiên cứu lý thuyết:**
- Nghiên cứu quy trình phân tích và thiết kế hệ thống hướng đối tượng (OOA/OOD) theo chuẩn UML 2.5.
- Nghiên cứu kiến trúc phần mềm Clean Architecture và các nguyên tắc SOLID.
- Tìm hiểu framework Flutter, ngôn ngữ Dart và hệ sinh thái Firebase.
- Nghiên cứu các tiêu chuẩn bảo mật OWASP Mobile và thiết kế giao diện Material Design 3.

**Về xây dựng sản phẩm:**
- Xây dựng chức năng SOS khẩn cấp với Panic Button và Shake Detection, đảm bảo gửi cảnh báo trong dưới 3 giây.
- Xây dựng hệ thống theo dõi chỉ số sức khỏe (huyết áp, đường huyết, nhịp tim, SpO2) với cảnh báo tự động.
- Xây dựng tính năng nhắc uống thuốc bằng giọng nói tiếng Việt (TTS).
- Xây dựng dashboard thời gian thực cho người thân và bác sĩ.
- Xây dựng hệ thống theo dõi vị trí GPS và bản đồ Google Maps.
- Thiết kế giao diện tối ưu cho người cao tuổi theo tiêu chuẩn WCAG AA.

**Về kiểm thử và đánh giá:**
- Thực hiện kiểm thử chức năng đầy đủ (Unit Test, Widget Test, Integration Test).
- Đánh giá hiệu năng, độ tin cậy và khả năng sử dụng của ứng dụng.
- Kiểm thử UAT với người dùng thực tế.

---

## 1.3 Đối tượng và phạm vi nghiên cứu

### 1.3.1 Đối tượng nghiên cứu

- **Đối tượng người dùng:** Người cao tuổi (60+), người thân/người chăm sóc, bác sĩ/điều dưỡng, quản trị viên hệ thống.
- **Đối tượng kỹ thuật:** Framework Flutter, Firebase Platform (Firestore, Auth, FCM, Storage), kiến trúc Clean Architecture, mô hình RBAC.

### 1.3.2 Phạm vi nghiên cứu

**Trong phạm vi:**
- Ứng dụng di động Android (Flutter).
- Backend serverless sử dụng Firebase.
- Các chức năng: SOS, theo dõi sức khỏe, nhắc thuốc, GPS, dashboard, báo cáo.
- Hỗ trợ hoạt động offline một phần và đồng bộ tự động.

**Ngoài phạm vi:**
- Ứng dụng iOS (đề xuất phát triển giai đoạn 2).
- Ứng dụng web/admin portal.
- Tích hợp thiết bị đo sức khỏe tự động (đề xuất giai đoạn 2).
- Tính năng AI/ML nâng cao (đề xuất giai đoạn 3).

### 1.3.3 Giới hạn nghiên cứu

- Dữ liệu sức khỏe được nhập thủ công, không tự động thu thập từ thiết bị y tế.
- Hệ thống phụ thuộc vào kết nối Internet cho các tính năng thời gian thực; một số chức năng hoạt động offline.
- Tính năng SOS dựa trên dịch vụ Firebase FCM; hiệu quả phụ thuộc vào cấu hình thiết bị.

---

## 1.4 Phương pháp nghiên cứu

### 1.4.1 Phương pháp thu thập thông tin

| Phương pháp | Mô tả | Đầu ra |
|---|---|---|
| **Nghiên cứu tài liệu** | Đọc tài liệu kỹ thuật Flutter, Firebase, UML, IEEE 830 | Cơ sở lý thuyết |
| **Khảo sát người dùng** | Phỏng vấn người cao tuổi và người thân về nhu cầu thực tế | Yêu cầu chức năng |
| **Phân tích cạnh tranh** | Khảo sát các ứng dụng sức khỏe hiện có | Khoảng trống thị trường |
| **Tham vấn chuyên gia** | Trao đổi với bác sĩ về các chỉ số sinh hiệu quan trọng | Yêu cầu y tế |

### 1.4.2 Phương pháp phân tích và thiết kế

- **Phân tích hướng đối tượng (OOA):** Sử dụng UML 2.5 để mô hình hóa hệ thống (Use Case, Class, Sequence, Activity, State, Component, Deployment Diagram).
- **Thiết kế hướng đối tượng (OOD):** Áp dụng Clean Architecture, SOLID Principles, Design Patterns (Repository, Factory, Observer).
- **Phương pháp Agile:** Chia nhỏ tính năng thành User Stories, ưu tiên theo MoSCoW, phát triển theo Sprint.

### 1.4.3 Phương pháp kiểm thử

- **Unit Testing:** Kiểm thử từng Use Case và Repository riêng lẻ với Mockito.
- **Widget Testing:** Kiểm thử giao diện và tương tác người dùng.
- **Integration Testing:** Kiểm thử luồng đầu cuối với Firebase Emulator.
- **UAT (User Acceptance Testing):** Kiểm thử với người dùng thực theo kịch bản.

---

## 1.5 Ý nghĩa khoa học và thực tiễn

### 1.5.1 Ý nghĩa khoa học

- **Đóng góp về quy trình:** Trình bày quy trình hoàn chỉnh từ phân tích yêu cầu đến triển khai ứng dụng di động theo chuẩn IEEE 830 và UML 2.5, áp dụng cho lĩnh vực chăm sóc sức khỏe người cao tuổi.
- **Đóng góp về kiến trúc:** Minh chứng tính hiệu quả của Clean Architecture kết hợp Riverpod trong phát triển ứng dụng Flutter quy mô vừa.
- **Đóng góp về thiết kế UX:** Đề xuất bộ tiêu chuẩn thiết kế giao diện dành riêng cho người cao tuổi Việt Nam (Senior-first UX Design).

### 1.5.2 Ý nghĩa thực tiễn

- **Về xã hội:** Góp phần nâng cao chất lượng sống và an toàn cho người cao tuổi sống độc cư; giảm gánh nặng lo lắng cho người thân.
- **Về y tế:** Hỗ trợ bác sĩ theo dõi bệnh nhân mãn tính từ xa; nâng cao tỷ lệ tuân thủ điều trị.
- **Về kinh tế:** Cung cấp giải pháp chi phí thấp thay thế thiết bị y tế đắt tiền; áp dụng mô hình kinh doanh Freemium phù hợp thị trường Việt Nam.
- **Về kỹ thuật:** Là nền tảng để phát triển thêm các tính năng AI phát hiện té ngã, dự báo nguy cơ sức khỏe trong tương lai.

---

## 1.6 Cấu trúc báo cáo

Báo cáo đồ án tốt nghiệp được tổ chức thành 6 chương như sau:

**Chương 1 – Tổng quan đề tài:** Trình bày bối cảnh, vấn đề thực tiễn, mục tiêu, phạm vi và phương pháp nghiên cứu của đề tài.

**Chương 2 – Cơ sở lý thuyết và công nghệ:** Trình bày các lý thuyết nền tảng về lập trình hướng đối tượng, kiến trúc phần mềm Clean Architecture, framework Flutter/Dart, Firebase Platform và các công nghệ liên quan.

**Chương 3 – Phân tích hệ thống:** Trình bày kết quả phân tích yêu cầu chức năng và phi chức năng, sơ đồ use case, đặc tả chi tiết các use case quan trọng theo phương pháp hướng đối tượng.

**Chương 4 – Thiết kế hệ thống:** Trình bày thiết kế kiến trúc tổng thể, thiết kế lớp (class diagram), thiết kế cơ sở dữ liệu Firestore, thiết kế giao diện và thiết kế bảo mật.

**Chương 5 – Cài đặt và thử nghiệm:** Trình bày môi trường triển khai, kết quả cài đặt các chức năng chính, kết quả kiểm thử và đánh giá tổng thể hệ thống.

**Chương 6 – Kết luận và hướng phát triển:** Tổng kết kết quả đạt được, những hạn chế còn tồn tại và đề xuất hướng phát triển trong tương lai.

---

*Kết thúc Chương 1*
