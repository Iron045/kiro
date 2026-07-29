# Phân chia công việc nhóm 3 người
## Dự án: GeriatricCare – Giám sát & Chăm sóc Người cao tuổi Độc cư

> **Môn học:** CSE441 - Phát triển ứng dụng trên thiết bị di động (Flutter)

---

# I. Nguyên tắc phân chia

Do đây là học phần **Phát triển ứng dụng trên thiết bị di động**, mục tiêu không chỉ là hoàn thành sản phẩm mà còn giúp **mỗi thành viên đều được trải nghiệm đầy đủ quy trình phát triển phần mềm**.

Vì vậy, nhóm **không phân chia theo Frontend - Backend - UI** mà phân chia theo **module chức năng (Feature-based Development)**.

Mỗi thành viên đều phải thực hiện:

- Phân tích yêu cầu
- Thiết kế giao diện
- Thiết kế cơ sở dữ liệu liên quan
- Lập trình Flutter
- Tích hợp Firebase
- Kiểm thử
- Viết tài liệu
- Thuyết trình

---

# II. Phân chia Module

```
                 GeriatricCare

        ┌────────────┬────────────┬────────────┐
        │            │            │
        ▼            ▼            ▼
 Người cao tuổi    Người thân     Bác sĩ
 (Elder App)      (Child App)   (Doctor Dashboard)
```

---

# III. Thành viên 1
# Module Người cao tuổi (Elder Mobile)

## 1. Phân tích

### Phụ trách User Story

- User Story 1
  - SOS khẩn cấp

- User Story 2
  - Nhắc uống thuốc

- User Story 3
  - Ghi nhận chỉ số sức khỏe

---

## 2. Thiết kế

- Use Case
- Activity Diagram
- Sequence Diagram
- UI Flow
- Thiết kế giao diện Figma

---

## 3. Lập trình Flutter

### Màn hình

- Login
- Home
- SOS
- Health Record
- Reminder
- Hồ sơ cá nhân

---

### Chức năng

- SOS Button

- Shake Detection

- GPS

- Flutter TTS

- Local Notification

- Nhập huyết áp

- Nhập đường huyết

- Accessibility

---

## 4. Firebase

- Đọc lịch thuốc

- Gửi SOS

- Lưu Health Record

- Đồng bộ dữ liệu

---

## 5. Kiểm thử

- Test SOS

- Test Reminder

- Test GPS

- Test nhập dữ liệu

---

## 6. Báo cáo

- Mô tả module

- Hình giao diện

- Kết quả thực hiện

---

# IV. Thành viên 2
# Module Người thân (Child Mobile)

## 1. Phân tích

### Phụ trách User Story

- User Story 4

- User Story 5

- User Story 6

---

## 2. Thiết kế

- Use Case

- Activity Diagram

- Sequence Diagram

- UI Flow

- Thiết kế giao diện

---

## 3. Lập trình Flutter

### Màn hình

- Dashboard

- Alert Feed

- Health Detail

- Charts

- Pill Schedule

- Settings

---

### Chức năng

- Nhận Notification

- Xem sức khỏe

- CRUD lịch thuốc

- Theo dõi chỉ số

- Xác nhận SOS

---

## 4. Firebase

- Firestore

- FCM

- CRUD Pill Schedule

- Đọc Health Record

---

## 5. Kiểm thử

- Notification

- CRUD thuốc

- Dashboard

- Charts

---

## 6. Báo cáo

- Chức năng

- Giao diện

- Kết quả

---

# V. Thành viên 3
# Module Bác sĩ (Doctor Dashboard)

## 1. Phân tích

### Phụ trách

- User Story 7

---

## 2. Thiết kế

- Dashboard

- UI Flow

- ERD

- Thiết kế giao diện

---

## 3. Lập trình Flutter

### Dashboard

- Login

- Patient List

- Patient Detail

- Health Charts

- SOS History

- Doctor Note

- Alert Feed

---

## 4. Firebase

- Firestore

- Authentication

- Cloud Functions

- Security Rules

- Dashboard Query

---

## 5. Kiểm thử

- Dashboard

