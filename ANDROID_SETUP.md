/// IMPORTANT: Android-specific configuration
/// Location: android/app/build.gradle

dependencies {
  // Google ML Kit dependencies
  implementation 'com.google.mlkit:image-labeling:17.0.7'
  implementation 'com.google.mlkit:image-labeling-custom:17.0.2'
  
  // Firebase
  implementation 'com.google.firebase:firebase-core'
  implementation 'com.google.firebase:firebase-auth'
  implementation 'com.google.firebase:firebase-firestore'
  implementation 'com.google.firebase:firebase-storage'
  implementation 'com.google.firebase:firebase-messaging'
}

/// AndroidManifest.xml requirements
/// Location: android/app/src/main/AndroidManifest.xml

<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.r_finder">

    <application
        android:label="R Finder"
        android:icon="@mipmap/ic_launcher"
        android:usesCleartextTraffic="false">
        
        <!-- Firebase meta-data -->
        <meta-data
            android:name="com.google.firebase.ml.vision.DEPENDENCIES"
            android:value="com.google.firebase.ml.vision.image_label.custom:0" />
            
    </application>

    <!-- Permissions -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    
</manifest>

Note: Run `flutterfire configure` to auto-generate proper configuration.
