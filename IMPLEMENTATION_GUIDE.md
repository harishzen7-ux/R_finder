# R Finder - Implementation Guide

This guide provides step-by-step instructions to set up and configure the R Finder application.

## Step 1: Project Initialization

### Create Flutter Project (if starting fresh)
```bash
flutter create r_finder
cd r_finder
```

### Install Dependencies
```bash
flutter pub get
```

## Step 2: Firebase Setup

### 2.1 Create Firebase Project
1. Visit [Firebase Console](https://console.firebase.google.com/)
2. Click "Create a project"
3. Name it "R Finder"
4. Enable Google Analytics (optional)
5. Create the project

### 2.2 Enable Firebase Services

#### Authentication
1. Go to **Authentication** section
2. Click **Get Started**
3. Enable **Email/Password** provider
4. Optional: Add other providers (Google, etc.)

#### Cloud Firestore
1. Go to **Firestore Database**
2. Click **Create Database**
3. Choose **Production mode** (configure rules below)
4. Select region (closest to your university)
5. Create

#### Firebase Storage
1. Go to **Storage**
2. Click **Get Started**
3. Start in **Production mode** (configure rules below)
4. Select same region as Firestore
5. Done

#### Cloud Messaging (Optional)
1. Go to **Cloud Messaging**
2. Keep default settings
3. Enable for notifications later

#### ML Kit
1. Out of the box, Google ML Kit is available
2. Image Labeling is enabled by default

### 2.3 Configure Firebase with Flutter

#### Install FlutterFire CLI
```bash
dart pub global activate flutterfire_cli
```

#### Configure Your Project
```bash
# From your project root
flutterfire configure

# Follow prompts:
# - Select platforms (Android/iOS/Web)
# - Select your Firebase project
# - Use default settings for new apps
```

This generates `lib/firebase_options.dart` automatically.

### 2.4 Set Firestore Security Rules

1. In Firebase Console, go to **Firestore > Rules**
2. Replace with:

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
      allow update: if request.auth.uid == get(/databases/$(database)/documents/items/$(resource.data.itemId)).data.postedBy;
    }
  }
}
```

3. Publish the rules

### 2.5 Set Storage Security Rules

1. In Firebase Console, go to **Storage > Rules**
2. Replace with:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    // Allow read for all authenticated users
    match /{allPaths=**} {
      allow read: if request.auth != null;
    }
    
    // Allow write to personal folder
    match /users/{userId}/{allPaths=**} {
      allow write: if request.auth.uid == userId;
    }
    
    // Allow write to own items
    match /items/{itemId}/{allPaths=**} {
      allow write: if request.auth != null;
    }
  }
}
```

3. Publish the rules

## Step 3: Project Configuration

### 3.1 Update Package Name

#### Android
1. Open `android/app/build.gradle`
2. Change applicationId to: `com.yourdomain.r_finder`
3. Update `android/app/src/main/AndroidManifest.xml` package name

#### iOS
1. Open `ios/Runner.xcodeproj` in Xcode
2. Change bundle identifier to: `com.yourdomain.r-finder`
3. Update iOS deployment target to 12.0 or higher

### 3.2 Update App Icons

