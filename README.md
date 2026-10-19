# FolioInk Pro PDF

FolioInk Pro PDF is an Android PDF reader and annotation app built around a local HTML interface hosted in Android WebView. It bundles its PDF and export libraries with the app and includes a 64 MiB memory-mapped cache asset.

## Features

- Open PDF files from device storage, including multiple files in one session.
- View multi-page documents with page thumbnails and zoom controls.
- Annotate pages with a pen, highlighter, eraser, and text notes.
- Choose ink colors and stroke width; undo and redo strokes.
- Switch the paper and interface theme.
- Export an annotated document as a PDF to the device's Downloads folder.
- Open PDF documents from compatible Android apps using the system share/open flow.
- Use bundled PDF.js, jsPDF, and JSZip assets without downloading those libraries at runtime.
- Keep the interface responsive with hardware-accelerated WebView rendering. The host requests the display's highest available refresh rate where supported.

## Requirements

- Android Studio or a compatible Android SDK installation.
- JDK 17.
- Android SDK Platform 34.
- Gradle 8.5 (the repository does not currently include the Gradle wrapper).
- An Android device or emulator running Android 7.0 (API 24) or later.

## Build

From the repository root, build a debug APK with Gradle:

```powershell
gradle assembleDebug
```

The APK is written to:

```text
app/build/outputs/apk/debug/app-debug.apk
```

Build both configured variants with:

```powershell
gradle assembleDebug assembleRelease
```

The release build is currently configured to use the debug signing configuration. It is suitable for testing, not for publishing to an app store. Configure a private production signing key before distributing a production release; never commit signing credentials.

## Continuous integration

The GitHub Actions workflow in `.github/workflows/build-android-apk.yml` builds debug and release APKs on pushes to `main` or `master`, and when manually dispatched. It uses JDK 17 and Gradle 8.5, then uploads the APKs as the `FolioInk-Fat-ZeroLag-APK` workflow artifact.

To retrieve a build, open the repository's **Actions** tab, select a successful workflow run, and download its artifact.

## Project layout

```text
.
├── .github/workflows/       # Android APK build workflow
├── app/
│   ├── build.gradle         # Android application configuration
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── assets/
│       │   ├── cache/       # Pre-baked pages and 64 MiB mmap arena
│       │   ├── vendor/      # Locally bundled browser libraries
│       │   └── index.html   # Reader and annotation interface
│       └── java/            # Android WebView host and PDF bridge
├── build.gradle
├── gradle.properties
└── settings.gradle
```

## Large asset

`app/src/main/assets/cache/folioink_gpu_mmap_arena.bin` is a 64 MiB asset included in the APK and Git repository. GitHub accepts it but warns that it exceeds the recommended 50 MB per-file size. If this asset becomes difficult to distribute or update, consider Git LFS or downloading/generating it during the build instead.

## Notes

- The application ID is `com.folioink.pro.pdf`.
- The project targets Android API 34 and supports API 24 and later.
- The app requests internet access, and the HTML interface includes a PDF.js CDN fallback. The PDF.js, jsPDF, and JSZip libraries are also present in the bundled assets.
- The vendor directory contains third-party libraries. Review and retain their applicable license notices when distributing the app.
