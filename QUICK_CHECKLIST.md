# R Finder - Quick Start Checklist

## ✅ Setup Checklist

### Phase 1: Firebase Configuration (15 min)
- [ ] Create Firebase project: https://console.firebase.google.com
- [ ] Enable Authentication (Email/Password)
- [ ] Create Firestore Database (Production mode)
- [ ] Create Firebase Storage (Production mode)
- [ ] Install FlutterFire CLI: `dart pub global activate flutterfire_cli`
- [ ] Run configuration: `flutterfire configure`
- [ ] Copy Firestore security rules from SETUP_AND_DEPLOYMENT.md
- [ ] Copy Storage security rules from SETUP_AND_DEPLOYMENT.md
- [ ] Publish rules in Firebase Console

### Phase 2: Local Setup (5 min)
- [ ] Navigate to project: `cd "c:\Users\haris\OneDrive\Desktop\R Finder"`
- [ ] Install dependencies: `flutter pub get`
- [ ] Run Flutter doctor: `flutter doctor`
- [ ] (Android) Download and place `google-services.json`
- [ ] (iOS) Download and place `GoogleService-Info.plist`

### Phase 3: Local Testing (10 min)
- [ ] Start emulator/simulator
- [ ] Run app: `flutter run`
- [ ] Test authentication (create account)
- [ ] Test posting item with camera
- [ ] Test AI image recognition
- [ ] Check home feed updates

### Phase 4: Build & Release (varies)
- [ ] Build for target platform:
  - Android: `flutter build apk --release`
  - iOS: `flutter build ios --release`
  - Web: `flutter build web --release`
- [ ] Sign APK for distribution (Android)
- [ ] Test on physical device
- [ ] Deploy to store (Google Play/App Store)

---

## 🔑 Key Commands Reference

### Development
```bash
# Install dependencies
flutter pub get

# Run app with verbose logging
flutter run -v

# Check setup
flutter doctor -v

# Clean build
flutter clean
flutter pub get
```

### Firebase Setup
```bash
# Configure Firebase
flutterfire configure

# View Firebase config
cat lib/firebase_options.dart
```

### Building
```bash
# Android APK
flutter build apk --release

# Android App Bundle
flutter build appbundle --release

# iOS
flutter build ios --release

# Web
flutter build web --release
```

### Testing
```bash
# Run tests
flutter test

# Run on specific device
flutter run -d <device-id>

# List available devices
flutter devices
```

---

## 📱 Platform-Specific Setup

### ☑️ Android Prerequisites
- Android SDK 21+
- Android Studio installed
- Java Development Kit (JDK) 11+
- `google-services.json` from Firebase

### ☑️ iOS Prerequisites (macOS only)
- Xcode 13+
- CocoaPods
- `GoogleService-Info.plist` from Firebase
- Apple Developer account (for distribution)

### ☑️ Web Prerequisites
- Chrome/Firefox for testing
- Any web hosting for deployment

---

## 🐛 Quick Troubleshooting

| Issue | Solution |
|-------|----------|
| FlutterFire not found | `dart pub global activate flutterfire_cli` |
| Firebase config missing | Run `flutterfire configure` again |
| App won't start | Check `flutter doctor` and fix warnings |
| Package conflicts | Run `flutter pub upgrade` |
| Android build fails | Run `flutter clean` and try again |
| iOS build fails | `cd ios && rm -rf Pods && cd ..` then `flutter pub get` |
| Camera not working | Grant permissions in app & device settings |
| Image recognition fails | Check image file permissions |
| Real-time updates slow | Check Firestore database capacity |

---

## 📊 Expected Features After Setup

### ✅ Core Features to Test
1. **Authentication**
   - Email/password signup
   - Email/password login
   - Password reset
   - Logout

2. **Post Items**
   - Take photo with camera
   - Pick photo from gallery
   - AI auto-categorizes item
   - Fill in item details
   - See item in home feed

3. **Search & Browse**
   - Filter by location
   - View all items
   - See item details
   - View poster profile

4. **Claim System**
   - Submit claim for item
   - Provide proof of ownership
   - See claim status
   - Receive notification on claim update

5. **User Profile**
   - Edit profile details
   - Upload profile photo
   - View trust score
   - See posted items history
   - View claims history

---

## 🚀 Performance Tips

1. **First Run** - Takes longer due to dependency resolution
2. **Subsequent Runs** - Much faster (10-30 seconds)
3. **Hot Reload** - Use during development: `r` key
4. **Hot Restart** - For state changes: `R` key
5. **Full Rebuild** - `flutter clean` then `flutter run`

---

## 📞 Getting Help

1. Check **SETUP_AND_DEPLOYMENT.md** for detailed instructions
2. Check **QUICK_START.md** for common issues
3. Run `flutter doctor -v` to diagnose environment issues
4. Check Firebase Console for authentication/database errors
5. Review app logs: `flutter run -v`

---

## ⏱️ Estimated Timeline

| Phase | Time | Status |
|-------|------|--------|
| Firebase Setup | 15 min | ⏭️ Start here |
| Local Setup | 5 min | ⏭️ |
| Local Testing | 10 min | ⏭️ |
| Build for Android | 5 min | ⏭️ |
| Build for iOS | 5 min | ⏭️ (if on macOS) |
| **Total** | **~40 min** | |

---

**Status**: Ready to start setup
**Last Updated**: February 16, 2025
