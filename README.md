# Note App 📝☁️

<p align="center">
  <strong>A Cloud-Synchronized Rich Text Notes Application built with Flutter, Supabase & Flutter Quill.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white" alt="Flutter" />
  <img src="https://img.shields.io/badge/Dart-0175C2?style=for-the-badge&logo=dart&logoColor=white" alt="Dart" />
  <img src="https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white" alt="Supabase" />
  <img src="https://img.shields.io/badge/Clean%20Architecture-4CAF50?style=for-the-badge" alt="Clean Architecture" />
  <img src="https://img.shields.io/badge/State%20Management-Bloc-blueviolet?style=for-the-badge" alt="Bloc" />
  <img src="https://img.shields.io/badge/Rich%20Text-Flutter%20Quill-FF6F00?style=for-the-badge" alt="Flutter Quill" />
</p>

---

## 📖 Overview

**Note App** is a modern, cross-platform note-taking application designed for effortless productivity. Combining the power of **Flutter**, **Supabase Backend-as-a-Service (BaaS)**, and **Flutter Quill**, users can draft, format, organize, and synchronize rich-text notes seamlessly across devices.

The project is structured according to **Clean Architecture** principles and leverages the **BLoC pattern** for robust, testable, and reactive state management.

---

## ✨ Features

- **✍️ Rich Text Formatting**: Full WYSIWYG rich text editor powered by `flutter_quill` supporting bold, italic, underline, strike-through, headings, bulleted & numbered lists, text styling, and custom toolbar controls.
- **☁️ Supabase Cloud Backend**: Real-time cloud persistence and synchronized data access.
- **🔐 Comprehensive Authentication**:
  - Email & Password registration and login.
  - Forgot password / reset password workflow.
  - **Google Sign-In** OAuth integration.
  - Authentication state monitoring and route guards (`authMiddleware`).
- **🔍 Instant Note Search**: Quick note search functionality using a tailored `NoteSearchDelegate`.
- **🗂️ Note Management (CRUD)**: Create, view, update, and delete notes with smooth transition animations.
- **🎨 Polished UI & Typography**: Custom **Nunito** font family across various weights, custom animated dialogs, and styled Snackbars.
- **🛣️ Declarative Routing**: Route transitions, slide animations, and deep-link handling via `go_router`.
- **🧩 Dependency Injection**: Centralized service locator with `get_it`.

---

## 🏗️ Architecture

```
note_app/
├── assets/
│   ├── auth/svg/            # Authentication illustrations
│   ├── fonts/Nunito/        # Nunito typography font family
│   └── notes/png/           # Note empty states and graphics
└── lib/
    ├── core/                # Shared utilities and configurations
    │   ├── failure/         # Custom failure definitions
    │   ├── middlewares/     # Route authentication middleware
    │   ├── quill/           # Custom Quill toolbar configurations and styles
    │   ├── result/          # Result pattern wrappers
    │   ├── router/          # GoRouter setup with custom page transitions
    │   ├── supabase/        # Supabase client initialization
    │   ├── theme/           # AppTheme & ThemeData
    │   ├── usecase/         # Base UseCase contract
    │   ├── utils/           # Custom dialogs and snackbar utilities
    │   └── widgets/         # Shared widgets
    ├── features/
    │   ├── authentication/  # Auth module (Sign In, Sign Up, Forgot Password)
    │   │   ├── data/        # Remote data source, models, repository impl
    │   │   ├── domain/      # Auth entity, repository contract, use cases
    │   │   └── presentation/# AuthBloc, authentication screens, and form fields
    │   └── note/            # Note management module
    │       ├── data/        # Note remote data source, model, repository impl
    │       ├── domain/      # Note entity, repository contract, use cases
    │       └── presentation/# NoteBloc, NoteDetailsBloc, screens, and search
    ├── injection_container.dart # Service locator registration (GetIt)
    └── main.dart            # Main entrypoint initializing Supabase & App
```

---

## 🛠️ Tech Stack & Dependencies

| Category | Technology / Package |
|---|---|
| **Framework** | Flutter (SDK ^3.9.0) |
| **Backend / DB** | Supabase (`supabase_flutter: ^2.10.0`) |
| **Rich Text Editor** | `flutter_quill: ^11.4.2` |
| **Authentication** | Supabase Auth + `google_sign_in: ^7.1.1` |
| **State Management** | `flutter_bloc: ^9.1.1`, `equatable: ^2.0.7` |
| **Navigation** | `go_router: ^16.2.1` |
| **Dependency Injection** | `get_it: ^8.2.0` |
| **Vectors & UI** | `flutter_svg: ^2.2.1`, `cupertino_icons: ^1.0.8` |

---

## 🗄️ Supabase Database Schema

To set up your own Supabase backend, execute the following SQL in your Supabase SQL Editor:

```sql
-- Create Notes Table
CREATE TABLE notes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE NOT NULL,
    title TEXT NOT NULL,
    content JSONB NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- Enable Row Level Security (RLS)
ALTER TABLE notes ENABLE ROW LEVEL SECURITY;

-- Create policy for user-scoped access
CREATE POLICY "Users can manage their own notes"
ON notes
FOR ALL
USING (auth.uid() = user_id)
WITH CHECK (auth.uid() = user_id);
```

---

## 🚀 Getting Started

### Prerequisites

- [Flutter SDK](https://docs.flutter.dev/get-started/install) (`>= 3.9.0`)
- A [Supabase](https://supabase.com/) project

### Setup & Run

1. **Clone the repository:**
   ```bash
   git clone https://github.com/floskam300-eng/note_app.git
   cd note_app
   ```

2. **Configure Supabase credentials:**
   Update your Supabase URL and Anon Key in `lib/core/supabase/supabase.dart`:
   ```dart
   await supabase.Supabase.initialize(
     url: "YOUR_SUPABASE_PROJECT_URL",
     anonKey: "YOUR_SUPABASE_ANON_KEY",
   );
   ```

3. **Install dependencies:**
   ```bash
   flutter pub get
   ```

4. **Run the app:**
   ```bash
   flutter run
   ```

---

## 👤 Author

- **Omar Shamy** - [GitHub Profile](https://github.com/floskam300-eng)
