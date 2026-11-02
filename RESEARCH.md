# Kai Lite research notes

## Upstream reference

Repository: https://github.com/SimonSchubert/Kai

The upstream README describes Kai 9000 as an open-source persistent-memory AI assistant for Android, iOS, desktop, and web. Relevant features include a customizable system prompt, memory, heartbeat, tool execution, MCP support, a Linux sandbox on Android, SSH, text-to-speech, and on-device LiteRT inference. The repository is Apache-2.0 licensed. Apache redistribution requires retaining the license and notices and marking modified files; the Apache license does not grant the original product trademark.

The upstream Android manifest currently declares INTERNET, ACCESS_LOCAL_NETWORK, READ_CALENDAR, WRITE_CALENDAR, POST_NOTIFICATIONS, SET_ALARM, FOREGROUND_SERVICE, and FOREGROUND_SERVICE_DATA_SYNC. It also declares a persistent `DaemonService` foreground service and a model download foreground service. The FOSS flavor adds SMS permissions, REQUEST_INSTALL_PACKAGES, and notification-listener service wiring.

## Offline model path

Kai's `LocalModelCatalog.kt` contains verified LiteRT model entries downloaded from immutable Hugging Face commits with SHA-256 checks. The smallest catalog entry observed is Qwen3 0.6B at about 614 MB; LFM2.5 1.2B is about 736 MB. Kai also supports importing `.litertlm` files. The user asked for a local offline model; the build should retain the existing LiteRT runtime and recommend the smallest compatible model on Android 8-class hardware. Avoid claiming that a model is "uncensored" unless its model card and license explicitly support that characterization; user-imported compatible models can remain an option.

## Build facts

Upstream uses compileSdk 37, minSdk 26, targetSdk 37, AGP 9.4.1, Kotlin 2.4.21, Compose Multiplatform 1.12.2, and Gradle 9.8.2. The current sandbox has Java 21 but its Java installation lacks the compiler capability required by Gradle (`JAVA_COMPILER`), and it has no Android SDK/ADB detected. The first native build attempt failed at `compileFossReleaseJavaWithJavac` for that reason.

## Official Expo/EAS references used earlier

- https://docs.expo.dev/build-reference/apk/ — configure `android.buildType` as `apk` or internal distribution for direct installation.
- https://docs.expo.dev/build/introduction/ — EAS Build is a hosted service that builds Android binaries and manages signing credentials if desired.
- https://docs.expo.dev/build-reference/local-builds/ — local EAS builds require Android SDK and NDK and use `eas build --local`.
