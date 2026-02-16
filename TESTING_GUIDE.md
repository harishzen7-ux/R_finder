# R Finder - Testing & QA Guide

## 📋 Complete Testing Guide

This guide covers all aspects of testing the R Finder application.

---

## Part 1: Pre-Launch Testing

### 1.1 Code Quality Checks

```bash
# Analyze code for issues
flutter analyze

# Format code
flutter format .

# Check for unused imports
dart fix --apply
```

### 1.2 Unit Testing

```bash
# Run basic tests
flutter test

# Run specific test file
flutter test test/models/user_model_test.dart

# Run tests with coverage
flutter test --coverage
```

---

## Part 2: Manual Testing Scenarios

### Test Case 1: User Registration

**Objective**: Verify user can create account

| Step | Action | Expected Result | Status |
|------|--------|-----------------|--------|
| 1 | Open app | See "Login/Signup" screen | ☐ Pass |
| 2 | Click "Create Account" | Navigate to signup form | ☐ Pass |
| 3 | Enter invalid email | Error message shown | ☐ Pass |
| 4 | Enter short password | Error message required | ☐ Pass |
| 5 | Fill valid form | All fields accept input | ☐ Pass |
| 6 | Click "Sign Up" | Account created, navigate to home | ☐ Pass |
| 7 | Logout and login | Can login with new credentials | ☐ Pass |

**Test Data**:
- Email: `testuser@example.com`
- Password: `TestPassword123`
- Name: `Test User`
- Department: `CSE`
- Year: `2`

---

### Test Case 2: Authentication Flow

**Objective**: Verify login/logout works correctly

| Step | Action | Expected Result | Status |
|------|--------|-----------------|--------|
| 1 | Open app | See auth screen if not logged in | ☐ Pass |
| 2 | Click login | Navigate to login form | ☐ Pass |
| 3 | Enter wrong password | Error shown | ☐ Pass |
| 4 | Enter correct credentials | Login successful, go to home | ☐ Pass |
| 5 | Close app | State persists on reopen | ☐ Pass |
| 6 | Click logout | Return to auth screen | ☐ Pass |

---

### Test Case 3: Post Item - Camera Flow

**Objective**: Verify posting item with camera photo

| Step | Action | Expected Result | Status |
|------|--------|-----------------|--------|
| 1 | Login successful | On home screen | ☐ Pass |
| 2 | Click "Post Item" | Navigate to post form | ☐ Pass |
| 3 | Click camera button | Camera opens | ☐ Pass |
| 4 | Take photo | Photo captured | ☐ Pass |
| 5 | Confirm photo | Image displayed in form | ☐ Pass |
| 6 | Wait 2-3 seconds | ML Kit processes image | ☐ Pass |
| 7 | Check preview | Category auto-filled (e.g., "Electronics") | ☐ Pass |
| 8 | See suggestions | Location suggestions show | ☐ Pass |
| 9 | Select or change category | Can modify suggestions | ☐ Pass |
| 10 | Select location | Dropdown works | ☐ Pass |
| 11 | Fill item details | Title, description required | ☐ Pass |
| 12 | Select status | "Lost/Found" choice available | ☐ Pass |
| 13 | Click "Post" | Item created, navigate to home | ☐ Pass |
| 14 | Check home feed | New item visible at top | ☐ Pass |

---

### Test Case 4: Post Item - Gallery Flow

**Objective**: Verify posting item with gallery photo

| Same as Test Case 3, but: |
|---|
| Step 3: Click gallery icon instead | ☐ Pass |
| Step 4-5: Select existing photo from gallery | ☐ Pass |
| Rest follows same flow | ☐ Pass |

---

### Test Case 5: Home Feed & Filtering

**Objective**: Verify home feed displays items correctly

| Step | Action | Expected Result | Status |
|------|--------|-----------------|--------|
| 1 | On home screen | Feed loads items | ☐ Pass |
| 2 | Scroll feed | Can scroll down, more items load | ☐ Pass |
| 3 | See location tabs | Locations like "Library", "Canteen" show | ☐ Pass |
| 4 | Click location tab | Items filtered to that location | ☐ Pass |
| 5 | Switch locations | Feed updates appropriately | ☐ Pass |
| 6 | See item card | Shows photo, title, location, status | ☐ Pass |
| 7 | See "Lost" badge | Items marked appropriately (red for lost, green for found) | ☐ Pass |

**Test Data**: Post 3-4 items at different locations before testing

---

### Test Case 6: View Item Details

**Objective**: Verify item detail screen works

