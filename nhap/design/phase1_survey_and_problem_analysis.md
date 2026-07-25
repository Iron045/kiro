# GIAI ĐOẠN 1: KHẢO SÁT & PHÂN TÍCH BÀI TOÁN
## GeriatricCare – Hệ thống Giám sát & Chăm sóc Người cao tuổi Độc cư

**Phiên bản:** 1.0.0 | **Ngày:** 2026-07-25 | **Trạng thái:** Hoàn chỉnh

---

## 1. MỤC TIÊU GIAI ĐOẠN

Giai đoạn này xác định:
- Bối cảnh thực tế và nhu cầu thị trường
- Các vấn đề cốt lõi cần giải quyết
- Đối tượng sử dụng và hành vi của họ
- Phạm vi, giả định và ràng buộc dự án
- Rủi ro và chiến lược giảm thiểu

---

## 2. KHẢO SÁT BỐI CẢNH

### 2.1 Tình hình nhân khẩu học Việt Nam

| Chỉ số | Số liệu |
|--------|---------|
| Dân số từ 60 tuổi trở lên (2026) | ~12 triệu người (~12% dân số) |
| Tốc độ già hóa | Nhanh nhất Đông Nam Á |
| Dự báo "dân số già" | Trước 2038 (tỷ lệ > 20%) |
| Người cao tuổi sống một mình | ~30% trong tổng số NCT |
| Tỷ lệ có smartphone Android | ~55% trong độ tuổi 60–70 |

### 2.2 Khảo sát hành vi người dùng

**Người cao tuổi (60–80 tuổi):**
- Thị lực giảm – khó đọc chữ nhỏ hơn 16sp
- Phản xạ chậm – cần nút bấm tối thiểu 48×48dp
- Trí nhớ giảm – hay quên lịch uống thuốc
- Ít quen công nghệ – cần giao diện tối giản
- Lo lắng về sức khỏe và an toàn bản thân

**Người thân (con cái, 30–50 tuổi):**
- Đi làm xa, bận rộn
- Lo lắng nhưng không có thông tin cụ thể
- Muốn nhận cảnh báo tức thời qua điện thoại
- Cần dashboard đơn giản, dễ đọc nhanh

**Bác sĩ / Điều dưỡng:**
- Chỉ có dữ liệu tại thời điểm khám, thiếu dữ liệu liên tục
- Cần biểu đồ xu hướng sức khỏe theo thời gian
- Muốn ghi chú tư vấn, theo dõi từ xa

### 2.3 Phân tích giải pháp hiện tại

| Giải pháp | Ưu điểm | Nhược điểm | Chi phí |
|-----------|---------|------------|---------|
| Camera giám sát | Quan sát trực tiếp | Vi phạm riêng tư, không cảnh báo thông minh | Trung bình |
| Điện thoại thông thường | Sẵn có | Phụ thuộc người dùng tự gọi | Thấp |
| Vòng đeo tay y tế | Tự động phát hiện té ngã | Đắt tiền, pin kém, ít tính năng | Cao |
| Viện dưỡng lão | Chăm sóc chuyên nghiệp | Không phù hợp văn hóa Việt Nam, chi phí cao | Rất cao |
| Ứng dụng GPS | Theo dõi vị trí | Không có SOS, không theo dõi sức khỏe | Thấp |

**Kết luận:** Chưa có giải pháp nào tích hợp đầy đủ SOS + nhắc thuốc + theo dõi sức khỏe + dashboard gia đình với UX phù hợp người cao tuổi Việt Nam.

---

## 3. PHÂN TÍCH BÀI TOÁN

### 3.1 Phát biểu bài toán chính

> **"Người cao tuổi sống một mình tại Việt Nam thiếu một hệ thống hỗ trợ tích hợp, dễ sử dụng, có khả năng phát hiện và cảnh báo khẩn cấp kịp thời, đồng thời kết nối với người thân và bác sĩ theo dõi sức khỏe từ xa."**

### 3.2 Phân tích gốc rễ – Sơ đồ Fishbone

