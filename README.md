# camera\_android

An Android implementation of [`camera`][1] built with the [Camera2 library][4].

## About this fork

This repository contains only the `camera_android` package extracted from the Flutter packages
monorepo. It is based on the official `camera_android-v0.10.10+3` tag and backports the
capture-session teardown race guards from `flutter/packages#12224` for Flutter 3.27.x projects.

The backport keeps the original package version and Flutter/Dart compatibility constraints. See
[`BACKPORT.md`](BACKPORT.md) for the exact upstream commits, local adaptations, and validation
scope.

## Usage

As of `camera: ^0.11.0`, add this package directly to select the Camera2 implementation instead of
[`camera_android_camerax`][3]:

```yaml
dependencies:
  camera: 0.11.2
  camera_android:
    git:
      url: https://github.com/gcc8080/camera_android.git
      ref: main
```

For production applications, replace the branch name with a tested commit SHA.

## Limitation of testing video recording on emulators
`MediaRecorder` does not work properly on emulators, as stated in [the documentation][5]. Specifically,
when recording a video with sound enabled and trying to play it back, the duration won't be correct and
you will only see the first frame.

[1]: https://pub.dev/packages/camera
[2]: https://flutter.dev/to/endorsed-federated-plugin
[3]: https://pub.dev/packages/camera_android_camerax
[4]: https://developer.android.com/media/camera/camera2
[5]: https://developer.android.com/reference/android/media/MediaRecorder
