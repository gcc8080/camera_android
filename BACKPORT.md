# Capture-session race backport

## Baseline

- Upstream repository: `flutter/packages`
- Package path: `packages/camera/camera_android`
- Upstream tag: `camera_android-v0.10.10+3`
- Baseline commit: `b2ce3b02a27b6e055fb9e2c11fdc30fd2c962669`
- Package version retained: `0.10.10+3`
- Supported baseline retained: Dart 3.6 / Flutter 3.27, Android minSdk 21

This compatibility baseline is intentional for applications fixed on Flutter 3.27.4. No files
from other packages in the Flutter packages monorepo are included.

## Backported fix

- Upstream pull request: <https://github.com/flutter/packages/pull/12224>
- Upstream fix commit: `a6c8a09b12560bb760ae6fc3db6d06cb5a7a9ee6`
- Backport date: 2026-08-12

The backport:

- marks `captureSession` as `volatile`;
- snapshots the current session before asynchronous camera operations;
- clears the shared field before closing the captured session;
- ignores still-capture callbacks from stale sessions;
- handles null, closed, and replaced sessions during preview, focus, still capture, and recording;
- adds regression tests for teardown, replacement, null-session, and stale-session paths.

## Compatibility adaptations

The upstream fix targets `camera_android 0.10.11+1`, whose current toolchain requirements are newer
than Flutter 3.27. The following unrelated newer behavior was deliberately not brought into this
backport:

- AE/AF trigger-reset callbacks introduced after `0.10.10+3`;
- newer Kotlin, Gradle, Java, compile SDK, and minSdk changes;
- later Pigeon-generated API changes;
- package version and SDK-constraint changes.

Where those newer callbacks conflicted, the `0.10.10+3` callback behavior was retained while all
accesses were switched to the stable local `CameraCaptureSession` snapshot.

## Validation

The source tree should be validated with Flutter 3.27.4 before release:

```bash
flutter pub get
flutter analyze
flutter test
cd android
gradle test
```

The Android regression suite includes deterministic checks that `closeCaptureSession()` clears the
shared field before closing, closes only its captured snapshot, preserves a replacement assigned by
a close callback, and safely handles an already-null session.
