# Expense Tracker Flutter

A mobile application built with Flutter and Dart for managing and visualizing personal expenses. The app allows users to add, categorize, track, and delete expenses while observing real-time dynamic bar chart breakdowns and comprehensive light/dark theme adaptations.

Targeted and optimized strictly for **Android** and **iOS**.

---

## Features

- **Expense Management**: Add new expenses with title, numeric amount, transaction date, and categorized labels (`Food`, `Travel`, `Leisure`, `Work`).
- **Interactive Modal Bottom Sheet**: Input form presented as a full-height bottom sheet configured to adapt dynamically against the virtual software keyboard.
- **Form Validation & Alert Dialogs**: Input sanitation using string trimming, `double.tryParse` validation, and native `AlertDialog` feedback for empty or invalid submissions.
- **Date Picker Integration**: Native platform calendar dialog using asynchronous `Future` resolution with `async`/`await`.
- **Swipe-to-Delete (Dismissible)**: Smooth swipe gesture removal with unique `ValueKey` tracking and automatic UI-to-data synchronization.
- **Undo Action via SnackBar**: Non-blocking `SnackBar` notifications with an instant "Undo" callback that restores deleted items at their exact previous index.
- **Dynamic Bar Chart**: Category-based expense aggregation displaying proportional expenditure bars calculated with `FractionallySizedBox` and custom layout math.
- **Centralized Theming & Dark Mode**: Cohesive color systems generated with `ColorScheme.fromSeed` supporting automated system-level dark and light mode transitions.

---

## Technical Architecture & Design Decisions

### 1. Data Models & Grouping
- **`Expense`**: Uses UUID (`uuid.v4()`) for unique item keys and Dart's `intl` package (`DateFormat.yMd()`) for locale-aware date formatting.
- **`ExpenseBucket`**: Implements custom named constructors (`ExpenseBucket.forCategory`) with collection `.where()` filters to partition total expenditure per category on demand.

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
    |-- expenses.dart                   # Main dashboard screen managing global expense state
    |-- new_expense.dart                # Form modal bottom sheet with validation logic
    |-- chart/
    |   |-- chart.dart                  # Category breakdown chart container
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
