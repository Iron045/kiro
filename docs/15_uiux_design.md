# Chương 15: UI/UX Design
## GeriatricCare – Thiết kế Giao diện & Trải nghiệm người dùng

---

## 15.1 Design Philosophy

**Senior-first Design** – Mọi quyết định thiết kế ưu tiên người cao tuổi trước.

| Nguyên tắc | Áp dụng |
|---|---|
| **Simplicity** | Tối đa 3 hành động chính per màn hình |
| **Clarity** | Font lớn, contrast cao, icon rõ nghĩa |
| **Forgiveness** | Luôn có nút Back, xác nhận trước khi thực hiện |
| **Feedback** | Mọi hành động có phản hồi ngay (haptic + visual) |
| **Consistency** | Layout nhất quán, không thay đổi vị trí nút |
| **Accessibility** | TTS, font scale, high contrast mode |

---

## 15.2 Design Tokens – Material Design 3

### Color Scheme (Light Mode)

```dart
// core/theme/color_scheme.dart
const geriatricCareColorScheme = ColorScheme(
  brightness: Brightness.light,
  primary: Color(0xFF1565C0),        // Xanh đậm – nút chính
  onPrimary: Color(0xFFFFFFFF),
  primaryContainer: Color(0xFFD6E4FF),
  secondary: Color(0xFF2E7D32),      // Xanh lá – trạng thái tốt
  onSecondary: Color(0xFFFFFFFF),
  error: Color(0xFFB71C1C),          // Đỏ đậm – SOS, nguy hiểm
  onError: Color(0xFFFFFFFF),
  errorContainer: Color(0xFFFFDAD6),
  surface: Color(0xFFF8F9FA),
  onSurface: Color(0xFF1A1A1A),
  outline: Color(0xFF757575),
  // SOS đặc biệt
  // sosRed: Color(0xFFD32F2F)
  // warningAmber: Color(0xFFFF8F00)
  // safeGreen: Color(0xFF388E3C)
);
```

### Color Scheme (Dark Mode)

```dart
const geriatricCareDarkColorScheme = ColorScheme(
  brightness: Brightness.dark,
  primary: Color(0xFF90CAF9),
  onPrimary: Color(0xFF003366),
  secondary: Color(0xFF81C784),
  onSecondary: Color(0xFF003300),
  error: Color(0xFFEF9A9A),
  surface: Color(0xFF121212),
  onSurface: Color(0xFFE0E0E0),
);
```

### Typography

```dart
// core/theme/typography.dart
// Base: Roboto (default), hỗ trợ scaling
TextTheme buildTextTheme(double fontScale) => TextTheme(
  displayLarge:  TextStyle(fontSize: 57 * fontScale, fontWeight: FontWeight.w400),
  headlineLarge: TextStyle(fontSize: 32 * fontScale, fontWeight: FontWeight.w600),
  headlineMedium:TextStyle(fontSize: 28 * fontScale, fontWeight: FontWeight.w600),
  titleLarge:    TextStyle(fontSize: 22 * fontScale, fontWeight: FontWeight.w600),
  titleMedium:   TextStyle(fontSize: 18 * fontScale, fontWeight: FontWeight.w500),
  bodyLarge:     TextStyle(fontSize: 18 * fontScale, fontWeight: FontWeight.w400),
  bodyMedium:    TextStyle(fontSize: 16 * fontScale, fontWeight: FontWeight.w400),
  labelLarge:    TextStyle(fontSize: 16 * fontScale, fontWeight: FontWeight.w600),
);

// Font scale options: 1.0 (Normal), 1.15 (Large), 1.3 (Extra Large), 1.5 (Max)
```

### Spacing & Sizing

```dart
// Minimum touch target: 48dp (Material spec)
// SOS button: 80dp minimum
// Padding standard: 16dp
// Card padding: 16dp
// Section spacing: 24dp
// Border radius: 12dp (cards), 8dp (buttons), 50% (FAB)
```

---

## 15.3 Wireframes – Màn hình Người cao tuổi

### SCR-01: Màn hình chính (Elderly Home)