| Step | Action | Expected Result | Status |
|------|--------|-----------------|--------|
| 1 | Click item in feed | Navigate to detail screen | ☐ Pass |
| 2 | See full image | Image displays correctly | ☐ Pass |
| 3 | See details | Title, category, location all visible | ☐ Pass |
| 4 | See poster info | Poster name, trust score visible | ☐ Pass |
| 5 | Click poster avatar | Navigate to poster profile | ☐ Pass |
| 6 | Go back | Return to item detail | ☐ Pass |
| 7 | See claims section | If any claims exist, they show | ☐ Pass |
| 8 | See "This Might Be Mine" button | Button visible if not poster | ☐ Pass |
| 9 | Click button | Navigate to claim form | ☐ Pass |

---

### Test Case 7: Submit Claim

**Objective**: Verify claiming an item works

| Step | Action | Expected Result | Status |
|------|--------|-----------------|--------|
| 1 | On item detail | Item found and claim button visible | ☐ Pass |
| 2 | Click "This Might Be Mine" | Navigate to claim form | ☐ Pass |
| 3 | Try submit without text | Error: "Proof required" | ☐ Pass |
| 4 | Enter short text | Error: "At least 20 characters" | ☐ Pass |
| 5 | Enter valid text | Text accepted (e.g., "This is my laptop, I left it...") | ☐ Pass |
| 6 | Click "Submit Claim" | Claim submitted, navigate back | ☐ Pass |
| 7 | Go back to item | Claim appears in claims list | ☐ Pass |
| 8 | See claim status | Shows "Pending Review" | ☐ Pass |

---

### Test Case 8: Review Claims (as item poster)

**Objective**: Verify posting user can review claims

| Step | Action | Expected Result | Status |
|------|--------|-----------------|--------|
| 1 | Post own item | Item created successfully | ☐ Pass |
| 2 | Other user claims it | Claim submitted | ☐ Pass |
| 3 | On item detail | See pending claim | ☐ Pass |
| 4 | See approval buttons | Has "Approve" and "Reject" buttons | ☐ Pass |
| 5 | Click "Approve" | Claim approved, status changes | ☐ Pass |
| 6 | Check notifications | Should receive notification (future) | ☐ Pass |

---

### Test Case 9: User Profile

**Objective**: Verify profile screen works

| Step | Action | Expected Result | Status |
|------|--------|-----------------|--------|
| 1 | Click profile icon | Navigate to profile | ☐ Pass |
| 2 | See user info | Name, email, department, year visible | ☐ Pass |
| 3 | See trust score | Trust score displayed | ☐ Pass |
| 4 | See posted items tab | List of items user posted | ☐ Pass |
| 5 | Click on posted item | Navigate to item detail | ☐ Pass |
| 6 | See claims tab | List of claims user made | ☐ Pass |
| 7 | See received items tab | Items user received/claimed | ☐ Pass |
| 8 | See edit profile button | Can access edit form | ☐ Pass |

---

### Test Case 10: Edit Profile

**Objective**: Verify user can edit their profile

| Step | Action | Expected Result | Status |
|------|--------|-----------------|--------|
| 1 | On profile | See "Edit Profile" button | ☐ Pass |
| 2 | Click button | Navigate to edit form | ☐ Pass |
| 3 | Change name | Field editable | ☐ Pass |
| 4 | Change department | Dropdown shows options | ☐ Pass |
| 5 | Upload photo | Camera/gallery picker available | ☐ Pass |
| 6 | Click upload | Photo taken/selected | ☐ Pass |
| 7 | Click "Save" | Profile updated, return to profile | ☐ Pass |
| 8 | Verify changes | New info visible on profile | ☐ Pass |

---

### Test Case 11: Real-time Updates

**Objective**: Verify live updates work

| Step | Action | Expected Result | Status |
|------|--------|-----------------|--------|
| 1 | Open app on 2 devices/emulators | Both logged in | ☐ Pass |
| 2 | Post item on device 1 | Item appears on feed | ☐ Pass |
| 3 | Wait 1-2 seconds | Item appears on device 2 feed automatically | ☐ Pass |
| 4 | Submit claim on device 2 | Claim appears on device 1 | ☐ Pass |
| 5 | Approve claim on device 1 | Status updates on device 2 | ☐ Pass |

---

## Part 3: Performance Testing

### 3.1 Load Testing

```bash
# Test with large dataset
# Manually:
# 1. Post 20+ items
# 2. Create 10+ claims
# 3. Check feed responsiveness
```

