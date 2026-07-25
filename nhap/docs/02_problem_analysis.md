# Chương 2: Problem Analysis
## GeriatricCare – Phân tích vấn đề

---

## 2.1 Phát biểu vấn đề (Problem Statement)

> **"Người cao tuổi sống một mình tại Việt Nam đang đối mặt với nguy cơ sức khỏe nghiêm trọng mà không có hệ thống hỗ trợ đủ hiệu quả để phát hiện kịp thời và kết nối với người thân hoặc y tế."**

---

## 2.2 Phân tích gốc rễ vấn đề (Root Cause Analysis)

### 2.2.1 Sơ đồ Fishbone (Ishikawa)

```
                           NGƯỜI CAO TUỔI GẶPNGUY HIỂM
                                      |
         ┌────────────┬───────────────┼───────────────┬────────────┐
         │            │               │               │            │
      Con Người    Quy Trình      Công Nghệ       Môi Trường   Hệ Thống
         │            │               │               │            │
  • Người thân     • Không có      • Điện thoại    • Sống        • Không có
    đi làm xa        quy trình       khó dùng        một mình      hệ thống
  • Không có       • Không ai      • Ứng dụng      • Nhà tầng    • Bác sĩ
    người           nhắc thuốc      phức tạp        nhiều         không
    chăm sóc      • Không ai      • Không có      • Cửa khóa    theo dõi
  • Không có        theo dõi        cảnh báo      • Thiếu ánh     từ xa
    kiến thức       định kỳ         tự động         sáng
    y tế           SOS
```

### 2.2.2 Phân tích 5 Why

**Vấn đề:** Người cao tuổi té ngã nhưng không được phát hiện kịp thời

| Why | Nguyên nhân |
|---|---|
| Why 1 | Không có ai ở nhà để phát hiện |
| Why 2 | Người thân đi làm, không ở cùng |
| Why 3 | Không có hệ thống giám sát tự động |
| Why 4 | Các giải pháp hiện tại quá đắt hoặc phức tạp |
| Why 5 | Chưa có sản phẩm tối ưu cho người cao tuổi Việt Nam |

---

## 2.3 Phân tích các vấn đề cụ thể

### Vấn đề 1: Không có cơ chế SOS hiệu quả

**Mô tả:** Khi xảy ra tai biến, người cao tuổi không thể gọi điện nhanh hoặc mở khóa điện thoại kịp thời.

**Tác động:**
- Chậm trễ cấp cứu → nguy hiểm tính mạng
- Người thân không biết → lo lắng, bất an

**Giải pháp đề xuất:**
- Nút SOS lớn luôn hiển thị trên màn hình chính
- Shake Detection: lắc điện thoại để gửi SOS
- Đếm ngược 5 giây có thể hủy (tránh báo nhầm)
- Tự động gửi GPS + thông báo đến tất cả người thân

---

### Vấn đề 2: Quên uống thuốc

**Mô tả:** Người cao tuổi thường quên lịch uống thuốc do bệnh mãn tính cần uống nhiều loại thuốc mỗi ngày.

**Tác động:**
- Bệnh không được kiểm soát
- Nguy cơ biến chứng

**Giải pháp đề xuất:**
- Lịch nhắc thuốc chi tiết
- Thông báo push + giọng nói tiếng Việt
- Xác nhận đã uống / bỏ qua
- Cảnh báo người thân nếu không phản hồi

---

### Vấn đề 3: Người thân không biết tình trạng sức khỏe thực tế

**Mô tả:** Người thân lo lắng nhưng không có thông tin cụ thể về sức khỏe hàng ngày của cha mẹ.

**Tác động:**
- Không can thiệp kịp thời
- Quan hệ gia đình căng thẳng vì lo lắng

**Giải pháp đề xuất:**
- Dashboard tổng quan cho người thân
- Cập nhật chỉ số sức khỏe theo thời gian thực
- Thông báo tức thời khi có bất thường

---

### Vấn đề 4: Bác sĩ thiếu dữ liệu liên tục

**Mô tả:** Bác sĩ chỉ có dữ liệu tại thời điểm khám, thiếu thông tin theo dõi hàng ngày.

**Tác động:**
- Chẩn đoán không chính xác
- Điều chỉnh thuốc không kịp thời

**Giải pháp đề xuất:**
- Dashboard bác sĩ với biểu đồ sức khỏe liên tục
- Lịch sử đo chỉ số theo ngày/tuần/tháng
- Ghi chú khám của bác sĩ

---

### Vấn đề 5: Giao diện ứng dụng không phù hợp người cao tuổi

**Mô tả:** Các ứng dụng hiện tại thiết kế cho người trẻ – font nhỏ, nhiều tính năng phức tạp.

**Tác động:**
- Người cao tuổi không dùng được
- Từ bỏ sử dụng ứng dụng

**Giải pháp đề xuất:**
- Font size tối thiểu 18sp
- Màu sắc tương phản cao
- Giao diện tối giản
- Nút to, dễ bấm (minimum 48dp)
- Giọng nói hỗ trợ

---

## 2.4 Ma trận vấn đề – Giải pháp

| ID | Vấn đề | Mức độ nghiêm trọng | Giải pháp | Module |
|---|---|---|---|---|
| P-01 | Không có SOS khẩn cấp | 🔴 Rất cao | Panic Button + Shake Detection | SOS Module |
| P-02 | Quên uống thuốc | 🔴 Cao | Medicine Reminder + TTS | Medication Module |
| P-03 | Người thân thiếu thông tin | 🟡 Cao | Family Dashboard | Dashboard Module |
| P-04 | Bác sĩ thiếu dữ liệu | 🟡 Trung bình | Doctor Dashboard | Health Module |
| P-05 | Giao diện khó dùng | 🔴 Cao | Senior-first UX Design | UI/UX |
| P-06 | Không biết vị trí | 🟡 Cao | GPS Tracking + Google Maps | Location Module |
| P-07 | Thiếu lịch sử sức khỏe | 🟡 Trung bình | Health History + Charts | Health Module |
| P-08 | Mất dữ liệu khi mất mạng | 🟠 Trung bình | Offline Mode + Sync | Data Module |

---

## 2.5 Impact Analysis

```
┌─────────────────────────────────────────────────────────────────┐
│                     IMPACT MATRIX                               │
│                                                                 │
│  Tác động cao ──────────────────────────────── Tác động thấp   │
│                                                                 │
│  [SOS Khẩn cấp]  [Quên thuốc]  [GPS]  [Dashboard]  [Báo cáo]  │
│       ↓               ↓          ↓          ↓            ↓     │
│  Nguy hiểm       Bệnh trở    Không tìm  Thiếu      Khó đánh   │
│  tính mạng       nặng        được       thông tin  giá        │
└─────────────────────────────────────────────────────────────────┘
```

---

## 2.6 Phạm vi giải quyết

GeriatricCare tập trung giải quyết **5 vấn đề cốt lõi**:

1. **An toàn khẩn cấp** – SOS + Shake Detection + GPS
2. **Tuân thủ điều trị** – Nhắc thuốc + xác nhận + cảnh báo
3. **Kết nối gia đình** – Dashboard + push notification
4. **Theo dõi sức khỏe** – Nhập chỉ số + biểu đồ + lịch sử
5. **Trải nghiệm người dùng cao tuổi** – UX đặc biệt dành cho senior
