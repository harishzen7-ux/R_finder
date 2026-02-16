# R Finder - Architecture & API Reference

## 🏗️ Application Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      UI Layer (Screens)                      │
│                                                               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │   Auth       │  │   Home       │  │   Details    │       │
│  │   Screens    │  │   Screen     │  │   Screens    │       │
│  └──────────────┘  └──────────────┘  └──────────────┘       │
└─────────────────────────────────────────────────────────────┘
                             │
                   ┌─────────┴─────────┐
                   │                   │
┌──────────────────────────┐  ┌─────────────────────────────┐
│   Widgets Layer          │  │   Services Layer           │
│                          │  │                            │
│ ├─ ItemCard            │  │ ├─ AuthService            │
│ ├─ ClaimCard           │  │ ├─ FirestoreService       │
│ ├─ UserAvatar          │  │ ├─ StorageService         │
│ ├─ CustomTextInput     │  │ ├─ ImageLabelingService   │
│ └─ Common Components   │  │ └─ (Singletons)           │
└──────────────────────────┘  └─────────────────────────────┘
          │                                      │
          └──────────────────┬───────────────────┘
                             │
┌──────────────────────────────────────────────────────────────┐
│                    Data Layer (Models)                        │
│                                                               │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │
│  │   User      │  │   Item      │  │   Claim     │          │
│  │   Model     │  │   Model     │  │   Model     │          │
│  └─────────────┘  └─────────────┘  └─────────────┘          │
└──────────────────────────────────────────────────────────────┘
                             │
┌──────────────────────────────────────────────────────────────┐
│              External Services (Firebase/ML Kit)             │
│                                                               │
│  ┌─────────────┐  ┌──────────────┐  ┌────────────┐         │
│  │ Firebase    │  │ Cloud        │  │ Google     │         │
│  │ Auth        │  │ Firestore    │  │ ML Kit     │         │
│  └─────────────┘  └──────────────┘  └────────────┘         │
│       │                  │                  │                │
│  ┌─────────────────────────────────────────────────┐        │
│  │         Firebase Storage                         │        │
│  └─────────────────────────────────────────────────┘        │
└──────────────────────────────────────────────────────────────┘
```

## 📡 Data Flow Diagrams

### Authentication Flow
```
LoginScreen
    ↓
AuthService.loginUser()
    ↓
Firebase Auth (verify)
    ↓
FirestoreService.getUser()
    ↓
User fetched → MainApp/HomeScreen
```

### Item Posting Flow
```
PostItemScreen
    ↓ (image selected)
ImageLabelingService.predictItemCategory()
    ↓ (ML Kit processes)
Category + SuggestedLocations returned
    ↓ (form filled)
StorageService.uploadItemImage()
    ↓ (image uploaded)
FirestoreService.createItem()
    ↓ (item saved)
HomeScreen updated (stream)
```

### Claim System Flow
```
ItemDetailScreen (user sees item)
    ↓ (clicks "This Might Be Mine")
ClaimItemScreen (fills proof)
    ↓ (submits claim)
FirestoreService.createClaim()
    ↓ (stored in claims collection)
ItemDetailScreen (claims stream updates)
    ↓ (item poster reviews)
FirestoreService.updateClaimStatus()
    ↓ (approved/rejected)
Both users notified (push notification)
```

## 🔗 API Reference

### AuthService

```dart
class AuthService {
  // Properties
  String? get currentUserUid
  bool get isAuthenticated
  Stream<User?> get authStateChanges
  
  // Methods
  Future<User> registerUser({
    required String email,
    required String password,
    required String name,
    required String department,
    required int year,
  })
  
  Future<User> loginUser({
    required String email,
    required String password,
  })
  
  Future<void> logout()
  
  Future<void> sendPasswordResetEmail(String email)
  
  Future<bool> isEmailRegistered(String email)
}
```

### FirestoreService

```dart
class FirestoreService {
  // User Operations
  Future<void> createUser(User user)
  Future<User?> getUser(String uid)
  Future<void> updateUser(User user)
  Future<void> increaseTrustScore(String userId, int amount)
  
  // Item Operations
  Future<String> createItem(Item item)
  Future<Item?> getItem(String itemId)
  Future<List<Item>> getItems({int limit = 20, DocumentSnapshot? startAfter})
  Future<List<Item>> getItemsByLocation(String location)
  Future<List<Item>> getItemsByType(ItemType type)
  Future<List<Item>> getItemsByUser(String userId)
  Stream<List<Item>> streamItemsByLocation(String location)
  Future<void> updateItem(Item item)
  Future<void> updateItemStatus(String itemId, ItemStatus status)
  Future<void> updateApprovedClaim(String itemId, String claimId)
  Future<void> deleteItem(String itemId)
  
  // Claim Operations
  Future<String> createClaim(Claim claim)
  Future<Claim?> getClaim(String claimId)
  Future<List<Claim>> getClaimsByItem(String itemId)
  Future<List<Claim>> getClaimsByUser(String userId)
  Stream<List<Claim>> streamClaimsByItem(String itemId)
  Future<Claim?> getUserClaimForItem(String itemId, String userId)
  Future<void> updateClaimStatus(String claimId, ClaimStatus status)
  Future<void> deleteClaim(String claimId)
  
