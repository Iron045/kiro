# GeriatricCare – Bộ tài liệu Phân tích & Thiết kế Hệ thống
## Mục lục tổng hợp

**Dự án:** GeriatricCare – Hệ thống Giám sát & Chăm sóc Người cao tuổi Độc cư  
**Phiên bản tài liệu:** 1.0.0  
**Ngày tạo:** 22/07/2026  
**Công nghệ:** Flutter + Firebase  

---

## Danh sách tài liệu

| # | File | Nội dung | Trang |
|---|---|---|---|
| 01 | [01_business_analysis.md](01_business_analysis.md) | Business Analysis: Bối cảnh, mô hình kinh doanh, phạm vi, rủi ro | ~9 trang |
| 02 | [02_problem_analysis.md](02_problem_analysis.md) | Problem Analysis: Fishbone, 5 Why, ma trận vấn đề | ~5 trang |
| 03 | [03_stakeholder_analysis.md](03_stakeholder_analysis.md) | Stakeholder Analysis: Persona, journey map, RACI | ~7 trang |
| 04 | [04_requirement_analysis.md](04_requirement_analysis.md) | Requirement Analysis: 10 module FR, MoSCoW, dependency map | ~8 trang |
| 05 | [05_functional_requirements.md](05_functional_requirements.md) | Functional Requirements: FR đầy đủ với flow, validation, BR | ~15 trang |
| 06 | [06_non_functional_requirements.md](06_non_functional_requirements.md) | Non-Functional Requirements: 10 nhóm NFR | ~10 trang |
| 07 | [07_business_rules.md](07_business_rules.md) | Business Rules: 50+ rules theo 10 nhóm | ~8 trang |
| 08 | [08_user_stories.md](08_user_stories.md) | User Stories: 25 US Agile với AC, SP, Sprint planning | ~12 trang |
| 09 | [09_use_cases.md](09_use_cases.md) | Use Cases: 26 UC đầy đủ main/alt/exception flows | ~15 trang |
| 10 | [10_uml_diagrams.md](10_uml_diagrams.md) | UML Diagrams: 12 PlantUML diagrams (8 loại) | ~20 trang |
| 11 | [11_database_design.md](11_database_design.md) | Database Design: Data dictionary, ER, indexes | ~12 trang |
| 12 | [12_firestore_design.md](12_firestore_design.md) | Firestore Design: Collection hierarchy, Security Rules, samples | ~15 trang |
| 13 | [13_api_design.md](13_api_design.md) | API Design: Cloud Functions, endpoints, request/response | ~15 trang |
| 14 | [14_flutter_architecture.md](14_flutter_architecture.md) | Flutter Architecture: Clean Architecture, DI, State, code mẫu | ~18 trang |
| 15 | [15_uiux_design.md](15_uiux_design.md) | UI/UX Design: Design tokens, 9 wireframes, components | ~15 trang |
| 16 | [16_security_design.md](16_security_design.md) | Security Design: OWASP, encryption, RBAC, audit | ~12 trang |
| 17 | [17_deployment_architecture.md](17_deployment_architecture.md) | Deployment Architecture: Flavors, CI/CD, Firebase, Play Store | ~12 trang |
| 18 | [18_testing_strategy.md](18_testing_strategy.md) | Testing Strategy: Unit/Widget/Integration/UAT test cases | ~15 trang |
| 19 | [19_future_improvements.md](19_future_improvements.md) | Future Improvements: Roadmap V1.5 → V3.0, AI, IoT | ~10 trang |

**Tổng: ~233 trang tài liệu**

---

## Hướng dẫn đọc tài liệu

### Cho Flutter Developer
1. Bắt đầu từ **Chương 4** (Requirement Analysis) để hiểu scope
2. Đọc **Chương 5** (Functional Requirements) cho chi tiết từng tính năng
3. Xem **Chương 10** (UML) để hiểu luồng hệ thống
4. Triển khai theo **Chương 14** (Flutter Architecture)
5. Tham khảo **Chương 12 + 13** (Firestore + API) để biết data structure

### Cho QA / Tester
1. **Chương 5** – Functional Requirements (acceptance criteria)
2. **Chương 7** – Business Rules (validation rules)
3. **Chương 18** – Testing Strategy (test cases sẵn sàng)

### Cho Product Owner / BA
1. **Chương 1–3** – Business & Stakeholder Analysis
2. **Chương 8** – User Stories (Sprint planning)
3. **Chương 19** – Future Improvements (roadmap)

### Cho Security Review
1. **Chương 6** – NFR (Security requirements)
2. **Chương 12** – Firestore Security Rules
3. **Chương 16** – Security Design (OWASP, encryption, audit)

---

## Công nghệ Stack tóm tắt

| Lớp | Công nghệ |
|---|---|
| Frontend | Flutter 3.19+ (Dart 3.0+) |
| State Management | Riverpod 2.x |
| Navigation | GoRouter 13.x |
| Architecture | Clean Architecture + Feature-first |
| DI | GetIt + Injectable |
| Local Storage | Hive (encrypted) |
| Remote Database | Cloud Firestore |
| Authentication | Firebase Auth |
| Push Notification | Firebase Cloud Messaging |
| File Storage | Firebase Storage |
| Maps | Google Maps Flutter |
| Location | Geolocator |
| Shake Detection | sensors_plus |
| Text-to-Speech | flutter_tts |
| Charts | fl_chart |
| PDF Generation | pdf + printing |
| Backend Logic | Firebase Cloud Functions (Node.js) |
| CI/CD | GitHub Actions |
| Distribution | Firebase App Distribution (staging), Play Store (prod) |
| Monitoring | Firebase Crashlytics + Performance |

---

## Quy ước trong tài liệu

| Ký hiệu | Ý nghĩa |
|---|---|
| ✅ | Bắt buộc / Hoàn thành / Bình thường |
| ⚠️ | Cảnh báo / Cần chú ý |
| 🔴 | Critical / Nguy hiểm |
| 🟠 | High priority |
| 🟡 | Medium priority |
| 🟢 | Low priority |
| ❌ | Không cho phép / Ngoài phạm vi |
| `SP` | Story Points |
| `BR-xxx` | Business Rule |
| `FR-xxx` | Functional Requirement |
| `NFR-xxx` | Non-Functional Requirement |
| `UC-xxx` | Use Case |
| `US-xxx` | User Story |
| `TC-xxx` | Test Case |
| `API-xxx` | API Endpoint |