```
                    ┌─────────────────────────────────┐
                    │  NGƯỜI CAO TUỔI GẶP NGUY HIỂM  │
                    └────────────────┬────────────────┘
          ┌──────────────┬───────────┴──────────┬──────────────┐
          ▼              ▼                      ▼              ▼
       Con Người      Quy Trình            Công Nghệ       Môi Trường
    ─────────────  ─────────────         ─────────────   ─────────────
    • Người thân   • Không nhắc          • App phức tạp  • Sống 1 mình
      đi làm xa     uống thuốc           • Font quá nhỏ  • Nhà nhiều tầng
    • Không có     • Không ai             • Không có      • Thiếu ánh sáng
      người          giám sát              SOS tích hợp  • Cửa khóa trong
      chăm sóc    • Không có quy         • Thiếu giọng
    • Hạn chế       trình SOS              nói hỗ trợ
      kiến thức
      y tế
```

### 3.3 Phân tích 5 Why cho từng vấn đề

**Bài toán A: Không phát hiện kịp thời khi tai biến**

| Bước | Tại sao? |
|------|----------|
| Why 1 | Không có ai ở nhà phát hiện |
| Why 2 | Người thân đi làm, không ở cùng |
| Why 3 | Không có thiết bị/hệ thống giám sát tự động |
| Why 4 | Các giải pháp hiện tại quá đắt hoặc phức tạp |
| Why 5 | Chưa có sản phẩm tối ưu cho NCT Việt Nam với chi phí hợp lý |
| **Root Cause** | **Thiếu hệ thống SOS tích hợp, dễ kích hoạt, chi phí thấp** |

**Bài toán B: Không tuân thủ lịch uống thuốc**

| Bước | Tại sao? |
|------|----------|
| Why 1 | Người cao tuổi quên uống thuốc |
| Why 2 | Không có ai nhắc nhở tại nhà |
| Why 3 | Lịch thuốc phức tạp (nhiều loại, nhiều giờ) |
| Why 4 | Ứng dụng nhắc thuốc không phù hợp người cao tuổi |
| Why 5 | Thiếu cơ chế xác nhận và cảnh báo người thân |
| **Root Cause** | **Thiếu hệ thống nhắc thuốc có giọng nói + xác nhận + cảnh báo leo thang** |

**Bài toán C: Người thân thiếu thông tin**

| Bước | Tại sao? |
|------|----------|
| Why 1 | Không biết tình trạng sức khỏe cha mẹ hàng ngày |
| Why 2 | Không có kênh thông tin tự động |
| Why 3 | Chỉ biết khi gọi điện hỏi trực tiếp |
| Why 4 | NCT không chủ động báo cáo sức khỏe |
| Why 5 | Thiếu dashboard tổng hợp thông tin tự động |
| **Root Cause** | **Thiếu hệ thống chia sẻ thông tin sức khỏe thời gian thực** |

---

## 4. CÁC VẤN ĐỀ CỐT LÕI (PROBLEM INVENTORY)

| ID | Vấn đề | Mức độ | Tần suất | Hậu quả nếu không giải quyết |
|----|--------|--------|----------|-------------------------------|
| P-01 | Không có cơ chế SOS khẩn cấp hiệu quả | 🔴 Critical | Bất cứ lúc nào | Tử vong, tàn phế |
| P-02 | Quên uống thuốc mãn tính | 🔴 High | Hàng ngày | Biến chứng bệnh, nhập viện |
| P-03 | Người thân không biết tình trạng NCT | 🟠 High | Hàng ngày | Lo lắng, không can thiệp kịp |
| P-04 | Bác sĩ thiếu dữ liệu liên tục | 🟡 Medium | Hàng tuần | Chẩn đoán sai, thuốc không phù hợp |
| P-05 | Giao diện app không phù hợp NCT | 🔴 High | Liên tục | NCT không dùng được, bỏ ứng dụng |
| P-06 | Không biết vị trí NCT khi khẩn cấp | 🟠 High | Khi SOS | Cấp cứu không đến đúng nơi |
| P-07 | Thiếu lịch sử sức khỏe để theo dõi | 🟡 Medium | Hàng tuần | Không đánh giá được xu hướng |
| P-08 | Mất dữ liệu khi mất kết nối mạng | 🟡 Medium | Không thường xuyên | Mất thông tin quan trọng |

---

## 5. ĐỐI TƯỢNG SỬ DỤNG (USER PERSONAS)

### Persona 1 – Cụ Nguyễn Thị Mai (Người cao tuổi)