```
┌─────────────────────────────────────┐
│ ☰  GeriatricCare      🔔  ⚙️       │  ← AppBar (height 64dp)
├─────────────────────────────────────┤
│                                     │
│  Xin chào, bà Nguyễn! 👋           │  ← greeting, titleLarge
│  Thứ Ba, 22 tháng 7 năm 2026       │  ← bodyMedium, muted
│                                     │
├─────────────────────────────────────┤
│  ┌─────────────────────────────┐    │
│  │  🏥  Sức khỏe hôm nay       │    │  ← Card
│  │  ───────────────────────    │    │
│  │  💉 HA:  148/92 ⚠️          │    │  ← warning icon, red
│  │  ❤️ Nhịp tim: 76 bpm  ✅    │    │
│  │  🩸 ĐH:   6.2 mmol/L ✅    │    │
│  │                             │    │
│  │  [+ Nhập chỉ số mới]        │    │  ← Button, 48dp height
│  └─────────────────────────────┘    │
│                                     │
│  ┌─────────────────────────────┐    │
│  │  💊  Thuốc hôm nay           │    │
│  │  ───────────────────────    │    │
│  │  ✅ 07:00 Amlodipine 5mg    │    │
│  │  ⏳ 12:00 Metformin 500mg   │    │  ← pending, amber
│  │  ○  19:00 Aspirin 81mg      │    │  ← scheduled
│  └─────────────────────────────┘    │
│                                     │
├─────────────────────────────────────┤
│                                     │
│  ┌─────────────────────────────┐    │
│  │                             │    │
│  │     🆘  GỌI CỨU GIÚP       │    │  ← SOS Button
│  │                             │    │  ← 80dp height, red
│  └─────────────────────────────┘    │
│                                     │
├──────┬──────────┬──────────┬────────┤
│ 🏠   │  💊      │  📊      │  👤    │  ← Bottom Nav (56dp)
│Trang │  Thuốc   │ Sức khỏe │ Hồ sơ │
│chính │          │          │       │
└──────┴──────────┴──────────┴────────┘

Mục tiêu: Người cao tuổi thấy ngay tình trạng sức khỏe và có thể
          bấm SOS trong vòng 2 giây.
Widget:   Scaffold > SafeArea > SingleChildScrollView > Column
```

---

### SCR-02: Màn hình đếm ngược SOS

```
┌─────────────────────────────────────┐
│                                     │ ← Full screen, background RED
│                                     │
│                                     │
│         ⚠️ ĐANG GỬI CẦU CỨU        │  ← titleLarge, white, center
│                                     │
│                                     │
│              ┌─────┐               │
│              │  3  │               │  ← displayLarge (72sp), white
│              └─────┘               │
│                                     │
│    Đang gửi đến người thân...       │  ← bodyLarge, white
│    Lắc điện thoại: đã phát hiện    │
│                                     │
│                                     │
│   ┌─────────────────────────────┐  │
│   │                             │  │
│   │         ✕  HỦY             │  │  ← 80dp, white fill, red text
│   │                             │  │
│   └─────────────────────────────┘  │
│                                     │
└─────────────────────────────────────┘

Accessibility: WillPopScope disabled (không cho back)
               Âm thanh beep 1 giây/lần
               Haptic feedback mỗi giây
```

---

### SCR-03: Nhập chỉ số sức khỏe

```
┌─────────────────────────────────────┐
│ ←  Nhập Huyết Áp                   │  ← AppBar
├─────────────────────────────────────┤
│                                     │
│  Tâm thu (SYS)                      │  ← labelLarge
│  ┌─────────────────────────────┐   │
│  │      148          mmHg      │   │  ← headlineLarge, center
│  └─────────────────────────────┘   │
│                                     │
│  Tâm trương (DIA)                   │
│  ┌─────────────────────────────┐   │
│  │       92          mmHg      │   │
│  └─────────────────────────────┘   │
│                                     │
│  Nhịp tim                           │
│  ┌─────────────────────────────┐   │
│  │       78          bpm       │   │
│  └─────────────────────────────┘   │
│                                     │
│  Thời gian: 22/07/2026  07:30      │  ← tự động điền
│  [Ghi chú tuỳ chọn...]             │  ← TextField, hint
│                                     │
│  ┌──────────────┐  ┌────────────┐  │
│  │  1  2  3     │  │ ← Backspace │  │  ← Numeric keypad lớn
│  │  4  5  6     │  │            │  │  ← mỗi nút 72dp
│  │  7  8  9     │  │            │  │
│  │     0        │  │            │  │
│  └──────────────┘  └────────────┘  │
│                                     │
│  ┌─────────────────────────────┐   │
│  │         💾  LƯU             │   │  ← 56dp, primary color
│  └─────────────────────────────┘   │
└─────────────────────────────────────┘

Validation: SYS 50–300, DIA 30–200, Pulse 30–250
Alert: Hiển thị warning banner ngay khi nhập nếu vượt ngưỡng
```

