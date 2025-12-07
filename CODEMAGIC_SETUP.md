# Codemagic Setup Guide for Driver Detector

This guide will walk you through setting up Codemagic CI/CD for the Driver Detector application.

## Prerequisites

- GitHub account with access to this repository
- Codemagic account (sign up at https://codemagic.io)
- Apple Developer account (for iOS builds)
- Google Play Developer account (for Android builds)

## Step 1: Connect Repository to Codemagic

1. Log in to [Codemagic](https://codemagic.io)
2. Click "Add application"
3. Select "GitHub" as your repository provider
4. Authorize Codemagic to access your GitHub account
5. Select the `kermitapan/Driver-Detector` repository
6. Click "Finish: Add application"

## Step 2: Configure Android Signing

### Generate Android Keystore (if you don't have one)

```bash
keytool -genkey -v -keystore driver-detector.jks -keyalg RSA -keysize 2048 -validity 10000 -alias driver-detector
```

### Add to Codemagic

1. In Codemagic, go to your app settings
2. Navigate to "Environment variables"
3. Create a new group called `google_play_credentials`
4. Add the following variables:
   - `ANDROID_KEYSTORE`: Convert your keystore to base64
     ```bash
     cat driver-detector.jks | base64
     ```
   - `ANDROID_KEYSTORE_PASSWORD`: Your keystore password
   - `ANDROID_KEY_PASSWORD`: Your key password
   - `ANDROID_KEY_ALIAS`: Your key alias (e.g., "driver-detector")
   - `GCLOUD_SERVICE_ACCOUNT_CREDENTIALS`: Your Google Play service account JSON

### Google Play Service Account

1. Go to [Google Play Console](https://play.google.com/console)
2. Navigate to "Setup" > "API access"
3. Create a new service account or use existing
4. Grant necessary permissions
5. Download the JSON key file
6. Copy the entire JSON content to `GCLOUD_SERVICE_ACCOUNT_CREDENTIALS`

## Step 3: Configure iOS Signing

### Prerequisites

- Apple Developer account
- App Store Connect API Key

### Generate App Store Connect API Key

1. Go to [App Store Connect](https://appstoreconnect.apple.com)
2. Navigate to "Users and Access" > "Keys"
3. Click "Generate API Key" or use existing
4. Download the `.p8` key file
5. Note the Key ID and Issuer ID

### Add to Codemagic

1. In Codemagic, go to your app settings
2. Navigate to "Environment variables"
3. Create a new group called `app_store_credentials`
4. Add the following variables:
   - `APP_STORE_CONNECT_PRIVATE_KEY`: Content of your `.p8` file
   - `APP_STORE_CONNECT_KEY_IDENTIFIER`: Your Key ID
   - `APP_STORE_CONNECT_ISSUER_ID`: Your Issuer ID
   - `CERTIFICATE_PRIVATE_KEY`: Your certificate private key (if using manual signing)

### Automatic Code Signing (Recommended)

Codemagic can automatically fetch and manage your signing certificates:

1. The workflow will automatically fetch signing files using:
   ```yaml
   app-store-connect fetch-signing-files "$BUNDLE_ID" --type IOS_APP_STORE --create
   ```
2. Make sure your App Store Connect API Key has proper permissions

## Step 4: Configure Workflows

The `codemagic.yaml` file in the repository root defines all workflows. You can customize:

### Update Email Notifications

Replace `user@example.com` with your actual email address in all workflows:

```yaml
publishing:
  email:
    recipients:
      - your-email@example.com
```

### Adjust Build Settings

Modify instance types, Flutter versions, or timeout durations as needed:

```yaml
instance_type: mac_mini_m1  # or mac_pro, linux, linux_x2
max_build_duration: 60      # minutes
flutter: stable             # or specific version like "3.10.0"
```

## Step 5: Available Workflows

### 1. driver-detector-test (Main Test Suite)
- **Purpose**: Comprehensive testing of all features
- **Triggers**: Manual or on push to main branch
- **Outputs**: Test reports, Android APK, iOS IPA
- **Use when**: Testing complete application

### 2. driver-detection-feature
- **Purpose**: Test driver detection feature only
- **Triggers**: Manual
- **Use when**: Working on driver detection module

### 3. car-movement-feature
- **Purpose**: Test car movement feature only
- **Triggers**: Manual
- **Use when**: Working on car movement module

### 4. crash-detection-feature
- **Purpose**: Test crash detection feature only
- **Triggers**: Manual
- **Use when**: Working on crash detection module

### 5. android-release
- **Purpose**: Build and publish Android release
- **Triggers**: Manual or tag-based
- **Outputs**: Signed AAB published to Google Play (internal track)

### 6. ios-release
- **Purpose**: Build and publish iOS release
- **Triggers**: Manual or tag-based
- **Outputs**: Signed IPA published to TestFlight

## Step 6: Triggering Builds

### Manual Builds

1. Go to Codemagic dashboard
2. Select your app
3. Click "Start new build"
4. Choose the workflow
5. Select branch
6. Click "Start build"

### Automatic Builds

Configure webhooks in the workflow file:

```yaml
triggering:
  events:
    - push
    - pull_request
  branch_patterns:
    - pattern: 'main'
      include: true
      source: true
```

## Step 7: Monitoring Builds

### Build Status

- **Green**: Build passed successfully
- **Red**: Build failed
- **Yellow**: Build in progress

### Build Logs

- Click on any build to view detailed logs
- Each step shows its execution time and output
- Download artifacts from the Artifacts tab

### Notifications

You'll receive email notifications for:
- Build failures (all workflows)
- Build success (release workflows only)

## Step 8: Best Practices

### For Development

1. Run feature-specific workflows during development
2. Use `driver-detector-test` before merging to main
3. Only use release workflows for production builds

### For Testing

1. Ensure all unit tests pass locally before pushing
2. Use code coverage to maintain quality
3. Run integration tests on Codemagic

### For Releases

1. Use semantic versioning for tags
2. Always test with `driver-detector-test` first
3. Use `android-release` and `ios-release` for production
4. Review build artifacts before publishing

## Troubleshooting

### Common Issues

**Build fails with "Flutter not found"**
- Ensure Flutter version is specified correctly in workflow
- Check that Flutter SDK is installed on build machine

**Signing fails for iOS**
- Verify App Store Connect API key permissions
- Check that Bundle ID matches in Xcode and workflow
- Ensure certificates are not expired

**Android signing fails**
- Verify keystore file is properly base64 encoded
- Check that passwords and alias are correct
- Ensure keystore is not expired

**Tests fail**
- Check test files exist in the repository
- Verify Flutter packages are up to date
- Review test logs for specific errors

### Getting Help

- Codemagic Documentation: https://docs.codemagic.io/
- Codemagic Support: support@codemagic.io
- GitHub Issues: Report issues in this repository

## Next Steps

1. ✅ Complete this setup guide
2. ⬜ Create the Flutter app structure
3. ⬜ Implement driver detection feature
4. ⬜ Implement car movement feature
5. ⬜ Implement crash detection feature
6. ⬜ Write comprehensive tests
7. ⬜ Trigger first successful build
8. ⬜ Deploy to TestFlight/Internal Testing

## Additional Resources

- [Codemagic YAML Documentation](https://docs.codemagic.io/yaml/yaml-getting-started/)
- [Flutter Testing Guide](https://flutter.dev/docs/testing)
- [iOS Code Signing Guide](https://docs.codemagic.io/code-signing/ios-code-signing/)
- [Android Code Signing Guide](https://docs.codemagic.io/code-signing/android-code-signing/)
