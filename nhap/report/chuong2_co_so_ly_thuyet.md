# CHƯƠNG 2: CƠ SỞ LÝ THUYẾT VÀ CÔNG NGHỆ

---

## 2.1 Lập trình hướng đối tượng (OOP)

### 2.1.1 Khái niệm và đặc điểm

Lập trình hướng đối tượng (Object-Oriented Programming – OOP) là một mô hình lập trình trong đó phần mềm được tổ chức xung quanh các **đối tượng** thay vì các hàm và logic. Mỗi đối tượng là một thực thể độc lập kết hợp dữ liệu (thuộc tính) và hành vi (phương thức).

OOP được xây dựng trên bốn nguyên lý cơ bản:

| Nguyên lý | Định nghĩa | Ứng dụng trong GeriatricCare |
|---|---|---|
| **Đóng gói (Encapsulation)** | Ẩn chi tiết triển khai, chỉ để lộ interface cần thiết | `HealthRepository` ẩn chi tiết Firestore; UI chỉ gọi UseCase |
| **Kế thừa (Inheritance)** | Lớp con kế thừa thuộc tính và phương thức từ lớp cha | `SosRepositoryImpl` kế thừa interface `SosRepository` |
| **Đa hình (Polymorphism)** | Đối tượng có thể có nhiều hình thức khác nhau | `UseCase<Type, Params>` – một base class cho mọi use case |
| **Trừu tượng (Abstraction)** | Mô hình hóa thực thể bằng cách tập trung vào thuộc tính quan trọng | `HealthRecordEntity` trừu tượng hóa dữ liệu sức khỏe |

### 2.1.2 Nguyên tắc SOLID

SOLID là tập hợp 5 nguyên tắc thiết kế phần mềm hướng đối tượng giúp code dễ bảo trì, mở rộng và kiểm thử:

| Nguyên tắc | Mô tả | Ví dụ trong dự án |
|---|---|---|
| **S** – Single Responsibility | Mỗi lớp chỉ có một lý do để thay đổi | `TriggerSosUseCase` chỉ xử lý logic kích hoạt SOS |
| **O** – Open/Closed | Mở để mở rộng, đóng để sửa đổi | Thêm `HealthType` mới không sửa code hiện có |
| **L** – Liskov Substitution | Lớp con có thể thay thế lớp cha | `HealthRepositoryImpl` thay thế `HealthRepository` |
| **I** – Interface Segregation | Nhiều interface nhỏ hơn một interface lớn | Tách `SosRepository`, `HealthRepository`, `MedicationRepository` |
| **D** – Dependency Inversion | Phụ thuộc vào trừu tượng, không phụ thuộc vào cụ thể | Domain Layer không phụ thuộc Firebase; inject qua GetIt |

### 2.1.3 Design Patterns sử dụng

**Repository Pattern:** Tách biệt logic truy cập dữ liệu khỏi logic nghiệp vụ. Domain Layer chỉ giao tiếp qua interface `Repository`, không biết về Firestore hay Hive.

**Observer Pattern:** Sử dụng `Stream` và Riverpod để lắng nghe thay đổi dữ liệu Firestore theo thời gian thực. Dashboard cập nhật tự động khi có dữ liệu mới.

**Factory Pattern:** `AppConfig.initialize(env)` tạo đối tượng cấu hình khác nhau theo môi trường (dev/staging/prod).

**Singleton Pattern:** `GetIt.instance` – container Dependency Injection toàn cục, đảm bảo mỗi service chỉ có một instance.

---

## 2.2 Phân tích và thiết kế hướng đối tượng (OOA/OOD)

### 2.2.1 Quy trình OOA/OOD

Đồ án này áp dụng quy trình phân tích và thiết kế hướng đối tượng theo chuẩn **UML 2.5** gồm các bước:

```
Yêu cầu → Use Case Model → Domain Model → Design Model → Implementation
             (UC Diagram)   (Class Diagram)  (Sequence, State)   (Code)
```

### 2.2.2 Ngôn ngữ mô hình hóa UML 2.5

UML (Unified Modeling Language) 2.5 cung cấp 14 loại sơ đồ chia thành hai nhóm:

**Sơ đồ cấu trúc (Structural Diagrams):**
- **Class Diagram:** Mô tả cấu trúc lớp, thuộc tính, phương thức và mối quan hệ.
- **Component Diagram:** Mô tả các thành phần phần mềm và sự phụ thuộc.
- **Deployment Diagram:** Mô tả cách triển khai vật lý hệ thống.
- **Package Diagram:** Mô tả tổ chức các gói (package) trong codebase.