---

### SCR-04: Nhắc uống thuốc (Notification + Màn hình xác nhận)

```
╔═════════════════════════════════════╗  ← Heads-up notification
║  GeriatricCare   💊                 ║
║  Đến giờ uống thuốc Amlodipine 5mg  ║
║  Liều: 1 viên – Uống sau ăn sáng   ║
║  [✅ Đã uống]         [⏭ Bỏ qua]  ║
╚═════════════════════════════════════╝

── Khi mở app ──────────────────────────
┌─────────────────────────────────────┐
│ ←  Xác nhận uống thuốc             │
├─────────────────────────────────────┤
│                                     │
│         💊                          │  ← icon lớn, 80dp
│                                     │
│   Amlodipine 5mg                    │  ← headlineMedium
│   1 viên – Sau ăn sáng             │  ← bodyLarge, muted
│   Lịch nhắc: 07:00                 │
│                                     │
│  ┌─────────────────────────────┐   │
│  │    🔊  Nghe lại nhắc nhở    │   │  ← TTS replay button
│  └─────────────────────────────┘   │
│                                     │
│  ┌─────────────────────────────┐   │
│  │      ✅  ĐÃ UỐNG THUỐC     │   │  ← 64dp, green
│  └─────────────────────────────┘   │
│                                     │
│  ┌─────────────────────────────┐   │
│  │      ⏭  BỎ QUA LẦN NÀY    │   │  ← 56dp, outlined
│  └─────────────────────────────┘   │
└─────────────────────────────────────┘
```

---

## 15.4 Wireframes – Màn hình Người thân

### SCR-05: Family Dashboard

```
┌─────────────────────────────────────┐
│ GeriatricCare           🔔 (3)  ⚙️  │  ← badge 3 notifications
├─────────────────────────────────────┤
│  Theo dõi người thân               │  ← titleLarge
│  Thứ Ba, 22 tháng 7 năm 2026       │
├─────────────────────────────────────┤
│                                     │
│  ┌─────────────────────────────┐   │
│  │ 🟢  Nguyễn Thị B  (Mẹ)     │   │  ← online indicator
│  │     Cập nhật 5 phút trước   │   │
│  │  ─────────────────────────  │   │
│  │  💉 148/92 ⚠️  ❤️ 76bpm   │   │  ← metrics row
│  │  💊 Thuốc: 1/3 hôm nay      │   │
│  │  🔋 Pin: 72%   📍 Q.1       │   │
│  │                             │   │
│  │  [📍 Xem bản đồ] [📊 Chi tiết]│  │  ← 2 action buttons
│  └─────────────────────────────┘   │
│                                     │
│  ┌─────────────────────────────┐   │
│  │ 🟢  Trần Văn C  (Ông nội)  │   │
│  │     Cập nhật 2 giờ trước    │   │
│  │  ─────────────────────────  │   │
│  │  💉 125/82 ✅   ❤️ 72bpm   │   │
│  │  💊 Thuốc: 3/3 ✅            │   │
│  │  🔋 Pin: 45%   📍 Q.3       │   │
│  └─────────────────────────────┘   │
│                                     │
│  [+ Thêm người thân]               │
│                                     │
├──────┬──────────┬──────────┬────────┤
│ 🏠   │  📊      │  🗺️      │  👤   │
│Tổng  │ Sức khỏe │  Bản đồ  │ Cá nhân│
└──────┴──────────┴──────────┴────────┘

SOS Alert Banner (khi có SOS active):
┌─────────────────────────────────────┐
│ 🚨 MẸ ĐỀU CẦU GIÚP ĐỠ!           │  ← RED banner, flashing
│    08:04 – 10.7769, 106.7009       │
│    [📍 MỞ BẢN ĐỒ]  [✅ XÁC NHẬN]  │
└─────────────────────────────────────┘
```

