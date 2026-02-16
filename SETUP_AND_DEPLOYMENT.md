# R Finder - Complete Setup & Deployment Guide

## 📋 Table of Contents
1. [Prerequisites](#prerequisites)
2. [Firebase Configuration](#firebase-configuration)
3. [Local Setup](#local-setup)
4. [Testing](#testing)
5. [Building & Deployment](#building--deployment)
6. [Troubleshooting](#troubleshooting)

---

## Prerequisites

### Required Tools
- **Flutter SDK** (>= 3.0.0) - [Download](https://flutter.dev/docs/get-started/install)
- **Android Studio** (for Android development)
- **Xcode** (for iOS development - macOS only)
- **Firebase CLI** - [Download](https://firebase.google.com/docs/cli)
- **Git** - [Download](https://git-scm.com/download)

### Verification
```bash
# Verify Flutter installation
flutter doctor

# Verify you're in the project root
cd "c:\Users\haris\OneDrive\Desktop\R Finder"
```

---

## Firebase Configuration

### Step 1: Create Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Create a project"** (or **"Add project"** if you have existing projects)
3. **Project name**: `r-finder` (or your preferred name)
4. Disable **Google Analytics** (optional, can enable later)
5. Click **"Create project"** and wait for initialization

### Step 2: Enable Required Services

#### 2.1 Authentication (Email/Password)
1. In Firebase Console, go to **Authentication** (left sidebar)
2. Click **"Get Started"**
3. Select **"Email/Password"** provider
4. Click **"Enable"** toggle
5. Click **"Save"**

#### 2.2 Cloud Firestore
1. Go to **Firestore Database** (left sidebar under Build)
2. Click **"Create Database"**
3. Choose region closest to your location
4. **Security Rules**: Select **"Start in production mode"**
5. Click **"Create"**
6. Once created, click **"Rules"** tab
7. Replace with security rules (see [Firestore Rules](#firestore-rules) below)

#### 2.3 Firebase Storage
1. Go to **Storage** (left sidebar under Build)
2. Click **"Get Started"**
3. Choose same region as Firestore
4. **Security Rules**: Select **"Start in production mode"**
5. Click **"Create"**
6. Once created, click **"Rules"** tab and update rules (see [Storage Rules](#storage-rules) below)

#### 2.4 Cloud Messaging (Optional - for notifications)
1. Go to **Cloud Messaging** (left sidebar)
2. Keep default settings
3. Can be configured later for push notifications

### Step 3: Configure FlutterFire

#### 3.1 Install FlutterFire CLI
```bash
# Activate FlutterFire CLI
dart pub global activate flutterfire_cli

# If this doesn't work, try:
flutter pub global activate flutterfire_cli
```

#### 3.2 Configure Your Project
```bash
# Navigate to project root
cd "c:\Users\haris\OneDrive\Desktop\R Finder"

# Run FlutterFire configuration
flutterfire configure

# When prompted:
# 1. Select your Firebase project
# 2. Select platforms: Android, iOS, Web (choose based on your needs)
# 3. Accept default settings for new apps
```

This will automatically generate `lib/firebase_options.dart` with your credentials.

### Step 4: Configure Firestore Security Rules

**File**: `lib/firebase_options.dart` is now auto-generated. Go to Firebase Console and update your Firestore Rules:

```firestore
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Allow read access for authenticated users
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == userId;
    }

    match /items/{itemId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update: if request.auth.uid == resource.data.postedBy;
      allow delete: if request.auth.uid == resource.data.postedBy;
    }

    match /claims/{claimId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update: if request.auth.uid == resource.data.madeBy || request.auth.uid == resource.data.claimingFor;
      allow delete: if request.auth.uid == resource.data.madeBy;
    }
  }
}
```

Steps:
1. Go to **Firestore > Rules** in Firebase Console
2. Clear existing rules
3. Paste the rules above
4. Click **"Publish"**

### Step 5: Configure Firebase Storage Rules

Go to **Storage > Rules** in Firebase Console and use:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    // Allow authenticated users to upload and read their own files
    match /users/{userId}/{allPaths=**} {
      allow read: if request.auth.uid == userId;
      allow write: if request.auth.uid == userId && request.resource.size < 10 * 1024 * 1024;
    }

    match /items/{itemId}/{allPaths=**} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.resource.size < 10 * 1024 * 1024;
    }
  }
}
```

Steps:
1. Go to **Storage > Rules**
2. Clear existing rules
3. Paste the rules above
4. Click **"Publish"**

---

## Local Setup

### Step 1: Install Dependencies
```bash
# Navigate to project directory
cd "c:\Users\haris\OneDrive\Desktop\R Finder"

# Install Flutter dependencies
flutter pub get
```

### Step 2: Verify Setup
```bash
# Run Flutter doctor to check for issues
flutter doctor -v

# Check if Android is properly configured
flutter doctor --android-licenses
```

### Step 3: (Android Only) Add Google Services Config
If you're building for Android:

1. Download `google-services.json` from Firebase Console:
   - Go to Firebase Console
   - Click **Project Settings** (gear icon)
   - Go to **"Your apps"** section
   - Find Android app
   - Download `google-services.json`

2. Place it in: `android/app/google-services.json`

### Step 4: (iOS Only) Add Google Services Config
If you're building for iOS:

1. Download `GoogleService-Info.plist` from Firebase Console:
   - Go to Firebase Console
   - Click **Project Settings**
   - Go to **"Your apps"** section
   - Find iOS app
   - Download `GoogleService-Info.plist`

2. Place it in: `ios/Runner/GoogleService-Info.plist`

3. Add to Xcode:
   - Open `ios/Runner.xcworkspace` (NOT `.xcodeproj`)
   - Right-click on **Runner** in left sidebar
   - Select **"Add Files to Runner"**
   - Select `GoogleService-Info.plist`
   - Check **"Copy items if needed"**
   - Click **"Add"**

---

## Testing

### Step 1: Run on Emulator/Simulator

#### Android
```bash
# Start Android emulator (if not already running)
flutter emulators --launch <emulator-name>

# Or use:
emulator -list-avds  # to see available emulators

# Run app
flutter run
```

#### iOS (macOS only)
```bash
# Open iOS simulator
open -a Simulator

# Run app
flutter run
```

### Step 2: Run on Physical Device
```bash
# Connect device via USB
# Enable USB Debugging on Android device

# List connected devices
flutter devices

# Run app
flutter run

# Or build and install APK
flutter install
```

### Testing Checklist
- [ ] **App Launches**: App starts without crashes
- [ ] **Authentication**: Can create account and login
- [ ] **Firebase Connection**: Watch logs for Firebase initialization
- [ ] **Post Item**: Can take photo and post item
- [ ] **Image Recognition**: AI categorizes items correctly
- [ ] **Home Feed**: Items display with real-time updates
- [ ] **Claim System**: Can submit and view claims
- [ ] **Profile**: Can view and update profile

---

## Building & Deployment

### Android Build

#### APK (for testing/distribution)
```bash
# Build APK
flutter build apk --release

# Output: build/app/outputs/flutter-apk/app-release.apk
```

#### App Bundle (for Google Play Store)
```bash
# Build App Bundle
flutter build appbundle --release

# Output: build/app/outputs/bundle/release/app-release.aab
```

#### Sign APK for Distribution
Before distributing, you need to sign your APK:

1. Create keystore (one-time):
```bash
keytool -genkey -v -keystore ~/key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key
```

2. Create `android/key.properties`:
```properties
storePassword=<your-password>
keyPassword=<your-password>
keyAlias=key
storeFile=<path-to-your-key.jks>
```

3. Update `android/app/build.gradle`:
```gradle
android {
    signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile file(keystoreProperties['storeFile'])
            storePassword keystoreProperties['storePassword']
        }
    }
    buildTypes {
        release {
            signingConfig signingConfigs.release
        }
    }
}
```

### iOS Build

#### Test Flight (for internal testing)
```bash
flutter build ios --release
```

Then use Xcode to upload to TestFlight.

#### App Store (production)
```bash
flutter build ios --release
```

Then follow Apple's app submission process via Xcode or App Store Connect.

### Web Build (Optional)
```bash
flutter build web --release

# Output: build/web/
# Deploy the build/web/ directory to any web hosting service
```

---

## Troubleshooting

### Common Issues & Solutions

#### 1. "Not authenticated" error
**Problem**: App says "NOT authenticated" on startup
**Solution**:
- Check Firebase credentials in `firebase_options.dart`
- Verify Firestore Rules are correctly published
- Check Firebase Console for errors

#### 2. "firebase_options.dart not found"
**Problem**: Build fails with file not found
**Solution**:
```bash
# Regenerate firebase_options.dart
flutterfire configure
```

#### 3. Image not uploading
**Problem**: Can't take/upload photos
**Solution**:
- Check Storage Rules in Firebase
- Grant camera permissions on device
- Verify user is authenticated

#### 4. ML Kit not working
**Problem**: Image labeling fails
**Solution**:
- Ensure `google_ml_kit` package is updated
- Check that image file exists and is readable
- Try with a different image

#### 5. Slow performance
**Problem**: App is laggy
**Solution**:
- Upgrade to latest Flutter version: `flutter upgrade`
- Clear build cache: `flutter clean`
- Rebuild: `flutter pub get && flutter run`

#### 6. Android build fails
**Problem**: Gradle or SDK errors
**Solution**:
```bash
flutter clean
flutter pub get
flutter build apk --verbose
```

#### 7. iOS build fails
**Problem**: Pod or Xcode errors
**Solution**:
```bash
cd ios
rm -rf Pods
cd ..
flutter clean
flutter pub get
flutter run
```

### Debug Logs
```bash
# Run with verbose logging
flutter run -v

# This shows all Firebase initialization and errors
```

---

## Deployment Checklist

### Before Production
- [ ] Firebase security rules reviewed and tested
- [ ] App tested on Android and iOS
- [ ] Performance optimized (no excessive rebuilds)
- [ ] Error handling implemented
- [ ] User privacy policy created
- [ ] Terms of service created
- [ ] Analytics configured (optional)

### Google Play Store
1. Create Google Play Developer account ($25 one-time fee)
2. Create signing key (see Android Build section)
3. Build App Bundle: `flutter build appbundle --release`
4. Upload to Google Play Console
5. Add screenshots, description, privacy policy
6. Submit for review

### Apple App Store
1. Create Apple Developer account ($99/year)
2. Create App ID in Apple Developer
3. Create provisioning profile
4. Build iOS app: `flutter build ios --release`
5. Upload via Xcode or App Store Connect
6. Add screenshots, description, privacy policy
7. Submit for review

---

## Next Steps

1. **Complete Firebase Setup** (sections above)
2. **Run Locally** to test
3. **Deploy to Testing Devices** (TestFlight/Internal Testing)
4. **Gather Feedback** and fix issues
5. **Deploy to Public Stores** (Google Play/App Store)

---

## Support & Resources

- **Flutter Docs**: https://flutter.dev/docs
- **Firebase Docs**: https://firebase.google.com/docs
- **Google ML Kit**: https://developers.google.com/ml-kit
- **Project Issues**: Check QUICK_START.md for common issues

---

**Created**: February 16, 2025  
**Status**: Ready for Setup
