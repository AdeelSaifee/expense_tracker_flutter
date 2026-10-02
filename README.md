# Expense Tracker Flutter

A mobile application built with Flutter and Dart for managing and visualizing personal expenses. The app allows users to add, categorize, track, and delete expenses while observing real-time dynamic bar chart breakdowns, comprehensive light/dark theme adaptations, and a **fully responsive & adaptive user interface** tailored across device orientations (Portrait & Landscape) and platforms (Android & iOS).

Targeted and optimized strictly for **Android** and **iOS**.

---

## Features

- **Expense Management**: Add new expenses with title, numeric amount, transaction date, and categorized labels (`Food`, `Travel`, `Leisure`, `Work`).
- **Responsive Layout (Portrait & Landscape)**:
  - **Portrait Mode**: Vertically stacked `Column` with the spending chart above the expense list.
  - **Landscape Mode**: Automatically transforms into a side-by-side `Row` layout (`width >= 600`) where the chart occupies the left half and the list occupies the right half.
- **Adaptive Modal Form (`LayoutBuilder`)**:
  - Dynamically rearranges form inputs based on available horizontal width.
  - Places **Title & Amount** and **Category & Date Picker** side-by-side in landscape mode to minimize vertical scrolling.
- **Keyboard-Aware Overlays (`viewInsets`)**:
  - Automatically calculates on-screen virtual keyboard height using `MediaQuery.of(context).viewInsets.bottom`.
  - Dynamically adds bottom padding (`keyboardSpace + 16`) and provides smooth scrolling via `SingleChildScrollView` to prevent keyboard obstruction.
- **Hardware Notch & Safe Area Protection**:
  - Configures `useSafeArea: true` on `showModalBottomSheet` to prevent content from overlapping physical device cutouts (camera notches, punch-holes, dynamic island, status bars).
- **Adaptive Platform Dialogs (Android & iOS)**:
  - Detects native operating system via `Platform.isIOS` from `dart:io`.
  - Displays native **`CupertinoAlertDialog`** on Apple devices and **Material `AlertDialog`** on Android for authentic platform feel.
- **Form Validation & Alert Feedback**: Input sanitation using string trimming, `double.tryParse` validation, and native platform alert feedback for invalid submissions.
- **Date Picker Integration**: Native platform calendar dialog using asynchronous `Future` resolution with `async`/`await`.
- **Swipe-to-Delete (Dismissible)**: Smooth swipe gesture removal with unique `ValueKey` tracking and automatic UI-to-data synchronization.
- **Undo Action via SnackBar**: Non-blocking `SnackBar` notifications with an instant "Undo" callback that restores deleted items at their exact previous index.
- **Dynamic Bar Chart**: Category-based expense aggregation displaying proportional expenditure bars calculated with `FractionallySizedBox` and custom layout math.
- **Centralized Theming & Dark Mode**: Cohesive color systems generated with `ColorScheme.fromSeed` supporting automated system-level dark and light mode transitions.

---

## Key Learnings & Flutter Concepts Mastered

Throughout this project, several critical Flutter and Dart concepts were studied, practiced, and integrated into production-ready code:

### 1. Responsive UI: Screen vs Component Constraints
- **`MediaQuery` vs `LayoutBuilder`**:
  - Used `MediaQuery.of(context).size.width` for macro-level responsive switching on the main `Expenses` screen (Breakpoint: `600px`).
  - Used `LayoutBuilder` with `constraints.maxWidth` for modular, component-level responsive restructuring of form rows in `NewExpense`.
- **Flutter Layout Protocol ("Constraints go down, sizes go up, parent sets position")**:
  - Mastered why unconstrained widgets inside unconstrained parents cause crashes (e.g. `Container(width: double.infinity)` inside `Row`).
  - Applied `Expanded` wrappers to enforce bounded constraints and allocate equal proportional space across horizontal rows.

### 2. Handling System Overlays & Hardware Cutouts
- **Software Keyboard Management (`viewInsets`)**:
  - Extracted real-time keyboard heights using `MediaQuery.of(context).viewInsets.bottom`.
  - Applied dynamic bottom padding (`EdgeInsets.fromLTRB(16, 16, 16, keyboardSpace + 16)`) combined with `SingleChildScrollView` and `SizedBox(height: double.infinity)` to maintain full-screen sheet height while keeping every input accessible.
- **Hardware Safe Areas**:
  - Leveraged `showModalBottomSheet(useSafeArea: true)` to avoid hardcoded padding guesses, ensuring flawless placement beneath camera notches, dynamic islands, and status bars across diverse hardware.

### 3. Adaptive Architecture & Native Design Languages
- **Platform Detection (`dart:io`)**:
  - Inspected runtime OS using `Platform.isIOS` and `Platform.isAndroid`.