```
┌─────────────────────────────────────────────────────────────┐
│  Cụ Nguyễn Thị Mai – 72 tuổi – Hà Nội                     │
│  Sống một mình, con cái đi làm TP.HCM                      │
├─────────────────────────────────────────────────────────────┤
│  Bệnh nền: Tăng huyết áp, tiểu đường type 2               │
│  Thuốc: 4 loại, uống 2–3 lần/ngày                          │
│  Smartphone: Android cũ, màn hình 5.5"                      │
│  Kỹ năng số: Chỉ dùng Zalo, chụp ảnh                       │
├─────────────────────────────────────────────────────────────┤
│  MỤC TIÊU           │  NỖI SỢ HÃI                         │
│  • Không làm phiền  │  • Ngã trong nhà không ai biết       │
│    con cái          │  • Bị đột quỵ một mình               │
│  • Được an toàn     │  • Quên thuốc gây biến chứng         │
│  • Tự chủ trong     │  • Con cái lo lắng quá mức           │
│    sinh hoạt        │                                       │
├─────────────────────────────────────────────────────────────┤
│  HÀNH VI SỬ DỤNG APP                                       │
│  • Cần nút bấm ≥ 48dp, chữ ≥ 20sp                         │
│  • Cần giọng nói nhắc thuốc (không đọc được chữ nhỏ)      │
│  • Cần nút SOS luôn hiện, dễ bấm nhất                      │
└─────────────────────────────────────────────────────────────┘
```

### Persona 2 – Anh Trần Văn Hùng (Con trai, Người thân)

```
┌─────────────────────────────────────────────────────────────┐
│  Anh Trần Văn Hùng – 45 tuổi – TP.HCM                     │
│  Kỹ sư, bận rộn, cha mẹ ở Hà Nội                          │
├─────────────────────────────────────────────────────────────┤
│  Smartphone: iPhone 15, dùng thành thạo                     │
│  Thói quen: Kiểm tra điện thoại mỗi 30 phút khi rảnh      │
├─────────────────────────────────────────────────────────────┤
│  MỤC TIÊU                   │  NỖI SỢ HÃI                  │
│  • Biết mẹ an toàn          │  • Không biết khi mẹ ngã     │
│  • Nhận ngay khi có SOS     │  • Mẹ quên thuốc             │
│  • Dashboard nhanh, rõ ràng │  • Không liên lạc được khi   │
│  • Theo dõi sức khỏe từ xa  │    cần gấp                   │
├─────────────────────────────────────────────────────────────┤
│  HÀNH VI SỬ DỤNG APP                                       │
│  • Xem dashboard 5–10 giây, cần thông tin rõ ngay          │
│  • Muốn push notification quan trọng (không bị bỏ qua)     │
│  • Theo dõi biểu đồ sức khỏe tuần/tháng                   │
└─────────────────────────────────────────────────────────────┘
```

### Persona 3 – BS. Lê Minh Tuấn (Bác sĩ)

```
┌─────────────────────────────────────────────────────────────┐
│  BS. Lê Minh Tuấn – 38 tuổi – BV Bạch Mai                 │
│  Nội khoa, theo dõi 15–20 bệnh nhân NCT                    │
├─────────────────────────────────────────────────────────────┤
│  MỤC TIÊU                     │  KHÓ KHĂN HIỆN TẠI        │
│  • Có dữ liệu liên tục        │  • Chỉ có kết quả khi khám │
│  • Phát hiện bất thường sớm   │  • BN không báo kịp thời   │
│  • Tư vấn từ xa hiệu quả      │  • Không biết có uống đúng │
│  • Lịch sử đầy đủ để khám     │    thuốc không             │
└─────────────────────────────────────────────────────────────┘
```

---

## 6. PHẠM VI DỰ ÁN

### 6.1 Trong phạm vi (In Scope)

| # | Tính năng | Ưu tiên |
|---|-----------|---------|
| 1 | Xác thực (Đăng ký/Đăng nhập/OTP) | Must Have |
| 2 | Quản lý hồ sơ NCT, bác sĩ, người thân | Must Have |
| 3 | Panic Button (SOS) với GPS + FCM | Must Have |
| 4 | Shake Detection tự động kích hoạt SOS | Must Have |
| 5 | Nhắc uống thuốc + TTS tiếng Việt | Must Have |
| 6 | Theo dõi chỉ số sức khỏe (6 loại) | Must Have |
| 7 | Dashboard người thân (real-time) | Must Have |
| 8 | Dashboard bác sĩ + biểu đồ | Should Have |
| 9 | Bản đồ Google Maps + GPS tracking | Should Have |
| 10 | Báo cáo PDF/Excel | Could Have |
| 11 | Dark Mode + Accessibility | Must Have |
| 12 | Nhật ký hoạt động | Should Have |

