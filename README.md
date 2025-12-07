# Driver-Detector

A Flutter mobile application for detecting drivers, tracking car movement, and detecting crashes using advanced sensor and machine learning technology.

## Features

- **Driver Detection**: Identifies and recognizes drivers using camera and AI
- **Car Movement Tracking**: Monitors vehicle movement patterns and behavior
- **Crash Detection**: Real-time crash detection using accelerometer and gyroscope sensors

## Codemagic CI/CD Setup

This repository is configured with Codemagic for continuous integration and deployment. The CI/CD pipeline includes multiple workflows:

### Available Workflows

1. **driver-detector-test** - Full test suite for all features
   - Runs comprehensive tests for driver detection, car movement, and crash detection
   - Builds Android APK and iOS app
   - Generates code coverage reports

2. **driver-detection-feature** - Driver detection specific testing
   - Unit tests for driver detection module
   - Integration tests for driver recognition

3. **car-movement-feature** - Car movement specific testing
   - Unit tests for movement tracking
   - Integration tests for motion detection

4. **crash-detection-feature** - Crash detection specific testing
   - Unit tests for crash detection algorithms
   - Integration tests for sensor data processing

5. **android-release** - Android production build
   - Builds signed Android App Bundle (AAB)
   - Publishes to Google Play Console (internal track)

6. **ios-release** - iOS production build
   - Builds signed iOS IPA
   - Publishes to TestFlight

### Setting Up Codemagic

1. **Connect Repository**: Link this repository to your Codemagic account
2. **Configure Environment Variables**:
   - Set up `app_store_credentials` group for iOS builds
   - Set up `google_play_credentials` group for Android builds
   - Add `ANDROID_KEYSTORE`, `ANDROID_KEYSTORE_PASSWORD`, `ANDROID_KEY_PASSWORD`, `ANDROID_KEY_ALIAS`
   - Add `APP_STORE_CONNECT_PRIVATE_KEY`, `APP_STORE_CONNECT_KEY_IDENTIFIER`, `APP_STORE_CONNECT_ISSUER_ID`
   - Update email addresses in the workflows

3. **Trigger Builds**:
   - Builds trigger automatically on push to main branch
   - Manual builds can be triggered from Codemagic dashboard
   - Feature-specific workflows can be run for targeted testing

### Testing Strategy

The project follows a comprehensive testing approach:

- **Unit Tests**: Test individual components and functions
- **Integration Tests**: Test feature workflows end-to-end
- **Widget Tests**: Test UI components (Flutter)
- **Coverage**: Aim for >80% code coverage

### Build Artifacts

Each successful build produces:
- Android: APK and AAB files with ProGuard mapping
- iOS: IPA file with debug symbols
- Test reports and coverage data

## Getting Started

### Prerequisites

- Flutter SDK (stable channel)
- Android Studio / Xcode
- Git

### Installation

```bash
# Clone the repository
git clone https://github.com/kermitapan/Driver-Detector.git

# Navigate to project directory
cd Driver-Detector

# Get dependencies
flutter pub get

# Run the app
flutter run
```

### Running Tests

```bash
# Run all tests
flutter test

# Run specific feature tests
flutter test test/driver_detection_test.dart
flutter test test/car_movement_test.dart
flutter test test/crash_detection_test.dart

# Run with coverage
flutter test --coverage
```

## Project Structure

```
Driver-Detector/
├── lib/
│   ├── features/
│   │   ├── driver_detection/
│   │   ├── car_movement/
│   │   └── crash_detection/
│   └── main.dart
├── test/
│   ├── driver_detection_test.dart
│   ├── car_movement_test.dart
│   └── crash_detection_test.dart
├── integration_test/
│   ├── driver_detection_integration_test.dart
│   ├── car_movement_integration_test.dart
│   └── crash_detection_integration_test.dart
└── codemagic.yaml
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Run tests locally
5. Push to Codemagic for CI testing
6. Create a Pull Request

## License

See LICENSE file for details.