  // Search
  Future<List<Item>> searchItems(String query)
}
```

### StorageService

```dart
class StorageService {
  Future<String> uploadItemImage({
    required File imageFile,
    required String itemId,
  })
  
  Future<String> uploadUserProfilePhoto({
    required File imageFile,
    required String userId,
  })
  
  Future<void> deleteImage(String imageUrl)
  
  Future<void> deleteItemFolder(String itemId)
  
  Future<String> getImageUrl(String path)
  
  Future<bool> imageExists(String imageUrl)
}
```

### ImageLabelingService

```dart
class ImageLabelingService {
  Future<List<ImageLabel>> getImageLabels(File imageFile)
  
  Future<(ItemCategory, double)> predictItemCategory(File imageFile)
  
  List<String> suggestLocations(ItemCategory category)
}
```

## 🎯 Routes & Navigation

| Route | Widget | Auth Required | Arguments |
|-------|--------|:-------------:|-----------|
| `/` | AuthWrapper | ❌ | None |
| `/auth` | AuthEntryScreen | ❌ | None |
| `/login` | LoginScreen | ❌ | onSuccess callback |
| `/signup` | SignupScreen | ❌ | onSuccess callback |
| `/home` | HomeScreen | ✅ | None |
| `/post-item` | PostItemScreen | ✅ | None |
| `/item-detail` | ItemDetailScreen | ✅ | Item object |
| `/profile` | ProfileScreen | ✅ | None |

## 🔒 Security Model

### Authentication
- Firebase Auth handles password hashing
- Email verification (optional)
- Password reset via email
- Session management automatic

### Firestore Rules
```firestore
// Only authenticated users can read
match /users/{userId} {
  allow read: if request.auth != null;
  allow write: if request.auth.uid == userId;
}

// Items editable only by creator
match /items/{itemId} {
  allow read: if request.auth != null;
  allow create: if request.auth != null;
  allow update: if request.auth.uid == resource.data.postedBy;
}

// Claims reviewable only by item poster
match /claims/{claimId} {
  allow read: if request.auth != null;
  allow create: if request.auth != null;
  allow update: if request.auth.uid == get(...).data.postedBy;
}
```

### Image Storage
- Per-user image upload restrictions
- Automatic cleanup on item deletion
- URL-based access control

## 📊 State Management

```
Provider Pattern (via Singleton Services)

AuthService (singleton)
  └─ Manages auth state
  └─ Notified by Stream<User?>
  
FirestoreService (singleton)
  └─ All database operations
  └─ Returns Futures and Streams
  
StorageService (singleton)
  └─ Image upload/download
  
ImageLabelingService (singleton)
  └─ ML Kit operations
```

## 🔄 Stream Handling

```dart
// Real-time item updates
StreamBuilder<List<Item>>(
  stream: firestoreService.streamItemsByLocation(location),
  builder: (context, snapshot) {
    // Rebuilds when items change
  }
)

// Real-time claim updates
StreamBuilder<List<Claim>>(
  stream: firestoreService.streamClaimsByItem(itemId),
  builder: (context, snapshot) {
    // Rebuilds when claims change
  }
)

// Auth state monitoring
StreamBuilder<User?>(
  stream: authService.authStateChanges,
  builder: (context, snapshot) {
    // Routes based on auth
  }
)
```

## 🎨 Widget Composition

```
MaterialApp (dark theme)
  ├─ AuthWrapper (auth routing)
  ├─ HomeScreen (main feed)
  │   ├─ ItemFeed (location items)
  │   │   └─ ItemCard
  │   │       ├─ CachedNetworkImage
  │   │       ├─ StatusBadge
  │   │       └─ LocationChip
  │   └─ LocationSidebar
  │       └─ LocationChip
  ├─ ItemDetailScreen
  │   ├─ CachedNetworkImage
  │   ├─ ClaimCard
  │   └─ UserAvatar
  ├─ PostItemScreen
  │   ├─ CustomTextInput
  │   ├─ ImagePicker
  │   └─ LocationChip
  └─ ProfileScreen
      ├─ UserAvatar
      ├─ TabBar
      └─ Item/Claim Lists
```

## 🔄 Model Relationships

```
User
├─ uid (PK)
├─ name
├─ email
└─ trustScore

Item
├─ id (PK)
├─ postedBy (FK → User.uid)
├─ approvedClaimId (FK → Claim.id)
├─ category (enum)
├─ location (enum)
└─ status (enum)

Claim
├─ id (PK)
├─ itemId (FK → Item.id)
├─ claimantId (FK → User.uid)
├─ status (enum)
└─ message
```

## 🚀 Performance Optimizations

- ✅ Cached network images
- ✅ Pagination for item lists
- ✅ Firestore indexing ready
- ✅ Lazy loading screens
- ✅ Singleton pattern (avoid recreates)
- ✅ Stream batching
- ✅ Error handling to prevent crashes

## 📈 Scalability

Ready for expansion:
- ✅ Add chat feature (new collection)
- ✅ Add notifications (Firebase Messaging)
- ✅ Add search (Algolia integration)
- ✅ Add user blocking (new fields)
- ✅ Add reports/moderation (new collection)
- ✅ Add ratings (extend Item model)

---

**This architecture ensures clean, maintainable, scalable code! 🎯**
