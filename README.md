# R Finder - Campus Lost & Found Application

A Flutter mobile application that replaces WhatsApp groups for reporting and finding lost items on campus. Built with Firebase, Material 3, and Google ML Kit for intelligent item categorization.

## Features

### Core Features
- ✅ **User Authentication**: Email/password login with college email preferred
- ✅ **Lost & Found Reporting**: Post lost or found items with images
- ✅ **Smart Categorization**: AI-powered item categorization using Google ML Kit
- ✅ **Location-Based Organization**: Items organized by campus locations (like Discord channels)
- ✅ **Claim System**: Secure claim system where users prove ownership before contact
- ✅ **Trust Score**: Track user reliability through successful item returns
- ✅ **Real-time Updates**: Firestore streams for instant item updates

### Technical Features
- 🎨 **Discord-Inspired UI**: Dark theme with modern card-based design
- 🔒 **Firebase Security**: Secure authentication and database rules
- 📸 **Image Intelligence**: Google ML Kit image labeling
- 📱 **Responsive Design**: Optimized for mobile devices
- 🌐 **Cloud Storage**: Firebase Storage for all images
- 🔔 **Push Notifications**: Firebase Cloud Messaging (ready for integration)

## Project Structure

```
lib/
├── main.dart                 # App entry point
├── firebase_options.dart     # Firebase configuration
├── models/
│   ├── user_model.dart       # User data model
│   ├── item_model.dart       # Item data model
│   └── claim_model.dart      # Claim data model
├── services/
│   ├── auth_service.dart              # Firebase Auth
│   ├── firestore_service.dart         # Cloud Firestore
│   ├── storage_service.dart           # Firebase Storage
│   └── image_labeling_service.dart    # ML Kit integration
├── screens/
│   ├── auth_screen.dart               # Auth entry screen
│   ├── login_screen.dart              # Login screen
│   ├── signup_screen.dart             # Registration screen
│   ├── home_screen.dart               # Main feed (Discord-style)
│   ├── post_item_screen.dart          # Post lost/found item
│   ├── item_detail_screen.dart        # Item details & claims
│   ├── claim_item_screen.dart         # Submit ownership claim
│   └── profile_screen.dart            # User profile
├── widgets/
│   ├── common_widgets.dart            # Common UI components
│   └── item_widgets.dart              # Item-specific widgets
├── theme/
│   └── app_theme.dart                 # Material 3 dark theme
└── utils/
    ├── constants.dart                 # App constants
    └── helpers.dart                   # Utility functions
```

## Setup Instructions

### Prerequisites
- Flutter 3.0 or higher
- Android Studio or Xcode
- Firebase CLI
- Google ML Kit setup

### 1. Clone Repository
```bash
git clone <repository-url>
cd r_finder
```

### 2. Install Dependencies
```bash
flutter pub get
```

### 3. Firebase Setup

#### a. Create Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project
3. Enable the following services:
   - **Authentication**: Email/Password
   - **Firestore Database**: Create database
   - **Storage**: Firebase Storage
   - **Cloud Messaging**: For notifications
   - **ML Kit**: Image Labeling API

#### b. Configure FlutterFire
```bash
# Install FlutterFire CLI
dart pub global activate flutterfire_cli

# Configure for your Firebase project
flutterfire configure
```

This will automatically generate `firebase_options.dart` with your project credentials.

#### c. Firestore Security Rules
Set the following security rules in Firestore:

```firestore
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users collection
    match /users/{userId} {
      allow read: if request.auth != null;
      allow create, update: if request.auth.uid == userId;
    }

    // Items collection
    match /items/{itemId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update, delete: if request.auth.uid == resource.data.postedBy;
    }

    // Claims collection
    match /claims/{claimId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update: if request.auth.uid == get(/databases/$(database)/documents/items/$(resource.data.itemId)).data.postedBy;
    }
  }
}
```

#### d. Storage Security Rules
```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /items/{itemId}/{allPaths=**} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }

    match /users/{userId}/profile.jpg {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == userId;
    }
  }
}
```

### 4. Run the App

#### Android
```bash
flutter run -d android
```

#### iOS
```bash
flutter run -d ios
```

#### Both (interactive)
```bash
flutter run
```

## Database Schema