**Sơ đồ hành vi (Behavioral Diagrams):**
- **Use Case Diagram:** Mô tả chức năng hệ thống từ góc nhìn người dùng.
- **Activity Diagram:** Mô tả luồng hoạt động và quyết định trong một quy trình.
- **Sequence Diagram:** Mô tả tương tác theo thứ tự thời gian giữa các đối tượng.
- **State Machine Diagram:** Mô tả vòng đời của một đối tượng qua các trạng thái.

---

## 2.3 Kiến trúc phần mềm

### 2.3.1 Clean Architecture

Clean Architecture (Robert C. Martin, 2012) là kiến trúc phân tầng nhằm tách biệt hoàn toàn các mối quan tâm (separation of concerns), giúp hệ thống dễ kiểm thử, bảo trì và thay thế công nghệ.

```
┌──────────────────────────────────────────────┐
│           PRESENTATION LAYER                 │
│   (Screens, Widgets, State Notifiers)        │
├──────────────────────────────────────────────┤
│           APPLICATION LAYER                  │
│         (Use Cases, DTOs, Mappers)           │
├──────────────────────────────────────────────┤
│             DOMAIN LAYER                     │
│   (Entities, Repository Interfaces,          │
│    Value Objects, Business Rules)            │
├──────────────────────────────────────────────┤
│              DATA LAYER                      │
│  (Repository Impl, Remote DS, Local DS)      │
├──────────────────────────────────────────────┤
│          INFRASTRUCTURE LAYER                │
│  (Firebase, Hive, FCM, GPS, TTS, Sensors)    │
└──────────────────────────────────────────────┘
         ↑ Phụ thuộc chỉ đi vào trong ↑
```

**Quy tắc phụ thuộc (Dependency Rule):** Các lớp bên ngoài phụ thuộc vào các lớp bên trong, không bao giờ ngược lại. Domain Layer là hạt nhân thuần túy của ứng dụng, không phụ thuộc bất kỳ framework hay thư viện nào.

**Lợi ích áp dụng trong GeriatricCare:**
- Có thể thay thế Firestore bằng bất kỳ database nào mà không ảnh hưởng Domain Layer.
- Domain Layer có thể kiểm thử hoàn toàn bằng mock object, không cần Firebase thật.
- Mỗi feature độc lập, nhóm có thể phát triển song song.

### 2.3.2 Feature-first Folder Structure

Thay vì tổ chức theo layer (tất cả repository ở một chỗ, tất cả screen ở một chỗ), dự án tổ chức theo feature. Mỗi feature chứa đầy đủ 3 layer: data, domain, presentation.

```
features/
├── auth/        (data/ domain/ presentation/)
├── sos/         (data/ domain/ presentation/)
├── health/      (data/ domain/ presentation/)
├── medication/  (data/ domain/ presentation/)
└── dashboard/   (family/ doctor/ elderly/)
```

### 2.3.3 Quản lý trạng thái với Riverpod

**Riverpod** là thư viện quản lý trạng thái (State Management) thế hệ mới cho Flutter, khắc phục hạn chế của Provider truyền thống:

| Đặc điểm | Mô tả |
|---|---|
| Compile-safe | Lỗi được phát hiện tại compile time, không runtime |
| Testable | Provider có thể override trong test dễ dàng |
| No context | Không cần `BuildContext` để đọc provider |
| Auto-dispose | Tự động giải phóng tài nguyên khi không còn listener |
| Family | Tạo provider với tham số động (ví dụ: theo `elderlyId`) |

---

## 2.4 Flutter và Dart

### 2.4.1 Flutter Framework

**Flutter** là framework phát triển ứng dụng đa nền tảng (cross-platform) do Google phát triển, ra mắt phiên bản ổn định đầu tiên năm 2018. Flutter sử dụng ngôn ngữ **Dart** và render UI thông qua engine đồ họa riêng (Skia/Impeller), không phụ thuộc vào các widget native của hệ điều hành.

**Ưu điểm của Flutter:**

| Tiêu chí | Flutter | React Native | Kotlin (Native) |
|---|---|---|---|
| Hiệu năng | ★★★★★ | ★★★★ | ★★★★★ |
| Tốc độ phát triển | ★★★★★ | ★★★★ | ★★★ |
| Codebase dùng chung | ~95% | ~85% | ❌ |
| UI nhất quán | ★★★★★ | ★★★ | N/A |
| Hỗ trợ Dart | ✅ | ❌ | ❌ |
| Cộng đồng | Lớn & tăng nhanh | Lớn | Lớn |