---

### SCR-06: Chi tiết sức khỏe + Biểu đồ

```
┌─────────────────────────────────────┐
│ ←  Nguyễn Thị B – Sức khỏe        │
├─────────────────────────────────────┤
│  [Huyết áp][Đường huyết][Nhịp tim] │  ← Tab bar, scrollable
├─────────────────────────────────────┤
│                                     │
│  ┌─────────────────────────────┐   │
│  │  Huyết áp (mmHg)            │   │
│  │                             │   │
│  │  160 ┤  ·           ⚠️ ngưỡng│  │
│  │  140 ┼─────────────────     │   │
│  │  120 ┤    ╱╲    ╱           │   │
│  │  100 ┤   ╱  ╲──╱            │   │
│  │   80 ┼──╱                   │   │
│  │      └──┬──┬──┬──┬──┬──┬──  │   │
│  │       T2 T3 T4 T5 T6 T7 T8  │   │
│  │                             │   │
│  └─────────────────────────────┘   │
│                                     │
│  [Ngày] [Tuần] [Tháng] [Năm]       │  ← period selector
│                                     │
│  Lịch sử gần nhất:                 │
│  ┌─────────────────────────────┐   │
│  │  22/07  07:30  148/92 ⚠️   │   │
│  │  21/07  07:15  135/85 ✅    │   │
│  │  20/07  08:00  142/88 ⚠️   │   │
│  └─────────────────────────────┘   │
│                                     │
│  [📄 Xuất báo cáo PDF]             │
└─────────────────────────────────────┘
```

---

### SCR-07: Bản đồ GPS

```
┌─────────────────────────────────────┐
│ ←  Vị trí: Nguyễn Thị B           │
├─────────────────────────────────────┤
│                                     │
│  ┌─────────────────────────────┐   │
│  │                             │   │
│  │   [Google Maps full view]   │   │  ← 60% screen height
│  │                             │   │
│  │      📍                     │   │  ← marker người cao tuổi
│  │   (tên + avatar nhỏ)        │   │
│  │                             │   │
│  └─────────────────────────────┘   │
│                                     │
│  ┌─────────────────────────────┐   │
│  │  📍 Nguyễn Thị B            │   │
│  │  123 Nguyễn Huệ, Q.1        │   │
│  │  Cập nhật: 08:15  (5 phút)  │   │
│  │  🔋 Pin: 72%                 │   │
│  │                             │   │
│  │  [🗺️ Chỉ đường]  [📞 Gọi]  │   │
│  └─────────────────────────────┘   │
│                                     │
│  Lịch sử 24h:                      │  ← collapsible
│  08:15 – Q.1 (Hiện tại)            │
│  06:30 – Q.1 (Nhà)                 │
└─────────────────────────────────────┘
```

---

### SCR-08: Tạo lịch uống thuốc

```
┌─────────────────────────────────────┐
│ ←  Thêm thuốc mới                  │
├─────────────────────────────────────┤
│                                     │
│  Tên thuốc *                        │
│  ┌─────────────────────────────┐   │
│  │ Amlodipine 5mg              │   │  ← TextField, 48dp
│  └─────────────────────────────┘   │
│                                     │
│  Liều lượng *                       │
│  ┌─────────────────────────────┐   │
│  │ 1 viên                      │   │
│  └─────────────────────────────┘   │
│                                     │
│  Số lần uống mỗi ngày              │
│  ┌────┐  ┌────┐  ┌────┐  ┌────┐  │
│  │ 1  │  │ 2  │  │ 3  │  │ 4+ │  │  ← SegmentedButton
│  └────┘  └────┘  └────┘  └────┘  │
│                                     │
│  Thời gian uống:                   │
│  ┌─────────────┐  [+ Thêm giờ]    │
│  │   07:00  ×  │                   │  ← Chips
│  └─────────────┘                   │
│                                     │
│  Ngày bắt đầu: 22/07/2026  📅     │
│  Ngày kết thúc: (Không giới hạn) ↗│
│                                     │
│  Ghi chú:                          │
│  ┌─────────────────────────────┐   │
│  │ Uống sau khi ăn sáng        │   │
│  └─────────────────────────────┘   │
│                                     │
│  ┌─────────────────────────────┐   │
│  │       💾  LƯU LỊCH THUỐC   │   │
│  └─────────────────────────────┘   │
└─────────────────────────────────────┘
```

