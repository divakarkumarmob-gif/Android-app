# DK App — Android Project

Vite + React (TypeScript) app converted to Android using native WebView.

---

## 📁 Project Structure

```
dk-app-android/
├── src/                        # React/TypeScript source
├── android/                    # Android native project
│   ├── app/
│   │   ├── src/main/
│   │   │   ├── java/com/dkapp/android/
│   │   │   │   ├── MainActivity.kt     # WebView host
│   │   │   │   └── SplashActivity.kt   # Splash screen
│   │   │   ├── assets/public/          # Bundled web app goes here
│   │   │   ├── res/                    # Icons, layouts, colors
│   │   │   └── AndroidManifest.xml
│   │   └── build.gradle.kts
│   ├── gradle/wrapper/
│   ├── build.gradle.kts
│   ├── settings.gradle.kts
│   └── gradlew
├── .github/workflows/build-apk.yml    # GitHub Actions CI
├── package.json
├── vite.config.ts
└── index.html
```

---

## 🚀 Quick Start (Local Build)

### Prerequisites
- Node.js 18+ & npm
- Android Studio or Android SDK (API 35)
- Java 21 (JDK)

### Step 1 — Build the Web App

```bash
# Install dependencies
npm install

# Set your Gemini API key
cp .env.example .env
# Edit .env → GEMINI_API_KEY=your_key_here

# Build the Vite app
npm run build
```

### Step 2 — Copy Web Assets to Android

```bash
mkdir -p android/app/src/main/assets/public
cp -r dist/* android/app/src/main/assets/public/
```

### Step 3 — Setup Android SDK Path

```bash
cd android
cp local.properties.example local.properties
# Edit local.properties → sdk.dir=/path/to/your/Android/Sdk
```

### Step 4 — Build the APK

```bash
cd android
chmod +x gradlew

# Debug APK (for testing)
./gradlew assembleDebug

# APK location:
# android/app/build/outputs/apk/debug/app-debug.apk
```

### One-liner (after setup)

```bash
npm run android:debug
```

---

## 🤖 GitHub Actions (Automatic Build)

Push to `main` or `master` and the workflow auto-builds your APK.

### Setup Steps:

1. Push this project to GitHub
2. Go to **Settings → Secrets and variables → Actions**
3. Add secret: `GEMINI_API_KEY` = your Gemini API key
4. Push a commit — CI will build and upload the APK as an artifact

**Download APK:**
- Go to **Actions** tab → latest workflow run → **Artifacts** section → Download `app-debug-apk`

---

## 🔧 Configuration

### Change App Name
Edit `android/app/src/main/res/values/strings.xml`:
```xml
<string name="app_name">Your App Name</string>
```

### Change App Package ID
Edit `android/app/build.gradle.kts`:
```kotlin
applicationId = "com.yourcompany.yourapp"
```
And rename folder: `java/com/dkapp/android/` → `java/com/yourcompany/yourapp/`

### Change App Icon
Replace files in `android/app/src/main/res/mipmap-*/`:
- `ic_launcher.png` — Square icon
- `ic_launcher_round.png` — Round icon

Sizes: mdpi=48px, hdpi=72px, xhdpi=96px, xxhdpi=144px, xxxhdpi=192px

---

## 🐛 Common Errors & Fixes

### ❌ Kotlin Duplicate Class Error
Already fixed via `build.gradle.kts`:
```kotlin
configurations.all {
    resolutionStrategy {
        force("org.jetbrains.kotlin:kotlin-stdlib:1.9.24")
    }
}
```

### ❌ `sdk.dir` not set
```bash
echo "sdk.dir=/home/$USER/Android/Sdk" > android/local.properties
```

### ❌ Java version mismatch
Set `JAVA_HOME` to JDK 21:
```bash
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
```

### ❌ Gradle permission denied
```bash
chmod +x android/gradlew
```

### ❌ Web assets not loading
Make sure you ran `npm run build` and copied files:
```bash
cp -r dist/* android/app/src/main/assets/public/
```

---

## 📱 Features

- ✅ Full WebView hosting React app
- ✅ Kotlin (Java 21 compatible)
- ✅ Splash screen with app branding
- ✅ App icon (all densities)
- ✅ Camera & Microphone permissions (for AI Voice)
- ✅ Back button navigation
- ✅ JavaScript ↔ Android bridge (`window.AndroidBridge`)
- ✅ Dark status/nav bar
- ✅ Kotlin stdlib duplicate class fix
- ✅ GitHub Actions CI/CD

---

## 🏗️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 19 + TypeScript + Vite |
| Styling | Tailwind CSS v4 |
| State | Zustand |
| AI | Google Gemini API |
| Android | Native WebView (Kotlin) |
| Build | Gradle 8.7 + AGP 8.5 |
| CI/CD | GitHub Actions |
