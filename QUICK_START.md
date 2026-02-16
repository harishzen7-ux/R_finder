# R Finder - Quick Start Guide

## 🚀 Get Started in 5 Minutes

### 1. Download & Setup (bash)
```bash
# Navigate to project
cd R\ Finder

# Install dependencies
flutter pub get

# Configure Firebase
flutterfire configure
```

### 2. Update firebase_options.dart
After running `flutterfire configure`, your credentials will be auto-populated in:
```
lib/firebase_options.dart
```

### 3. Run the App
```bash
flutter run
```

### 4. Create an Account
- Click "Create Account"
- Use your email (any email works)
- Password must have: uppercase, lowercase, number, 8+ chars

### 5. Post an Item
- Click "Post Item" in left sidebar
- Accept camera/gallery permission
- Take or upload a photo
- AI will automatically categorize it
- Fill in title, description
- Click "Post Lost/Found Item"

## 📁 File Structure Overview

```
lib/
├── main.dart                    # App entry point
├── firebase_options.dart        # ← Update with credentials
├── models/                      # Data structures
├── services/                    # Firebase & ML logic
├── screens/                     # UI screens
├── widgets/                     # Reusable components
├── theme/                       # Design system
└── utils/                       # Helpers & constants
```

## 🔐 Firebase Setup Checklist

- [ ] Create Firebase project at console.firebase.google.com
- [ ] Enable Authentication (Email/Password)
- [ ] Create Firestore Database (Production mode)
- [ ] Enable Storage (Production mode)
- [ ] Run: `flutterfire configure`
- [ ] Update Firestore security rules (see IMPLEMENTATION_GUIDE.md)
- [ ] Update Storage security rules (see IMPLEMENTATION_GUIDE.md)

## 🎯 Key Features to Try

### Authentication
- Sign up with email
- Login with credentials
- View profile

### Posting Items
- Report lost/found items
- Upload photos (auto-categorized)
- Select location
- View posted items

### Claims System
- Browse items in locations
- Click "This Might Be Mine"
- Provide ownership proof
- Receive claim as item poster
- Approve/reject claims

### Profiles
- View user profiles
- See trust scores
- Check personal items/claims
- Update profile photo

## 🐛 Common Issues & Fixes

### App won't start
```bash
flutter clean
flutter pub get
flutter run
```

### Firebase not connecting
```bash
# Reconfigure with your Firebase project
flutterfire configure --override
```

### ML Kit not categorizing
- Check image is valid and clear
- Ensure permissions granted
- Try different image angle

### Build fails on Android
```bash
cd android
./gradlew clean
cd ..
flutter run
```

### Build fails on iOS
```bash
cd ios
rm Podfile.lock
pod install
cd ..
flutter run
```

## 📱 Test Accounts

Create your own accounts:

**Account 1** (Item Poster)
- Email: poster@example.com
- Password: Poster123!

**Account 2** (Claimant)
- Email: claimer@example.com
- Password: Claimer123!

## 🎨 Customization Points

### Change Theme Colors
Edit `lib/theme/app_theme.dart`:
```dart
static const Color primaryColor = Color(0xFF5865F2); // Change this
```

### Add New Locations
Edit `lib/utils/constants.dart`:
```dart
const List<String> LOCATIONS = [
  'Library',
  'Canteen',
  // Add your locations here
];
```

### Add Item Categories
Edit `lib/models/item_model.dart`:
```dart
enum ItemCategory {
  idCard,
  bottle,
  // Add categories here
  newCategory
}
```

## 📊 Database Status

Check data in Firebase Console:
- **Users**: View registered students
- **Items**: See all lost/found reports
- **Claims**: Track ownership claims

## 🚀 Ready to Deploy?

### Android Play Store
1. Create Google Play Developer account ($25)
2. Generate signing key
3. Build release APK
4. Upload to Play Store

### iOS App Store
1. Create Apple Developer account ($99/year)
2. Set up certificates
3. Build release archive
4. Upload to App Store

See [IMPLEMENTATION_GUIDE.md](IMPLEMENTATION_GUIDE.md) for detailed steps.

## 💡 Pro Tips

1. **Test with multiple accounts** to see claim system in action
2. **Use clear item photos** for better ML categorization
3. **Check Firebase rules** if you see "permission denied" errors
4. **Monitor Firestore usage** as it has free tier limits
5. **Test on real device** for camera/gallery to work properly

## 📚 Documentation Files

- `README.md` - Full project documentation
- `IMPLEMENTATION_GUIDE.md` - Detailed setup instructions
- `ANDROID_SETUP.md` - Android-specific configuration
- `IOS_SETUP.md` - iOS-specific configuration

## 🤝 Support

### Stack Overflow Tags
- flutter, firebase, google-ml-kit

### GitHub Issues
Create detailed issues with:
- Device/OS version
- Error logs
- Steps to reproduce
- Screenshots

## ✨ Feature Highlights

✅ Real-time item updates  
✅ AI-powered categorization  
✅ Secure claim system  
✅ Trust scoring  
✅ Discord-style UI  
✅ Mobile-first design  
✅ Offline-friendly  

## 🎓 Learning Resources

- [Flutter Docs](https://flutter.dev/docs)
- [Firebase Setup](https://firebase.google.com/docs/flutter/setup)
- [Firestore Guide](https://firebase.google.com/docs/firestore)
- [ML Kit Integration](https://developers.google.com/ml-kit/vision/image-labeling)

---

**Questions? Check IMPLEMENTATION_GUIDE.md or the README!**

**Happy coding! 🎉**