**Lý do chọn Flutter cho GeriatricCare:**
- Một codebase cho cả Android và (tương lai) iOS.
- Hot Reload giúp tăng tốc độ phát triển.
- Widget system linh hoạt, dễ xây dựng giao diện tùy chỉnh cho người cao tuổi.
- Tích hợp tốt với Firebase SDK.
- Performance gần ngang native nhờ Ahead-of-Time compilation.

### 2.4.2 Ngôn ngữ Dart

Dart là ngôn ngữ lập trình hướng đối tượng, strongly-typed, được tối ưu cho Flutter. Các đặc điểm quan trọng:

- **Null Safety:** Hệ thống kiểu ngăn chặn lỗi NullPointerException tại compile time.
- **Async/Await:** Xử lý bất đồng bộ (network calls, database) rõ ràng và dễ đọc.
- **Generics:** Hỗ trợ kiểu dữ liệu tổng quát, ví dụ `Either<Failure, Success>`.
- **Extension Methods:** Thêm phương thức vào lớp có sẵn mà không cần kế thừa.
- **Freezed/JsonSerializable:** Code generation cho immutable data classes và JSON mapping.

### 2.4.3 Widget System

Flutter xây dựng UI thông qua hệ thống Widget theo mô hình **Composition over Inheritance**. Mọi thứ trong Flutter đều là Widget:

```
Widget Tree (ví dụ màn hình SOS):
Scaffold
└── SafeArea
    └── Column
        ├── SOSCountdownDisplay  ← Custom widget
        ├── SOSButton            ← Custom widget (80dp, đỏ)
        └── CancelButton         ← Custom widget
```

---

## 2.5 Firebase Platform

### 2.5.1 Tổng quan Firebase

**Firebase** là nền tảng phát triển ứng dụng Backend-as-a-Service (BaaS) do Google cung cấp. Firebase cung cấp bộ công cụ tích hợp giúp nhà phát triển xây dựng, cải thiện và phát triển ứng dụng mà không cần quản lý server.

| Dịch vụ | Vai trò trong GeriatricCare |
|---|---|
| **Firebase Authentication** | Đăng ký, đăng nhập, quản lý phiên, OTP |
| **Cloud Firestore** | Lưu trữ dữ liệu NoSQL, realtime listeners, offline sync |
| **Firebase Storage** | Lưu trữ ảnh đại diện, file báo cáo PDF |
| **Firebase Cloud Messaging (FCM)** | Gửi push notification SOS, nhắc thuốc, cảnh báo sức khỏe |
| **Firebase Analytics** | Theo dõi hành vi người dùng, DAU/MAU |
| **Firebase Crashlytics** | Theo dõi và báo cáo crash ứng dụng |
| **Firebase Performance** | Đo hiệu năng app, trace SOS trigger time |
| **Firebase Remote Config** | Feature flags, cấu hình từ xa không cần cập nhật app |
| **Cloud Functions** | Serverless logic: gửi SOS alert, health alert automation |

### 2.5.2 Cloud Firestore

Cloud Firestore là cơ sở dữ liệu NoSQL document-based với các đặc điểm:

- **Document Model:** Dữ liệu lưu dạng Document trong Collection, hỗ trợ subcollection lồng nhau.
- **Realtime Updates:** SDK tự động đẩy thay đổi đến client qua WebSocket, không cần polling.
- **Offline Persistence:** Firestore SDK cache dữ liệu local, cho phép đọc/ghi offline.
- **Security Rules:** Kiểm soát truy cập tại tầng database, không phụ thuộc logic server.
- **Scalability:** Tự động scale, không giới hạn về số lượng document hay concurrent connections.

**So sánh với MySQL (RDBMS):**

| Tiêu chí | Firestore (NoSQL) | MySQL (SQL) |
|---|---|---|
| Cấu trúc | Linh hoạt (schema-less) | Cố định (schema) |
| Realtime | ✅ Native | ❌ Cần polling/WebSocket |
| Offline sync | ✅ Built-in | ❌ Tự implement |
| Scale | Tự động | Cần cấu hình thủ công |
| JOIN | ❌ Không hỗ trợ | ✅ Hỗ trợ đầy đủ |
| Phù hợp với | Mobile app realtime | Hệ thống phức tạp |

### 2.5.3 Firebase Authentication

