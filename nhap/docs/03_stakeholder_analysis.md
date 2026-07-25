# Chương 3: Stakeholder Analysis
## GeriatricCare – Phân tích các bên liên quan

---

## 3.1 Tổng quan các bên liên quan

```
┌─────────────────────────────────────────────────────────────────┐
│                    STAKEHOLDER MAP                              │
│                                                                 │
│                    [GeriatricCare App]                          │
│                           │                                     │
│         ┌─────────────────┼─────────────────┐                  │
│         │                 │                 │                   │
│   [Người dùng]    [Nhà vận hành]     [Đối tác bên ngoài]       │
│         │                 │                 │                   │
│   • Người cao tuổi  • Admin        • Firebase/Google            │
│   • Người thân      • Dev Team     • Google Maps                │
│   • Bác sĩ          • Product Owner                             │
│   • Điều dưỡng                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 3.2 Ma trận Stakeholder

| Stakeholder | Vai trò | Mức quan tâm | Mức ảnh hưởng | Chiến lược tiếp cận |
|---|---|---|---|---|
| Người cao tuổi | Primary User | Rất cao | Cao | Thiết kế UX ưu tiên |
| Người thân (con/cháu) | Secondary User | Rất cao | Cao | Feature-rich dashboard |
| Bác sĩ / Điều dưỡng | Professional User | Cao | Trung bình | Clinical dashboard |
| Quản trị viên | System Admin | Trung bình | Cao | Admin panel |
| Product Owner | Sponsor | Rất cao | Rất cao | Báo cáo tiến độ tuần |
| Flutter Dev Team | Implementer | Cao | Cao | Technical docs đầy đủ |
| Firebase / Google | Infra Provider | Thấp | Cao | SLA monitoring |

---

## 3.3 Phân tích chi tiết từng Stakeholder

### 3.3.1 Người cao tuổi (Elderly User)

**Đặc điểm nhân khẩu học:**
- Tuổi: 60–85+
- Trình độ công nghệ: Thấp đến trung bình
- Thị lực: Kém (cần font lớn)
- Thính giác: Có thể suy giảm
- Vận động tinh: Kém (ngón tay run)

**Pain Points:**
| ID | Pain Point | Mức độ |
|---|---|---|
| EP-01 | Không gọi được cứu trợ khi khẩn cấp | 🔴 Rất cao |
| EP-02 | Quên uống thuốc | 🔴 Cao |
| EP-03 | Sợ làm phiền con cái | 🟡 Trung bình |
| EP-04 | Giao diện ứng dụng quá phức tạp | 🔴 Cao |
| EP-05 | Khó nhìn chữ nhỏ | 🔴 Cao |

**Needs:**
- Nút SOS to, dễ bấm, luôn hiện
- Nhắc uống thuốc bằng giọng nói tiếng Việt
- Giao diện cực đơn giản, tối đa 3–4 nút chính
- Font chữ lớn ≥ 18sp
- Màu sắc tương phản cao

**Goals:**
- Sống độc lập nhưng an toàn
- Không làm gánh nặng cho con cái
- Được chăm sóc sức khỏe đúng cách

---

### 3.3.2 Người thân (Family Caregiver)

**Đặc điểm:**
- Tuổi: 25–55
- Thường xuyên đi làm xa hoặc bận rộn
- Sử dụng smartphone thành thạo
- Lo lắng thường xuyên về cha/mẹ

**Pain Points:**
| ID | Pain Point | Mức độ |
|---|---|---|
| FP-01 | Không biết cha/mẹ có an toàn không | 🔴 Rất cao |
| FP-02 | Không biết cha/mẹ đã uống thuốc chưa | 🔴 Cao |
| FP-03 | Không biết vị trí khi có sự cố | 🔴 Cao |
| FP-04 | Phải gọi điện nhiều lần mới liên lạc được | 🟡 Trung bình |
| FP-05 | Không có báo cáo sức khỏe định kỳ | 🟡 Trung bình |

**Needs:**
- Nhận thông báo SOS ngay lập tức
- Xem vị trí GPS realtime
- Biết tình trạng uống thuốc
- Dashboard tổng quan sức khỏe
- Nhận cảnh báo chỉ số bất thường

**Goals:**
- Yên tâm khi đi làm
- Can thiệp kịp thời khi có sự cố
- Theo dõi sức khỏe cha/mẹ từ xa

---

### 3.3.3 Bác sĩ / Điều dưỡng (Healthcare Professional)

**Đặc điểm:**
- Chuyên môn y tế
- Quản lý nhiều bệnh nhân cùng lúc
- Cần thông tin chính xác, đầy đủ

**Pain Points:**
| ID | Pain Point | Mức độ |
|---|---|---|
| DP-01 | Thiếu dữ liệu theo dõi liên tục | 🔴 Cao |
| DP-02 | Không biết bệnh nhân có tuân thủ điều trị | 🟡 Trung bình |
| DP-03 | Khó liên lạc với bệnh nhân cao tuổi | 🟡 Trung bình |
| DP-04 | Không có công cụ theo dõi từ xa | 🔴 Cao |

**Needs:**
- Biểu đồ sức khỏe theo thời gian
- Lịch sử đo chỉ số sinh tồn
- Cảnh báo khi chỉ số vượt ngưỡng
- Ghi chú khám bệnh
- Danh sách bệnh nhân đang theo dõi

**Goals:**
- Chăm sóc bệnh nhân hiệu quả hơn
- Ra quyết định điều trị chính xác hơn
- Phát hiện biến chứng sớm

---

### 3.3.4 Quản trị viên (System Administrator)

**Đặc điểm:**
- Quản lý kỹ thuật hệ thống
- Cần công cụ quản lý toàn diện

**Needs:**
- Quản lý tài khoản người dùng
- Theo dõi log hệ thống
- Thống kê sử dụng
- Quản lý thông báo hàng loạt
- Phân quyền người dùng

---

## 3.4 Bản đồ hành trình người dùng (User Journey Map)

### Journey: Người cao tuổi – Khi xảy ra sự cố

```
Giai đoạn   │ Nhận biết  │  Hành động  │  Chờ đợi   │  Kết quả
            │            │             │            │
