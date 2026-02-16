/// IMPORTANT: iOS-specific configuration
/// Location: ios/Podfile

Add to Podfile:

post_install do |installer|
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)
    
    # ML Kit setup
    target.build_configurations.each do |config|
      config.build_settings['GCC_PREPROCESSOR_DEFINITIONS'] ||= [
        '$(inherited)',
        'FIREBASE_CORE_VERSION=<version>'
      ]
    end
  end
end

/// Info.plist configuration
/// Location: ios/Runner/Info.plist

Add to Info.plist:

<key>NSCameraUsageDescription</key>
<string>This app needs camera access to photograph items for reporting lost or found items.</string>

<key>NSPhotoLibraryUsageDescription</key>
<string>This app needs access to your photos to upload item images.</string>

<key>NSPhotoLibraryAddOnlyUsageDescription</key>
<string>This app needs permission to save photos.</string>

<key>UIApplicationSupportedInterfaceOrientations</key>
<array>
    <string>UIInterfaceOrientationPortrait</string>
    <string>UIInterfaceOrientationPortraitUpsideDown</string>
</array>

Note: Run `flutterfire configure` to auto-generate proper configuration.
