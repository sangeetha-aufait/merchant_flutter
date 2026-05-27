# Strict Development Rules

These rules represent the absolute constraints of this project. Both developers and AI-agents must strictly follow them without exceptions.

## 1. Development & Version Command Rules
- **Rule**: All commands must use `FVM` (Flutter Version Management).
  - Use `fvm flutter ...` instead of `flutter ...`.
  - Use `fvm dart ...` instead of `dart ...`.

## 2. API Integration Constraints
- **Rule**: Always use the Repository + Model approach. Never invoke Dio/Http directly in the ViewModel or View.
- **Rule**: Run `build_runner` with `--delete-conflicting-outputs` to generate serialization code (`.g.dart` files).
- **Rule**: Do not assume API contracts. If headers, methods, or request/response structures are missing, stop and request them immediately.

## 3. UI and Sizing Rules
- **Rule**: Sizing must use `ScreenUtil` responsive units.
  - **CRITICAL**: You must strictly use **only** the `.spMin` extension for all sizes, heights, widths, margins, paddings, border radiuses, and font sizes (e.g. `24.spMin`, `EdgeInsets.symmetric(horizontal: 24.spMin)`). Do **not** use other ScreenUtil extensions like `.w`, `.h`, `.r`, or `.sp`.
  - Sizing must match the Figma design spec design size: `const Size(428, 926)` (configured in `main.dart`).
- **Rule**: Use the predefined styling classes inside `lib/utils/styles.dart` (e.g. `tsS14W400`). Do not write ad-hoc `TextStyle` declarations on text widgets.
- **Rule**: Use the predefined color values inside `lib/utils/brand_colors.dart` (via `context.brand`) or `lib/utils/app_palette.dart` (via `context.palette`). Avoid using raw hardcoded hexadecimal color representations (e.g. `Color(0xFF...)`).

## 4. Routing and Navigation
- **Rule**: Always implement routing using **Named Routes** registered in `lib/utils/routes.dart`.
- **Rule**: If a destination view requires arguments, define a strongly-typed `Params` class inside that destination screen file (e.g. `ProfileScreenParams`).
- **Rule**: Retrieve arguments using the `context.getScreenParamsOf<Params>()` extension method rather than raw `ModalRoute` casting inside views.

## 5. Component Reuse & Zero-Duplication
- **Rule**: Before writing a new UI component, search the `lib/widgets` directory to see if a matching component (e.g., `AppButton`, `CustomTextField`) already exists.
- **Rule**: Do not duplicate core business flows or utilities. Always check `lib/utils/extensions.dart` and `lib/utils/app_build_methods.dart` for helpers first.

## 6. Localization
- **Rule**: Localization must be strictly English-first. Only English (`app_en.arb`) must be created by default and is mandatory for all projects. No additional localization files should be generated unless explicitly specified. Japanese (`app_ja.arb`) is optional and should be added only when specifically requested or required. If any other language support is needed, it must be explicitly specified before implementation. No unspecified language localization should be created.
- **Rule**: Never use hardcoded String literals in presentation widgets. All copy must be localized using ARB files under `lib/l10n/` and these localised strings are to be used.
- **Rule**: Keys inside `.arb` files must follow `lowerCamelCase` formatting.
- **Rule**: After modifying an `.arb` file, run `fvm flutter gen-l10n`.

## 7. Security and Credentials
- **Rule**: Never commit credentials, passwords, private keys, or system-sensitive endpoints to source control.
- **Rule**: Store configurations in the `.env` file and instruct the development environment on the required environment keywords.