1. Create 1024x1024 icon: place in `assets/icon/icon.png`
2. Use [Flutter Icon Generator](https://appicon.co/) or:

```bash
flutter pub add flutter_launcher_icons
# Add to pubspec.yaml:
flutter_launcher_icons:
  android: true
  ios: true
  image_path: assets/icon/icon.png

# Run:
dart run flutter_launcher_icons
```

### 3.3 Configure Permissions

#### Android Permissions
Edit `android/app/src/main/AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

#### iOS Permissions
Edit `ios/Runner/Info.plist`:

```xml
<key>NSCameraUsageDescription</key>
<string>Camera access is needed to photograph items.</string>

<key>NSPhotoLibraryUsageDescription</key>
<string>Photo library access is needed to upload item images.</string>
```

## Step 4: Run the Application

### Clear Previous Builds
```bash
flutter clean
```

### Get Dependencies
```bash
flutter pub get
```

### Run on Emulator/Device
```bash
# List devices
flutter devices

# Run on Android
flutter run -d <device-id>

# Run on iOS
flutter run -d <device-id>

# Interactive mode (select device)
flutter run
```

## Step 5: Testing

### Test Authentication
1. Launch app
2. Click "Create Account"
3. Fill in test details:
   - Name: Test User
   - Email: test@example.com
   - Department: CSE
   - Year: 2
   - Password: TestPassword123!
4. Verify in Firebase Console > Authentication

### Test Item Posting
1. Login with created account
2. Click "Post Item"
3. Take/upload photo
4. Verify ML Kit categorization
5. Fill details and post
6. Verify in Firestore > items collection

### Test Claims
1. Create another test account
2. View first user's item
3. Click "This Might Be Mine"
4. Submit proof
5. View in first account's profile
6. Approve/reject claim

## Step 6: Deployment

### Android APK Build
```bash
# Debug APK
flutter build apk --debug

# Release APK
flutter build apk --release

# Located at: build/app/outputs/flutter-apk/app-release.apk
```

### Android App Bundle (For Play Store)
```bash
flutter build appbundle --release
# Located at: build/app/outputs/bundle/release/app-release.aab
```

### iOS Build
```bash
# Build for App Store
flutter build ios --release

# Then use Xcode:
# 1. Open ios/Runner.xcworkspace
# 2. Select Runner > Product > Archive
# 3. Validate and upload to App Store
```

## Troubleshooting

### Firebase Configuration Issues

**Problem**: `FirebaseException: No Firebase App '[DEFAULT]' has been created`

**Solution**:
```bash
flutter clean
flutterfire configure --override
flutter pub get
```

### ML Kit Not Loading

**Problem**: Image labeling returns empty results

**Solution**:
1. Ensure image quality > 50% in image picker
2. Check ML Kit is enabled in Firebase
3. Verify image file exists and is valid
4. Check Android/iOS permissions

### Build Issues

**Problem**: Gradle/build errors

**Solution**:
```bash
flutter clean
cd android
./gradlew clean
cd ..
flutter pub get
flutter run
```

### iOS Pods Issues

**Problem**: Pod dependency errors

**Solution**:
```bash
cd ios
rm Podfile.lock
pod repo update
pod install
cd ..
flutter run
```

## Environment Variables (Optional)

Create `.env` file in project root:
```
DEBUG_MODE=false
ENABLE_MOCK_DATA=false
API_TIMEOUT=30
```

Install package:
```bash
flutter pub add flutter_dotenv
```

Load in main.dart:
```dart
import 'package:flutter_dotenv/flutter_dotenv.dart';

void main() async {
  await dotenv.load();
  runApp(const MyApp());
}
```

## Production Checklist

- [ ] Firebase project created and configured
- [ ] All services enabled (Auth, Firestore, Storage)
- [ ] Security rules configured
- [ ] App signing configured (Android)
- [ ] App bundle uploaded to Play Store (Android)
- [ ] App archived and uploaded to App Store (iOS)
- [ ] Password requirements enforced
- [ ] Rate limiting implemented
- [ ] Error handling comprehensive
- [ ] Privacy policy created
- [ ] Terms of Service created
- [ ] Analytics configured

## Next Steps

### For Notifications
1. Install `firebase_messaging` (already in pubspec)
2. Implement message handlers
3. Request notification permissions
4. Test with Firebase Cloud Messaging

### For Search
1. Implement full-text search using Algolia
2. Or use Firestore search with proper indexing

### For Chat
1. Create messages collection
2. Implement real-time chat after claim approval
3. Add message notifications

### For Advanced Features
1. User blocking system
2. Reporting/moderation
3. Item expiration
4. Payment integration (if needed)

## Support Resources

- [Flutter Documentation](https://flutter.dev/docs)
- [Firebase Documentation](https://firebase.google.com/docs)
- [Firebase Console](https://console.firebase.google.com/)
- [Flutter Pub](https://pub.dev/)

---

**You're all set! Start building your campus community! 🚀**
