# Strictly Prohibited Anti-Patterns

This document outlines prohibited practices that must be strictly avoided. Writing any of these anti-patterns in the codebase will fail validation.

## 1. Direct Network Calls in View or ViewModel
- **Anti-Pattern**: Direct instantiation of `Dio`, `Http`, or invocation of `WebAPIService` within widget classes or ViewModels.
- **Why**: Violates separation of concerns. All network operations must go through repositories to keep views and business state separated from network implementation details.
- **Correction**: Wrap network operations in a dedicated Repository class under `lib/features/<feature>/repo/`.

## 2. Hardcoded Typography Styles and Hexadecimal Colors
- **Anti-Pattern**: Defining inline `TextStyle(color: Color(0xFF1A2B3C), fontSize: 16)` or using raw hex codes in widgets.
- **Why**: Breaks dark/light mode switching and the central design system.
- **Correction**: Use base text styles from `lib/utils/styles.dart` and retrieve colors via `context.brand` or `context.palette`. Always append `.spMin` to the font size for responsiveness.

## 3. Inline Hardcoded String Literals
- **Anti-Pattern**: Rendering UI text labels using inline Strings: `const Text("Submit Form")`.
- **Why**: Prevents localization support.
- **Correction**: Always add keys in standard camelCase to the `.arb` files and consume them via the localization helper: `Localization.locale.submitForm`.

## 4. Passing ViewModels in Widget Constructors
- **Anti-Pattern**: Instantiating or passing `ViewModel` instances inside custom child widget constructor parameters: `MyChildWidget(viewModel: myViewModel)`.
- **Why**: Causes unnecessary rebuilds, couples components, and bypasses Provider's dependency injection context.
- **Correction**: Use standard Provider accessors inside the child widget: `context.read<MyViewModel>()`, `context.watch<MyViewModel>()`, or wrap specific sections in a `Consumer` or `Selector`.

## 5. Bypassing Base Provider Classes
- **Anti-Pattern**: Extending ViewModels from `ChangeNotifier` directly instead of the project's base `ViewModel` class.
- **Why**: Bypasses shared logic for pagination, unified error handling, and loading state management.
- **Correction**: Ensure all feature ViewModel classes extend `ViewModel` from `lib/providers/view_model.dart`.

## 6. Duplicate or Redundant Utility Functions
- **Anti-Pattern**: Implementing custom formatters (e.g. currency, date), progress spinners, or dialog popups from scratch.
- **Why**: Inflates bundle sizes and leads to design inconsistencies.
- **Correction**: Always check `lib/utils/extensions.dart` and `lib/utils/app_build_methods.dart` for reusable extensions (e.g., `.showCircleProgressOnCenter`, `.setProgress`, `.toFormatted`, `showToast`) before writing custom helpers.