- Charts

- Alert

- Doctor Note

---

## 6. Báo cáo

- Kiến trúc hệ thống

- Firebase

- Công nghệ

---

# VI. Công việc chung của cả nhóm

## 1. Phân tích yêu cầu

Cả nhóm cùng thực hiện

- Functional Requirement

- Non-functional Requirement

- User Story

- Acceptance Criteria

---

## 2. Thiết kế hệ thống

Cùng xây dựng

- Use Case Diagram

- Activity Diagram

- Sequence Diagram

- ER Diagram

- Architecture Diagram

---

## 3. Thiết kế Database

Cùng thống nhất

Collections

```
users

patients

health_records

pill_schedules

pill_logs

sos_alerts
```

---

## 4. Quy chuẩn dự án

Thống nhất

- Theme

- Color

- Font

- Component

- Folder Structure

- Naming Convention

- Git Workflow

---

## 5. Tích hợp hệ thống

Ba thành viên cùng thực hiện

- Merge source

- Resolve Conflict

- Test Integration

- Test Firebase

- Test Notification

---

## 6. Kiểm thử cuối

Kiểm thử toàn hệ thống

- Login

- SOS

- GPS

- Notification

- CRUD

- Charts

- Background Service

---

## 7. Báo cáo

Cùng thực hiện

- Slide

- Video Demo

- Báo cáo

- Thuyết trình

---

# VII. Quy trình phát triển

## Giai đoạn 1

### Phân tích

Cả nhóm

↓

Phân tích yêu cầu

↓

User Story

↓

Acceptance Criteria

↓

Use Case

---

## Giai đoạn 2

### Thiết kế

Mỗi thành viên

↓

Thiết kế UI

↓

Thiết kế Database liên quan

↓

UI Flow

↓

Prototype

---

## Giai đoạn 3

### Phát triển

Mỗi người

↓

Flutter

↓

Riverpod

↓

Firebase

↓

Firestore

↓

Notification

---

## Giai đoạn 4

### Tích hợp

Merge

↓

Firebase

↓

Kiểm thử

↓

Sửa lỗi

---

## Giai đoạn 5

### Hoàn thiện

Testing

↓

Slide

↓

Video Demo

↓

Báo cáo

↓

Bảo vệ

---

# VIII. Ma trận phân công

| Công việc | TV1 | TV2 | TV3 |
|------------|:---:|:---:|:---:|
| Phân tích yêu cầu | ✓ | ✓ | ✓ |
| User Story | ✓ | ✓ | ✓ |
| Thiết kế UI | ✓ | ✓ | ✓ |
| Use Case | ✓ | ✓ | ✓ |
| Activity Diagram | ✓ | ✓ | ✓ |
| Database | ✓ | ✓ | ✓ |
| Flutter | ✓ | ✓ | ✓ |
| Riverpod | ✓ | ✓ | ✓ |
| Firebase | ✓ | ✓ | ✓ |
| Firestore | ✓ | ✓ | ✓ |
| Notification | ✓ | ✓ | ✓ |
| Testing | ✓ | ✓ | ✓ |
| Documentation | ✓ | ✓ | ✓ |
| Slide | ✓ | ✓ | ✓ |
| Demo | ✓ | ✓ | ✓ |
| Thuyết trình | ✓ | ✓ | ✓ |

---

# IX. Kết quả mong muốn

Sau khi hoàn thành dự án, **mỗi thành viên đều có cơ hội trải nghiệm đầy đủ quy trình phát triển một ứng dụng Flutter**, bao gồm:

- Phân tích yêu cầu.
- Thiết kế giao diện và luồng chức năng.
- Xây dựng ứng dụng Flutter.
- Tích hợp Firebase và Firestore.
- Kiểm thử và sửa lỗi.
- Viết tài liệu kỹ thuật.
- Chuẩn bị slide và tham gia bảo vệ dự án.

Cách phân chia theo **module chức năng** giúp đảm bảo khối lượng công việc tương đối cân bằng, giảm xung đột khi phát triển và phù hợp với mục tiêu học phần **Phát triển ứng dụng trên thiết bị di động**.