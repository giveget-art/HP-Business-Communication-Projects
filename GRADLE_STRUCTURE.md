# HP Communication - Android Project Structure (Gradle Format)

## Multi-Module Gradle Project

```
HP-Business-Communication-Projects/
├── Root Module (Project Level)
│   ├── build.gradle                 # AGP: 8.2.0, Gradle: 8.14
│   ├── settings.gradle              # Multi-module config
│   └── gradle.properties            # Shared settings
│
├── :app                             # Main Application
│   ├── build.gradle
│   ├── src/main/AndroidManifest.xml
│   ├── src/main/java/
│   ├── src/main/res/
│   └── deployment/
│
├── :common                          # Shared Library
│   ├── build.gradle
│   ├── src/main/AndroidManifest.xml
│   ├── src/main/java/com/hp/communication/common/
│   └── src/main/res/
│
└── :ui                              # UI Components Library
    ├── build.gradle
    ├── src/main/AndroidManifest.xml
    ├── src/main/java/com/hp/communication/ui/
    ├── src/main/res/
    └── deployment/
```

## Build Configuration

### Gradle Versions
- **Gradle**: 8.14
- **Android Gradle Plugin**: 8.2.0
- **Compile SDK**: 34
- **Min SDK**: 21
- **Target SDK**: 34
- **Java Version**: 11

### Dependencies Management
- **AndroidX**: 1.6.1
- **Material**: 1.11.0
- **ConstraintLayout**: 2.1.4
- **Testing**: JUnit 4, Espresso, AndroidTest

### Module Dependencies
```
app ← ui ← common
app ← common
```

## Building the Project

### Build APK
```bash
./gradlew clean build
```

### Build Release APK
```bash
./gradlew clean assembleRelease
```

### Run Tests
```bash
./gradlew test
./gradlew connectedAndroidTest
```

### Clean Build
```bash
./gradlew clean
```

## Deployment

See individual module deployment guides:
- `app/deployment/README.md`
- `ui/deployment/README.md`