Trải nghiệm │ Cảm thấy   │ Bấm SOS     │ Đếm ngược  │ Người thân
            │ không ổn   │ hoặc lắc    │ 5 giây     │ nhận thông báo
            │            │ điện thoại  │            │ và đến hỗ trợ
            │            │             │            │
Cảm xúc     │ Lo lắng    │ Căng thẳng  │ Hồi hộp   │ Nhẹ nhõm
            │ Sợ hãi     │             │            │ An tâm
            │            │             │            │
Điểm đau    │ Không thể  │ Tay run,    │ Không biết │
            │ gọi điện   │ khó bấm     │ ai sẽ đến  │
            │ được       │ điện thoại  │            │
            │            │             │            │
Cơ hội      │ Nút SOS    │ Nút lớn +   │ Hiển thị   │ GPS tự động
            │ luôn hiện  │ shake detect│ trạng thái │ chia sẻ vị trí
```

### Journey: Người thân – Theo dõi hàng ngày

```
Giai đoạn   │ Sáng        │  Trong ngày  │  Tối
            │             │              │
Hành động   │ Mở app kiểm │ Nhận thông   │ Xem báo cáo
            │ tra dashboard│ báo nhắc thuốc│ sức khỏe ngày
            │             │              │
Cảm xúc     │ Lo lắng     │ Yên tâm khi  │ Hài lòng khi
            │ trước khi   │ cha/mẹ xác   │ mọi thứ bình
            │ mở app      │ nhận uống thuốc│ thường
```

---

## 3.5 RACI Matrix

| Tính năng | Người cao tuổi | Người thân | Bác sĩ | Admin |
|---|---|---|---|---|
| Đăng ký tài khoản | R | A | I | C |
| Tạo hồ sơ sức khỏe | R | A/C | C | I |
| Nhập chỉ số sức khỏe | R/A | C | I | I |
| Tạo lịch uống thuốc | I | R/A | C | I |
| Kích hoạt SOS | R/A | I | I | I |
| Xem dashboard người thân | I | R/A | I | I |
| Xem dashboard bác sĩ | I | I | R/A | I |
| Quản lý người dùng | I | I | I | R/A |
| Cấu hình hệ thống | I | I | I | R/A |

> R = Responsible, A = Accountable, C = Consulted, I = Informed

---

## 3.6 Chiến lược tiếp cận Stakeholder

| Stakeholder | Kênh giao tiếp | Tần suất | Nội dung |
|---|---|---|---|
| Product Owner | Meeting + Báo cáo | Hàng tuần | Tiến độ, rủi ro, quyết định |
| Dev Team | Daily standup + Jira | Hàng ngày | Task, blocker, review |
| Người cao tuổi | User Testing | Sprint review | UX feedback |
| Người thân | Beta Testing | Sprint review | Feature feedback |
| Bác sĩ | Focus Group | Tháng | Clinical requirements |