---

## 15.5 Wireframes – Doctor Dashboard

### SCR-09: Doctor Dashboard

```
┌─────────────────────────────────────┐
│ GeriatricCare – Bác sĩ      🔔 ⚙️  │
├─────────────────────────────────────┤
│  Danh sách bệnh nhân (3)           │
│  [🔍 Tìm kiếm bệnh nhân...]        │
├─────────────────────────────────────┤
│  ┌─────────────────────────────┐   │
│  │ 👤 Nguyễn Thị B, 78 tuổi   │   │
│  │    Tăng HA • ĐTĐ type 2    │   │
│  │  ─────────────────────────  │   │
│  │  💉 148/92 ⚠️  📅 22/07   │   │
│  │  💊 Tuân thủ thuốc: 92%    │   │
│  │  [Xem chi tiết →]          │   │
│  └─────────────────────────────┘   │
│                                     │
│  ┌─────────────────────────────┐   │
│  │ 👤 Lê Văn D, 72 tuổi       │   │
│  │    Bệnh tim • Tiểu đường   │   │
│  │  SpO2: 91% 🔴               │   │  ← alert highlight
│  │  [Xem chi tiết →]          │   │
│  └─────────────────────────────┘   │
├──────┬──────────┬──────────┬────────┤
│ 🏥   │  📊      │  📝      │  👤   │
│BN    │ Thống kê │  Ghi chú │ Cá nhân│
└──────┴──────────┴──────────┴────────┘
```

---

## 15.6 Shared Components

### SosButton Widget

```dart
class SosButton extends StatelessWidget {
  final VoidCallback onPressed;

  const SosButton({super.key, required this.onPressed});

  @override
  Widget build(BuildContext context) {
    return Semantics(
      label: 'Nút cấp cứu SOS. Bấm để gọi người thân',
      button: true,
      child: Material(
        elevation: 8,
        borderRadius: BorderRadius.circular(16),
        color: const Color(0xFFD32F2F),
        child: InkWell(
          onTap: () {
            HapticFeedback.heavyImpact();
            onPressed();
          },
          borderRadius: BorderRadius.circular(16),
          child: Container(
            height: 80,
            width: double.infinity,
            alignment: Alignment.center,
            child: Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                const Icon(Icons.emergency, color: Colors.white, size: 32),
                const SizedBox(width: 12),
                Text(
                  'GỌI CỨU GIÚP',
                  style: Theme.of(context).textTheme.headlineMedium?.copyWith(
                    color: Colors.white,
                    fontWeight: FontWeight.w900,
                    letterSpacing: 1.5,
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```

### HealthMetricCard Widget

```dart
class HealthMetricCard extends StatelessWidget {
  final String label;
  final String value;
  final String unit;
  final bool isAbnormal;
  final IconData icon;

  @override
  Widget build(BuildContext context) {
    final color = isAbnormal
        ? Theme.of(context).colorScheme.error
        : const Color(0xFF2E7D32);
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(12),
        child: Row(children: [
          Icon(icon, color: color, size: 28),
          const SizedBox(width: 12),
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(label, style: Theme.of(context).textTheme.bodyMedium),
                Text(
                  '$value $unit',
                  style: Theme.of(context).textTheme.titleMedium?.copyWith(
                    color: color, fontWeight: FontWeight.bold,
                  ),
                ),
              ],
            ),
          ),
          if (isAbnormal)
            const Icon(Icons.warning_amber, color: Colors.amber, size: 24),
        ]),
      ),
    );
  }
}
```

---

## 15.7 Accessibility Features

| Tính năng | Triển khai |
|---|---|
| **Semantic Labels** | Tất cả widget tương tác có `Semantics` rõ ràng |
| **TalkBack / VoiceOver** | Test với screen reader, mô tả đầy đủ |
| **Large Touch Targets** | Minimum 48dp (SOS: 80dp) |
| **Font Scaling** | Hỗ trợ 1.0x–1.5x, không bị layout break |
| **High Contrast Mode** | Toggle trong Settings, contrast ratio ≥ 4.5:1 |
| **Color Blind Friendly** | Không chỉ dùng màu, kèm icon + text |
| **Reduced Motion** | Tắt animation khi bật reduce motion |
| **Haptic Feedback** | Confirm action quan trọng với vibration |
| **Focus Order** | Tab order logic, không nhảy bừa |
| **Skip to Main Content** | Shortcut cho screen reader users |