**Expected Results**:
- Initial load: 2-3 seconds
- Scroll: Smooth (60 fps)
- Photo upload: < 5 seconds
- Claim submission: < 2 seconds

### 3.2 Memory Profiling

```bash
# Monitor memory usage
flutter run --enable-impeller  # Better performance
```

**Expected Results**:
- Initial: ~100-150 MB
- After navigation: ~200 MB
- Stable (no leaks)

### 3.3 Network Testing

- Test on slow network (throttle in DevTools)
- Test with WiFi disconnected
- Test with WiFi reconnecting

---

## Part 4: Device Testing

### 4.1 Android Devices to Test

- [ ] Android 12 phone
- [ ] Android 13 phone  
- [ ] Android 14 phone
- [ ] Tablet (if applicable)

### 4.2 iOS Devices to Test (if on macOS)

- [ ] iPhone 12
- [ ] iPhone 13
- [ ] iPhone 14
- [ ] iPad (if applicable)

### 4.3 Screen Sizes to Test

- [ ] Small phone (5")
- [ ] Large phone (6.5"+)
- [ ] Tablet (10"+)

### 4.4 Device Conditions

- [ ] Airplane mode
- [ ] Low battery mode
- [ ] Dark theme
- [ ] Large text size

---

## Part 5: Security Testing

### 5.1 Authentication Security

- [ ] Can't access home without login
- [ ] Can't modify other user's data
- [ ] Session expires properly
- [ ] Forgot password works

### 5.2 Data Security

- [ ] Can only edit own items
- [ ] Can't delete other user's items
- [ ] Claims visible only to involved users
- [ ] Private data not exposed

### 5.3 Firebase Rules Testing

```bash
# Test by examining Firestore rules
# Verify:
# - Users can only read own profile
# - Users can only write to own profile
# - Items visible to all authenticated users
# - Claims only visible to involved parties
```

---

## Part 6: Accessibility Testing

- [ ] Text is readable (size, contrast)
- [ ] All buttons are tappable (min 48x48 dp)
- [ ] Form fields have labels
- [ ] Error messages are clear
- [ ] App works with system gestures

---

## Part 7: Edge Cases

### 7.1 Offline Behavior
- [ ] App doesn't crash without internet
- [ ] Shows "offline" message
- [ ] Retries when connection restored

### 7.2 Image Edge Cases
- [ ] Very large images (5+ MB)
- [ ] Very small images
- [ ] Corrupted images
- [ ] Screenshot images

### 7.3 Text Input Edge Cases
- [ ] Very long titles
- [ ] Special characters
- [ ] Emoji support
- [ ] Copy/paste functionality

### 7.4 Claim Edge Cases
- [ ] Multiple claims on one item
- [ ] Claim after item status change
- [ ] Modify claim after submission
- [ ] Delete claim

---

## Part 8: Sign-Off Checklist

### Functionality
- [ ] All screens load without errors
- [ ] All buttons work correctly
- [ ] Forms validate input properly
- [ ] Navigation works as expected
- [ ] Real-time updates function

### Performance
- [ ] App starts in < 5 seconds
- [ ] Scrolling is smooth
- [ ] No memory leaks
- [ ] Images load quickly

### Security
- [ ] Authentication works
- [ ] Authorization enforced
- [ ] Data is protected
- [ ] No sensitive data in logs

### Compatibility
- [ ] Android 12+ ✓
- [ ] iOS (if testing) ✓
- [ ] Multiple screen sizes ✓
- [ ] Poor network conditions ✓

---

## Test Results Summary

### Test Date: ___________
### Tester: ___________
### Overall Status: 
- [ ] Ready for Beta
- [ ] Ready for Release
- [ ] Issues Found - See Below

### Critical Issues Found:
1. _______________________
2. _______________________
3. _______________________

### Minor Issues Found:
1. _______________________
2. _______________________

---

## Automated Testing (Future)

```dart
// Example unit test
test('User creation stores correct data', () {
  final user = User(
    uid: '123',
    name: 'Test User',
    email: 'test@example.com',
    department: 'CSE',
    year: 2,
    createdAt: DateTime.now(),
  );
  
  expect(user.name, 'Test User');
  expect(user.email, 'test@example.com');
});

// Example widget test
testWidgets('Login button works', (WidgetTester tester) async {
  await tester.pumpWidget(const MyApp());
  
  await tester.tap(find.byKey(Key('login_button')));
  await tester.pumpAndSettle();
  
  expect(find.byType(HomeScreen), findsOneWidget);
});
```

---

**Created**: February 16, 2025  
**Status**: Complete Testing Guide Ready