### 6.2 Ngoài phạm vi (Out of Scope)

| # | Loại trừ | Lý do |
|---|----------|-------|
| 1 | Ứng dụng Web | Giai đoạn 1 tập trung mobile |
| 2 | iOS | Ưu tiên Android trước |
| 3 | AI Fall Detection tự động | Giai đoạn 2 |
| 4 | Smart Watch integration | Giai đoạn 2 |
| 5 | Video call | Giai đoạn 2 |
| 6 | Quản lý bệnh viện / ERP | Ngoài phạm vi sản phẩm |
| 7 | Kết nối thiết bị đo tự động (Bluetooth) | Giai đoạn 2 |

---

## 7. GIẢ ĐỊNH & RÀNG BUỘC

### 7.1 Giả định

| ID | Giả định |
|----|----------|
| A-01 | NCT sử dụng Android smartphone (≥ Android 10) |
| A-02 | Có kết nối Internet để sử dụng tính năng real-time |
| A-03 | Người thân cài đặt và cấu hình ban đầu cho NCT |
| A-04 | Firebase đáp ứng SLA 99.9% uptime |
| A-05 | Google Maps API hoạt động tại Việt Nam |
| A-06 | NCT đồng ý cấp quyền: vị trí, microphone, thông báo, gia tốc kế |
| A-07 | Dữ liệu sức khỏe nhập thủ công (chưa có thiết bị đo tự động) |
| A-08 | Người thân cài đặt ứng dụng trên Android hoặc iOS |

### 7.2 Ràng buộc

| ID | Ràng buộc | Loại |
|----|----------|------|
| C-01 | Dùng Firebase Backend (không tự xây REST backend) | Kiến trúc |
| C-02 | Font size ≥ 18sp, nút ≥ 48×48dp | UX |
| C-03 | Thời gian gửi SOS ≤ 3 giây | Hiệu năng |
| C-04 | Hoạt động offline một phần (Hive cache) | Kỹ thuật |
| C-05 | Dữ liệu y tế tuân thủ bảo mật, mã hóa | Bảo mật |
| C-06 | Đếm ngược 5 giây trước khi gửi SOS (tránh nhầm) | Nghiệp vụ |

---

## 8. PHÂN TÍCH RỦI RO

| ID | Rủi ro | Xác suất | Tác động | Chiến lược |
|----|--------|----------|----------|------------|
| R-01 | NCT không chịu dùng app | Cao | Cao | UX siêu đơn giản, TTS hỗ trợ, người thân setup |
| R-02 | GPS không chính xác trong nhà | Trung bình | Trung bình | Kết hợp WiFi, hiển thị cảnh báo độ chính xác |
| R-03 | Pin cạn → không gửi SOS | Cao | Rất cao | Cảnh báo khi pin < 20%, khuyến khích cắm sạc |
| R-04 | Notification bị tắt bởi Android | Trung bình | Cao | Foreground service, hướng dẫn cấu hình |
| R-05 | Dữ liệu y tế rò rỉ | Thấp | Rất cao | Firebase Security Rules, mã hóa Hive |
| R-06 | Chi phí Firebase vượt ngân sách | Trung bình | Trung bình | Tối ưu reads/writes, Firestore pagination |
| R-07 | Shake Detection báo nhầm | Cao | Trung bình | Đếm ngược hủy 5 giây, điều chỉnh độ nhạy |
| R-08 | Mất kết nối Internet khi SOS | Trung bình | Rất cao | SMS fallback, queue offline |

---

## 9. KẾT LUẬN GIAI ĐOẠN 1

**Vấn đề được xác nhận:** 8 vấn đề cốt lõi, trong đó 3 vấn đề Critical.

**Giải pháp hướng đến:**
- Mobile App Flutter + Firebase Backend
- UX tối ưu cho NCT (Senior-first Design)
- SOS tích hợp Shake Detection + GPS + FCM
- Nhắc thuốc với giọng nói tiếng Việt
- Dashboard real-time cho người thân và bác sĩ

**Đầu ra chuyển sang Giai đoạn 2:** Danh sách vấn đề, persona, phạm vi xác nhận → làm đầu vào phân tích yêu cầu.
