# AGENTS.md

## Cursor Cloud specific instructions

### Project overview
BaseJson is a Java/Android library (single Gradle module: `basejson`) providing fault-tolerant JSON parsing built on `org.json`. See `README.md` for API details.

### System requirements
- **JDK 11** — required by Android Gradle Plugin 7.1.0. Installed at `/usr/lib/jvm/java-11-openjdk-amd64`.
- **Android SDK (API 30)** — installed at `/opt/android-sdk`. `local.properties` points there via `sdk.dir`.
- Environment variables are set in `~/.bashrc`: `JAVA_HOME`, `ANDROID_HOME`, `ANDROID_SDK_ROOT`.

### Build commands
```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export ANDROID_HOME=/opt/android-sdk
./gradlew assembleDebug     # Build debug AAR
./gradlew assembleRelease   # Build release AAR
./gradlew lint              # Run Android Lint (pre-existing errors in repo — 1 NewApi error, 4 warnings)
```

### Key gotchas
- `gradlew` needs `chmod +x` after a fresh clone.
- `local.properties` with `sdk.dir=/opt/android-sdk` must exist at the project root. The update script creates it automatically.
- The Android SDK command-line tools require JDK 17+ to run `sdkmanager`, but the Gradle build must use JDK 11. Set `JAVA_HOME` to JDK 11 for builds.
- `./gradlew lint` exits non-zero due to a pre-existing `NewApi` error in `JsonListAdapter.java` (uses `Objects.equals` with `minSdkVersion 14`). This is not an environment issue.
- The repository has **no automated test suite** (`src/test/` and `src/androidTest/` do not exist). There is nothing to run via `./gradlew test`.
- Build output (AAR files) is at `basejson/build/outputs/aar/`.
