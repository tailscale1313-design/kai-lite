# Kai Lite — Android 8 derivative

Kai Lite is an unofficial lightweight derivative of [Kai 9000](https://github.com/SimonSchubert/Kai). It preserves the main chat, memory, editable soul/system prompt, Linux sandbox/terminal, networking-oriented tools, SSH support, heartbeat settings, and LiteRT local inference path.

## What was reduced

The Android manifest no longer requests calendar or alarm permissions. The FOSS overlay no longer requests SMS, APK-install, or notification-listener capabilities. Heartbeat is retained but disabled by default so the app does not keep a background service active until you opt in from settings.

The app uses the separate Android application ID `com.inspiredandroid.kailite` and is displayed as **Kai Lite**. The source remains under the upstream Apache-2.0 license; keep `LICENSE.txt`, upstream attribution, and this modification notice when redistributing.

## Android 8 target

The upstream project already uses `minSdk = 26`, which is Android 8.0/API 26. The LiteRT runtime and native model execution remain part of the Android build. The APK still needs an ARM64 phone for the Linux sandbox and should have substantial free storage and RAM for local inference.

## Offline local model

Download or import a compatible `.litertlm` model once while online. After the model is present on the device, local chat can run without an API key or internet connection. The smallest catalog model is **Qwen3 0.6B** at approximately 614 MB and is recommended for Android 8-class hardware. The app also preserves the import path for other compatible LiteRT models.

A specific model should not be advertised as “uncensored” without checking its model card, license, and behavior. Kai Lite supports user-supplied compatible local models; the app does not require cloud moderation for local inference, but local model licenses and safety characteristics remain the responsibility of the model provider and user.

## Build an installable APK without a PC

The repository includes `.github/workflows/build-kai-lite.yml`. Put this source tree in a GitHub repository, open the **Actions** tab, run **Build Kai Lite APK**, and download the generated `kai-lite-foss-release` artifact from the completed workflow. The workflow installs Java 21, the Android SDK platform/build tools, and the NDK, then runs:

```bash
./gradlew :androidApp:assembleFossRelease
```

The APK output is under:

```text
androidApp/build/outputs/apk/foss/release/
```

Because no signing key is supplied by default, the release build uses the Gradle debug key for installation/testing. For a permanent redistributable APK, configure `KEYSTORE_FILE`, `KEYSTORE_PASSWORD`, `KEY_ALIAS`, and the corresponding GitHub Actions secrets, or sign the artifact locally. Android may require enabling installation from the browser or file manager used to download it.

## Security scope

The networking and red-team functionality is intended only for devices and networks that you own or are explicitly authorized to test. Keep use limited to inventory, service checks, SSH administration, log analysis, reporting, and remediation guidance.