### Users Collection
```dart
{
  uid: String,
  name: String,
  email: String,
  department: String,
  year: int,
  trustScore: int,
  createdAt: Timestamp,
  photoUrl: String?
}
```

### Items Collection
```dart
{
  id: String,
  title: String,
  description: String,
  imageUrl: String,
  type: "lost" | "found",
  category: String,
  location: String,
  suggestedLocations: [String],
  status: "open" | "claimed" | "returned",
  postedBy: String,
  approvedClaimId: String?,
  createdAt: Timestamp
}
```

### Claims Collection
```dart
{
  id: String,
  itemId: String,
  claimantId: String,
  message: String,
  status: "pending" | "approved" | "rejected",
  createdAt: Timestamp,
  reviewedAt: Timestamp?
}
```

## Available Locations
- Library
- Canteen
- Lab
- Classroom
- Ground
- Hostel
- Bus
- Parking
- Corridor

## Item Categories
- ID Card
- Bottle
- Electronics
- Bag
- Keys
- Stationery
- Other

## Key Functionality

### Smart Item Categorization
The app uses Google ML Kit's Image Labeling to automatically detect item categories:
- Analyzes uploaded image
- Suggests item category based on detected objects
- Recommends likely locations for the item type
- Shows confidence score to user

### Claim System
- Users click "This Might Be Mine"
- Provide ownership proof (20-500 characters)
- Finder reviews claims and approves/rejects
- On approval: item marked as claimed, contact unlocked
- Trust scores updated on successful returns

### Location-Based Organization
Discord-style channels for each campus location:
- Users see items posted in selected location
- Real-time updates using Firestore streams
- Filter by item type (lost/found)

## Environment Variables

Create a `.env` file (optional):
```
FIREBASE_API_KEY=your_api_key
FIREBASE_PROJECT_ID=your_project_id
```

## Build for Production

### Android
```bash
flutter build apk --release
```

### iOS
```bash
flutter build ios --release
```

## Code Quality

The project follows these best practices:

- **Clean Architecture**: Separation of concerns
- **Reusable Widgets**: DRY principle with customizable UI components
- **Error Handling**: Comprehensive try-catch and user feedback
- **Loading States**: Proper loading indicators and skeleton screens
- **Validation**: Input validation with helpful error messages
- **Documentation**: Inline comments explaining complex logic
- **Type Safety**: Strong typing with Dart

## Troubleshooting

### Firebase Configuration Issues
```bash
# Clear Flutter cache
flutter clean

# Get new dependencies
flutter pub get

# Force FlutterFire reconfiguration
flutterfire configure --override
```

### Image Labeling Not Working
- Ensure ML Kit permissions are set in AndroidManifest.xml
- Check Google ML Kit is enabled in Firebase Console
- Verify Android build.gradle has correct dependencies

### Build Issues
```bash
# Update Flutter
flutter upgrade

# Clean and rebuild
flutter clean
flutter pub get
flutter pub upgrade
flutter run
```

## API Routes / Screens

| Route | Purpose | Auth Required |
|-------|---------|---------------|
| `/` | Auth check -> Route home/auth | ❌ |
| `/auth` | Auth entry screen | ❌ |
| `/login` | Login screen | ❌ |
| `/signup` | Registration screen | ❌ |
| `/home` | Main feed | ✅ |
| `/post-item` | Report item | ✅ |
| `/item-detail` | Item details & claims | ✅ |
| `/profile` | User profile | ✅ |

## Future Enhancements

- [ ] In-app chat after claim approval
- [ ] Push notifications
- [ ] Search functionality with Algolia
- [ ] Email notifications
- [ ] User blocking/reporting
- [ ] Advanced filters (date, category, location)
- [ ] Item expiration (auto-archive after 30 days)
- [ ] QR codes for items
- [ ] Image comparison for claims

## Dependencies

Core dependencies handled via pubspec.yaml:
- Firebase Core, Auth, Firestore, Storage, Messaging
- Google ML Kit
- Image Picker
- Material 3 UI
- Provider for state management

## License

This project is provided as-is for educational purposes.

## Support

For issues and questions:
1. Check this README
2. Review Firebase documentation
3. Check Flutter documentation
4. Create an issue in the repository

## Contributors

Built with ❤️ for your campus community.

---

**Happy Lost & Found Finding! 🔍**