- **Material vs Cupertino Integration**:
  - Rendered `showCupertinoDialog` with `CupertinoAlertDialog` on iOS and `showDialog` with `AlertDialog` on Android.
- **Device Orientation Locking (SystemChrome)**:
  - Explored `SystemChrome.setPreferredOrientations` alongside `WidgetsFlutterBinding.ensureInitialized()` to control device orientation when locked orientations are required.

### 4. Form Handling & Controller Lifecycle
- **`TextEditingController`**: Managed persistent user input across multiple fields without unnecessary UI rebuilds.
- **Memory Leak Prevention (`dispose`)**: Overrode `dispose()` on controllers to release memory when widgets leave the widget tree.
- **Input Sanitization**: Implemented robust validation combining `.trim()`, `.isEmpty`, `double.tryParse()`, and short-circuit boolean logic.

### 5. High-Performance Lists & Gestures
- **`ListView.builder`**: Utilized lazy loading to render lists on-demand for optimized memory usage and fluid scrolling.
- **`Dismissible` & `ValueKey`**: Handled swipe-to-delete gestures while maintaining tight synchronization between the on-screen widget tree and internal Dart lists using explicit keys.
- **`ScaffoldMessenger` & Undo Restorations**: Implemented dismissible `SnackBar` banners with `persist: false` and targeted list index restoration using `List.insert(index, element)`.

### 6. Advanced Theming & Dark Mode
- **`ColorScheme.fromSeed`**: Generated complete, harmonious Material 3 color palettes from a single root seed color for both light and dark modes.
- **Granular Sub-theming**: Styled components uniformly across the app by configuring `appBarTheme`, `cardTheme` (`CardThemeData`), `elevatedButtonTheme`, and `textTheme`.
- **Consuming Theme Tokens**: Accessed global themes directly in child widgets using `Theme.of(context)` for consistent typography and dynamic error tinting.

---

## Technical Architecture & Design Decisions

### 1. Responsive & Adaptive Strategy
- **Breakpoint Rule**: `600px` threshold determines whether the layout is rendered vertically (Column) for handheld portrait phones, or horizontally (Row) for landscape devices and tablets.
- **Component Independence**: Form inputs inside `NewExpense` rely strictly on `LayoutBuilder` parent constraints, enabling the modal to be embedded anywhere in the widget tree without coupling to device dimensions.
- **Native OS Fidelity**: Critical dialogs branch conditionally to provide native Cupertino aesthetics on iOS and Material Design 3 on Android.

### 2. State & Lifecycle Management
- **Lifting State Up**: State is managed in parent components (`Expenses`) and delegated to pure presentation and form inputs via typed callbacks (`void Function(Expense)`).
- **Resource Cleanup**: Explicit `dispose()` lifecycle hooks implemented on all `TextEditingController` instances to prevent memory leaks during frequent modal transitions.

### 3. Theming & Design System
- **`ColorScheme.fromSeed`**: Single source of truth for both light (`Color(0xFF603BB5)`) and dark (`Color(0xFF05637D)`) palettes.
- **Sub-theme Overrides**: Dedicated configuration for `AppBarTheme`, `CardThemeData`, `ElevatedButtonThemeData`, and `TextTheme` ensuring UI consistency without redundant per-widget styling.
- **Platform Brightness Detection**: Responsive color switching driven by `MediaQuery.of(context).platformBrightness`.

---

## Project Structure

```text
lib/
|-- main.dart                           # Application entry point & theme configurations
|-- models/
|   `-- expense.dart                    # Expense & ExpenseBucket data models, Category enum
`-- widgets/
    |-- expenses.dart                   # Main dashboard with responsive MediaQuery (Column <-> Row)
    |-- new_expense.dart                # Adaptive form with LayoutBuilder, viewInsets & Cupertino dialog
    |-- chart/
    |   |-- chart.dart                  # Category breakdown chart container (Expanded in Row)
    |   `-- chart_bar.dart              # Proportional vertical bar representation
    `-- expenses_list/
        |-- expenses_list.dart          # ListView.builder with Dismissible wrappers
        `-- expense_item.dart           # Formatted expense card item
```

---

## Getting Started

### Prerequisites
- Flutter SDK (v3.16.0 or higher recommended)
- Android Studio / Xcode for emulators or physical device deployment

### Dependencies
Defined in `pubspec.yaml`:
- `intl`: Locale formatting for currency and dates
- `uuid`: Universally Unique Identifier generation

### Running the App
1. Clone the repository:
   ```bash
   git clone https://github.com/AdeelSaifee/expense_tracker_flutter.git
   cd expense_tracker_flutter
   ```

2. Fetch dependencies:
   ```bash
   flutter pub get
   ```

3. Run static code analysis:
   ```bash
   flutter analyze
   ```

4. Launch the application:
   ```bash
   flutter run
   ```
