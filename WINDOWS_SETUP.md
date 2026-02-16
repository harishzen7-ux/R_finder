# R Finder - Windows Setup Guide

## 🪟 Windows-Specific Configuration

This guide covers setup specifically for Windows systems.

---

## Step 1: Install Flutter

### 1.1 Download Flutter
1. Visit [Flutter Downloads](https://flutter.dev/docs/get-started/install/windows)
2. Download latest stable Flutter SDK
3. Extract to a permanent location (e.g., `C:\flutter`)

### 1.2 Add Flutter to PATH
1. Open **Environment Variables**:
   - Press `Win + X` → **System**
   - Click **Advanced system settings**
   - Click **Environment Variables** button
   - Under **User variables**, click **New**

2. Add Flutter to PATH:
   - **Variable name**: `Flutter_Path` (optional, for clarity)
   - **Variable value**: `C:\flutter\bin` (adjust if different)
   - Click **OK**

3. Verify in PATH:
   - In **System variables**, find **Path**
   - Click **Edit**
   - Verify `C:\flutter\bin` is listed
   - If not, click **New** and add it

### 1.3 Verify Installation
```powershell
# Open PowerShell and verify
flutter --version

# Run doctor to check system
flutter doctor
```

---

## Step 2: Install Android Development Tools

### 2.1 Android Studio
1. Download from [Android Studio](https://developer.android.com/studio)
2. Run installer, follow default options
3. Launch Android Studio
4. Go to **File > Settings > Appearance & Behavior > System Settings > Android SDK**
5. Note the SDK Location (e.g., `C:\Users\username\AppData\Local\Android\sdk`)

### 2.2 Install SDK Components
In Android Studio:
1. Go to **SDK Manager**
2. Install:
   - **SDK Platforms**: Android 14.0 (API 34), Android 13 (API 33), Android 12 (API 31)
   - **SDK Tools**: 
     - Android SDK Build-Tools 35.0.0+
     - Android SDK Platform-Tools
     - Android SDK Tools

### 2.3 Add Android SDK to PATH
1. Open Environment Variables (see Step 1.2)
2. Add new **User variable**:
   - **Variable name**: `ANDROID_SDK_ROOT`
   - **Variable value**: `C:\Users\username\AppData\Local\Android\sdk`

3. Add to **Path**:
   - Add `%ANDROID_SDK_ROOT%\tools`
   - Add `%ANDROID_SDK_ROOT%\tools\bin`
   - Add `%ANDROID_SDK_ROOT%\platform-tools`
   - Add `%JAVA_HOME%\bin` (for Java)

### 2.4 Accept Licenses
```powershell
flutter doctor --android-licenses
# Type 'y' to accept all licenses
```

### 2.5 Verify Setup
```powershell
flutter doctor

# You should see:
# ✓ Android toolchain - develop for Android devices
```

---

## Step 3: Install Git

1. Download from [Git for Windows](https://git-scm.com/download/win)
2. Run installer with default options
3. Verify:
```powershell
git --version
```

---

## Step 4: Configure Flutter for the Project

### 4.1 Navigate to Project
```powershell
cd "C:\Users\haris\OneDrive\Desktop\R Finder"
```

### 4.2 Activate FlutterFire
```powershell
# Option 1: Using dart
dart pub global activate flutterfire_cli

# Option 2: Using flutter
flutter pub global activate flutterfire_cli
```

If that doesn't work, try:
```powershell
# Add pub to PATH first
$env:PATH += ";$HOME\.pub-cache\bin"

# Then try activating again
dart pub global activate flutterfire_cli
```

### 4.3 Verify FlutterFire
```powershell
flutterfire --version
```

---

## Step 5: Install Project Dependencies

```powershell
cd "C:\Users\haris\OneDrive\Desktop\R Finder"
flutter pub get
```

---

## Step 6: Run Flutter Doctor

```powershell
flutter doctor -v
```

**Expected Output** (with ✓ marks):
```
[✓] Flutter (Channel stable, version 3.X.X)
[✓] Android toolchain - develop for Android devices
[✓] Android Studio (version 4.X or higher)
[✓] VS Code
```

---

## Step 7: Android Emulator Setup

### 7.1 Create Virtual Device
1. Open Android Studio
2. **Tools > Device Manager**
3. Click **Create Device**
4. Select device (e.g., Pixel 5)
5. Select Android version (API 30+)
6. Click **Finish**

### 7.2 Launch Emulator
```powershell
# List available emulators
flutter emulators

# Launch specific emulator
flutter emulators --launch <emulator-name>

# Or use Android Studio: Tools > Device Manager > Play button
```

### 7.3 Verify Device
```powershell
flutter devices
```

---

## Step 8: Configure Firebase

### 8.1 Install Firebase CLI
```powershell
# Using npm (if you have Node.js installed)
npm install -g firebase-tools

# Or download from: https://firebase.google.com/docs/cli#direct-download
```

### 8.2 Login to Firebase
```powershell
firebase login
```

This opens a browser window. Log in with your Google account.

### 8.3 Configure FlutterFire Project
```powershell
cd "C:\Users\haris\OneDrive\Desktop\R Finder"
flutterfire configure

# When prompted:
# 1. Select your Firebase project
# 2. Platforms: Select Android, iOS (or just Android if on Windows-only)
# 3. Review defaults and confirm
```

---

## Step 9: Download Firebase Configuration Files

### 9.1 Download Android Config
1. Go to [Firebase Console](https://console.firebase.google.com)
2. Select your project
3. Go to **Project Settings** (gear icon)
4. Go to **"Your apps"** section
5. Click your Android app
6. Click **google-services.json** to download
7. Place in: `android/app/google-services.json`

### 9.2 Download iOS Config (if needed)
Similarly, download `GoogleService-Info.plist` and place in `ios/Runner/`

---

## Step 10: Run the App

### 10.1 Start Emulator
```powershell
flutter emulators --launch <emulator-name>
```

### 10.2 Run App
```powershell
cd "C:\Users\haris\OneDrive\Desktop\R Finder"
flutter run
```

Or with verbose logging:
```powershell
flutter run -v
```

---

## Troubleshooting

### Issue: "Flutter command not found"
**Solution**:
```powershell
# Restart PowerShell or Command Prompt
# Or manually add to PATH:
$env:PATH += ";C:\flutter\bin"
```

### Issue: "java.exe not found"
**Solution**:
1. Install Java JDK 11+: [Java Downloads](https://www.oracle.com/java/technologies/downloads/)
2. Set JAVA_HOME environment variable
3. Add to PATH: `%JAVA_HOME%\bin`

### Issue: "Android SDK not found"
**Solution**:
1. Launch Android Studio
2. Go to **SDK Manager**
3. Install Android SDK (API 31+)
4. Set `ANDROID_SDK_ROOT` environment variable

### Issue: "Gradle build failed"
**Solution**:
```powershell
cd android
./gradlew wrapper --gradle-version 7.5
cd ..
flutter clean
flutter pub get
flutter run
```

### Issue: "FlutterFire not found"
**Solution**:
```powershell
# Add pub to PATH
$env:PATH += ";$HOME\.pub-cache\bin"

# Reactivate
dart pub global activate flutterfire_cli
```

### Issue: "Emulator won't start"
**Solution**:
1. Ensure **Virtualization** is enabled in BIOS
2. Disable Hyper-V if you want to use Android native emulator
3. Or use [Android Emulator Accelerator](https://github.com/google/android-emulator-hypervisor-driver-for-amd-processors)

---

## Performance Optimization

### Memory & CPU
- Close unnecessary applications
- Allocate sufficient RAM to emulator (4GB+ recommended)
- Don't run multiple emulators simultaneously

### Build Speed
```powershell
# Enable incremental builds
flutter config --enable-android-embedding-v2

# Use release mode for testing (slower but more realistic)
flutter run --release
```

---

## Next Steps

After completing this setup:

1. ✅ Run `flutter doctor` - should show all ✓
2. ✅ Run the app: `flutter run`
3. ✅ Create Firebase project and configure
4. ✅ Test all features locally
5. ✅ Build APK for Android: `flutter build apk --release`
6. ✅ Deploy to Google Play Store

---

## Useful Windows PowerShell Commands

```powershell
# Check Flutter version
flutter --version

# Check system compatibility
flutter doctor -v

# Clear cache
flutter clean

# Get dependencies
flutter pub get

# Build APK
flutter build apk --release

# List connected devices
flutter devices

# Run with verbose output
flutter run -v
```

---

## Additional Resources

- **Flutter Windows Setup**: https://flutter.dev/docs/get-started/install/windows
- **Android Studio Setup**: https://developer.android.com/studio/install
- **Firebase for Flutter**: https://firebase.flutter.dev/docs/overview
- **Stack Overflow**: Tag `flutter-setup` for Windows-specific issues

---

**Status**: Windows Setup Ready  
**Last Updated**: February 16, 2025
