# Driver-Detector

iOS app for driver detection.

## Building for Real Device with Codemagic

This project is configured to build IPA files for installation on real iOS devices using Codemagic CI/CD.

### Prerequisites

1. **Apple Developer Account** (paid membership required - $99/year)
2. **App Store Connect API Key** from Apple

### Setup Instructions

#### Step 1: Get App Store Connect API Key

1. Go to [App Store Connect](https://appstoreconnect.apple.com)
2. Navigate to **Users and Access** → **Keys** tab
3. Click **Generate API Key** (or use existing)
4. Download the `.p8` file
5. Note your **Issuer ID** and **Key ID**

#### Step 2: Configure Codemagic

1. Go to your Codemagic project settings
2. Navigate to **Environment variables**
3. Add these variables as **secure** environment variables:
   - `APP_STORE_CONNECT_ISSUER_ID` - Your Issuer ID
   - `APP_STORE_CONNECT_KEY_IDENTIFIER` - Your Key ID
   - `APP_STORE_CONNECT_PRIVATE_KEY` - Contents of your .p8 file (paste the entire file content)

4. *Optional*: Update the email address in `codemagic.yaml` under `publishing.email.recipients`

#### Step 3: Register Your Device

1. Go to [Apple Developer Portal](https://developer.apple.com/account/resources/devices/list)
2. Click **+** to add a new device
3. Enter device name and UDID
   - Get UDID from Finder when iPhone is connected (click on device info)
   - Or from Settings → General → About (tap on "Serial Number" to show UDID)
4. Save the device

#### Step 4: Build in Codemagic

1. In Codemagic, start a new build
2. Codemagic will automatically:
   - Create/update provisioning profiles with your registered devices
   - Sign the app
   - Build the IPA file

#### Step 5: Install on Your iPhone

**Option A: Direct Install from Windows**
1. Download the IPA from Codemagic artifacts
2. Use **3uTools** (free Windows app):
   - Download from [3u.com](https://www.3u.com/)
   - Connect your iPhone
   - Go to "Apps" → "Install" → Select the IPA file

**Option B: From Mac**
- **Apple Configurator 2** (free from Mac App Store)
- **Xcode Devices Window** (Window → Devices and Simulators → drag IPA to device)

### Bundle ID

The app uses bundle identifier: `com.example.DriverDetector`

**To change it:**
1. Update the `BUNDLE_ID` variable in `codemagic.yaml`
2. Update `bundle_identifier` under `ios_signing` in `codemagic.yaml`
3. Update in Xcode project settings (or edit `project.pbxproj`)

### Troubleshooting

**"No profiles for bundle identifier"**
- Make sure your Apple Developer account has an active paid membership
- Check that the bundle ID in `codemagic.yaml` matches your App ID in Apple Developer Portal
- Verify App Store Connect API credentials are correct in Codemagic environment variables

**"Device not in provisioning profile"**
- Add your device UDID in Apple Developer Portal
- Trigger a new build (Codemagic will automatically update the profile)

**Build fails at signing step**
- Double-check the API key, issuer ID, and key identifier in Codemagic environment variables
- Ensure the .p8 file content was copied correctly (including BEGIN/END lines)

### What the App Does

Simple demo iOS app that displays "Driver Detector" text on the screen. Replace with your actual driver detection logic.

### Development Without Codemagic

To build locally on your Mac:

```bash
xcodebuild -project DriverDetector.xcodeproj \
  -scheme DriverDetector \
  -configuration Debug \
  -sdk iphoneos \
  -derivedDataPath build
```