Firebase Authentication cung cấp SDK xác thực người dùng với nhiều phương thức: Email/Password, Phone (OTP), Google, Facebook. Trong GeriatricCare sử dụng Email/Password và Email OTP.

**Quy trình xác thực:**
```
Client → Firebase Auth SDK → Google Identity Platform
                ↓
         ID Token (JWT, 1h)  +  Refresh Token (30d)
                ↓
    Mọi request đến Firestore đính kèm ID Token
                ↓
    Firestore Security Rules kiểm tra token
```

### 2.5.4 Firebase Cloud Messaging (FCM)

FCM là dịch vụ gửi thông báo đẩy (push notification) miễn phí, hỗ trợ Android, iOS và Web. Trong GeriatricCare, FCM đóng vai trò quan trọng nhất trong tính năng SOS.

**Loại thông báo FCM:**
- **Notification Message:** Hiển thị tự động trên notification tray (khi app background/killed).
- **Data Message:** App xử lý trong code (khi app foreground).
- **HIGH Priority:** Đánh thức thiết bị ngay lập tức (dùng cho SOS).

---

## 2.6 Các công nghệ và thư viện bổ trợ

### 2.6.1 Danh sách thư viện chính

| Thư viện | Phiên bản | Chức năng |
|---|---|---|
| `flutter_riverpod` | 2.4.x | State management |
| `go_router` | 13.x | Navigation, deep links |
| `get_it` + `injectable` | 7.x / 2.x | Dependency injection |
| `freezed` + `json_serializable` | 2.x | Immutable models, JSON |
| `dartz` | 0.10.x | Either<Failure, Success> |
| `hive_flutter` | 2.x | Local NoSQL database |
| `geolocator` | 10.x | GPS location |
| `google_maps_flutter` | 2.x | Bản đồ Google Maps |
| `sensors_plus` | 4.x | Accelerometer (Shake Detection) |
| `flutter_local_notifications` | 16.x | Local notification (nhắc thuốc) |
| `flutter_tts` | 3.x | Text-to-Speech tiếng Việt |
| `fl_chart` | 0.66.x | Biểu đồ sức khỏe |
| `pdf` + `printing` | 3.x / 5.x | Xuất báo cáo PDF |
| `flutter_secure_storage` | 9.x | Lưu token an toàn |
| `connectivity_plus` | 5.x | Kiểm tra kết nối mạng |
| `workmanager` | 0.5.x | Background task (sync offline) |

### 2.6.2 Google Maps Platform

Google Maps Flutter SDK cho phép tích hợp bản đồ tương tác trong ứng dụng Flutter. Trong GeriatricCare sử dụng:
- **Marker:** Hiển thị vị trí người cao tuổi với ảnh đại diện.
- **Polyline:** Vẽ lịch sử di chuyển 24 giờ.
- **MapType:** Chế độ normal và hybrid.

### 2.6.3 Geolocator và GPS

Thư viện `geolocator` cung cấp API truy cập GPS của thiết bị:
- `getCurrentPosition()`: Lấy vị trí hiện tại một lần.
- `getPositionStream()`: Stream vị trí cập nhật liên tục.
- Hỗ trợ `LocationPermission`, `LocationAccuracy`.

**Chiến lược tiêu thụ pin:**
- Accuracy: `LocationAccuracy.balanced` (50m, ít pin hơn `best`).
- Cập nhật mỗi 15 phút khi background.
- Dừng GPS khi pin < 15% để bảo tồn pin cho SOS.

### 2.6.4 Text-to-Speech (flutter_tts)

`flutter_tts` cung cấp khả năng chuyển văn bản thành giọng nói, hỗ trợ tiếng Việt thông qua engine TTS của Android:

```dart
await flutterTts.setLanguage("vi-VN");
await flutterTts.setSpeechRate(0.8);  // Chậm hơn cho người cao tuổi
await flutterTts.speak("Đã đến giờ uống thuốc huyết áp Amlodipine 5mg");
```

### 2.6.5 Material Design 3

Material Design 3 (Material You) là hệ thống thiết kế của Google với các cải tiến:
- **Dynamic Color:** Màu sắc thích ứng theo wallpaper người dùng.
- **Adaptive Components:** Widget tự điều chỉnh theo kích thước màn hình.
- **Accessibility-first:** Contrast ratio, touch target size được quy định rõ ràng.

Đối với GeriatricCare, áp dụng Material Design 3 với bộ màu tùy chỉnh và typography scale riêng dành cho người cao tuổi.

---

*Kết thúc Chương 2*