---

## 15.8 Responsive Layout Strategy

```dart
// Breakpoints
const mobileBreakpoint = 600;
const tabletBreakpoint = 900;

// Layout builder
class ResponsiveLayout extends StatelessWidget {
  final Widget mobile;
  final Widget? tablet;
  final Widget? desktop;

  @override
  Widget build(BuildContext context) {
    final width = MediaQuery.of(context).size.width;
    if (width >= tabletBreakpoint && desktop != null) return desktop!;
    if (width >= mobileBreakpoint && tablet != null) return tablet!;
    return mobile;
  }
}

// Example usage: Dashboard có thể 2 cột trên tablet
ResponsiveLayout(
  mobile: SingleColumnDashboard(),
  tablet: TwoColumnDashboard(),
)
```

---

## 15.9 Animation & Transitions

| Transition | Duration | Curve |
|---|---|---|
| Screen push/pop | 300ms | easeInOut |
| Card expand | 250ms | fastOutSlowIn |
| SOS countdown | 1000ms | linear |
| Loading spinner | Continuous | linear |
| Success checkmark | 600ms | elasticOut |
| Error shake | 400ms | easeInOut |

```dart
// Countdown animation example
AnimatedBuilder(
  animation: _controller,
  builder: (context, child) {
    return Transform.scale(
      scale: 1 + (_controller.value * 0.2),
      child: Text(
        '${(_controller.value * 5).ceil()}',
        style: Theme.of(context).textTheme.displayLarge,
      ),
    );
  },
)
```

---

## 15.10 Dark Mode Strategy

```dart
// Auto switch theo system
ThemeMode themeMode = ThemeMode.system;

// Manual override trong Settings
ThemeData lightTheme = ThemeData(
  useMaterial3: true,
  colorScheme: geriatricCareColorScheme,
  textTheme: buildTextTheme(fontScale),
);

ThemeData darkTheme = ThemeData(
  useMaterial3: true,
  colorScheme: geriatricCareDarkColorScheme,
  textTheme: buildTextTheme(fontScale),
);

MaterialApp(
  theme: lightTheme,
  darkTheme: darkTheme,
  themeMode: themeMode,
  ...
);
```

**Dark Mode color adjustments:**
- SOS red slightly lighter (tăng L trong HSL)
- Contrast vẫn đảm bảo ≥ 4.5:1
- Surface elevation dùng tint thay shadow

---

## 15.11 Localization (i18n)

```dart
// Hỗ trợ Tiếng Việt + Tiếng Anh
supportedLocales: [
  Locale('vi', 'VN'),
  Locale('en', 'US'),
],

// Strings extracted trong l10n/
class AppLocalizations {
  static String sosButton(BuildContext context) {
    return Localizations.of<AppLocalizations>(context)!.sosButton;
  }
}

// vi.arb
{
  "sosButton": "GỌI CỨU GIÚP",
  "medicationReminder": "Đến giờ uống thuốc",
  "healthAlertHigh": "Chỉ số cao bất thường"
}

// en.arb
{
  "sosButton": "CALL FOR HELP",
  "medicationReminder": "Time to take medication",
  "healthAlertHigh": "Abnormally high reading"
}
```

---

## 15.12 UI Testing Strategy

```dart
// Widget test – SosButton
testWidgets('SosButton triggers callback on tap', (tester) async {
  bool wasPressed = false;
  await tester.pumpWidget(
    MaterialApp(
      home: Scaffold(
        body: SosButton(onPressed: () => wasPressed = true),
      ),
    ),
  );

  await tester.tap(find.byType(SosButton));
  await tester.pumpAndSettle();

  expect(wasPressed, isTrue);
});

// Golden test – Visual regression
testWidgets('ElderlyHomeScreen golden test', (tester) async {
  await tester.pumpWidget(
    ProviderScope(
      child: MaterialApp(home: ElderlyHomeScreen()),
    ),
  );
  await expectLater(
    find.byType(ElderlyHomeScreen),
    matchesGoldenFile('goldens/elderly_home_screen.png'),
  );
});
```
