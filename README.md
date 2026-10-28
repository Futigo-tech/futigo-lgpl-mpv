# futigo-lgpl-mpv

Complete corresponding source code of **mpv (libmpv)** as shipped inside the **Futigo** mobile app (iOS and Android), starting with app version 2.0.0.

License: **LGPL-2.1-or-later**.

mpv is built by media-kit with `-Dgpl=false`, so libmpv is under the GNU LGPL version 2.1 or later. mpv's `Copyright` file is included.

This repository contains only unmodified upstream source code, the upstream patches applied by the media-kit project, the media-kit build scripts, and the license texts. It contains no Futigo code.

## Source files

All files are attached to the GitHub release **`app-2.0.0-sources`** of this repository, together with `SHA256SUMS`.

| Component / version | Platform | License | Original source URL | Release file | SHA-256 |
|---|---|---|---|---|---|
| mpv 0.36.0 (tag v0.36.0, commit f4210f84906c3b00a65fba198c8127b6757b9350) | iOS | LGPL-2.1-or-later | https://github.com/mpv-player/mpv/archive/refs/tags/v0.36.0.tar.gz | `mpv-0.36.0.tar.gz` | `29abc44f8ebee013bb2f9fe14d80b30db19b534c679056e4851ceadf5a5e8bf6` |
| mpv 0.36.0-549-g78d43740f5 (commit 78d43740f52db817d98bcf24fb30a76ab6fa13ff) | Android | LGPL-2.1-or-later | https://github.com/mpv-player/mpv/archive/78d43740f52db817d98bcf24fb30a76ab6fa13ff.tar.gz | `mpv-78d43740f52db817d98bcf24fb30a76ab6fa13ff.tar.gz` | `678889728708141e97d283b2a7ef9953aa3102127a4c2aacefaf1adcb211915d` |

- `mpv-0.36.0.tar.gz`: SHA-256 equals `downloads.lock` of libmpv-darwin-build v0.6.0.
- `mpv-78d43740f52db817d98bcf24fb30a76ab6fa13ff.tar.gz`: the Android build resets a clone of github.com/mpv-player/mpv to this commit; GitHub archive of that exact commit (commit id checked in the archive).

## Build scripts (how the binaries were made)

| Build repository | Platform | Original URL | Release file | SHA-256 |
|---|---|---|---|---|
| media-kit/libmpv-darwin-build, tag v0.6.0 (commit 4286f5557bdccc0747030e3c376ce5cd160a96a0) | iOS | https://github.com/media-kit/libmpv-darwin-build/archive/refs/tags/v0.6.0.tar.gz | `libmpv-darwin-build-0.6.0.tar.gz` | `afade6a15e001700c6d92860bb33eba43020450001e7464595717ab9a222d28a` |
| media-kit/libmpv-android-video-build, tag v1.1.7 (commit fe8c3ac1a91c09aa6fb1deccbc833f1bafa54768) | Android | https://github.com/media-kit/libmpv-android-video-build/archive/refs/tags/v1.1.7.tar.gz | `libmpv-android-video-build-1.1.7.tar.gz` | `6624ea32232f7a8d6d07ab7a7bc7b42c16f85fc50c4e3ecfd174b52636d9252a` |

Where it is used: libmpv plays video in the app. iOS: Mpv.framework. Android: `libmpv.so`.

The app ships the **prebuilt media-kit binaries unchanged**:

- iOS: Flutter package `media_kit_libs_ios_video` 1.1.4, which downloads `libmpv-xcframeworks_v0.6.0_ios-universal-video-default.tar.gz` built by libmpv-darwin-build v0.6.0 (variant `video`, flavor `default`). Exact source versions and checksums are in `downloads.lock` of that build repository.
- Android: Flutter package `media_kit_libs_android_video` 1.3.8, which downloads `default-<abi>.jar` from the v1.1.7 release of libmpv-android-video-build, built by libmpv-android-video-build v1.1.7 (`buildscripts/include/depinfo.sh`, `buildscripts/include/download-deps.sh`, `buildscripts/flavors/default.sh`).

## Patches

Copies are in `patches/` of this repository and in the release:

- `patches/ios/mpv-fix-missing-objc.patch`
- `patches/android/mpv_lavc_set_java_vm.patch`

iOS: `mpv-fix-missing-objc.patch` applied by `scripts/mpv/build.sh` of libmpv-darwin-build (`mpv-remove-libass.patch` is applied only to the audio variant, which the app does not use). Android: `buildscripts/patches/mpv/mpv_lavc_set_java_vm.patch` applied by `buildscripts/patch.sh`. Release files: `ios-mpv-fix-missing-objc.patch`, `android-mpv-mpv_lavc_set_java_vm.patch`.

## How to rebuild

iOS (macOS host):

```sh
tar -xzf libmpv-darwin-build-0.6.0.tar.gz
cd libmpv-darwin-build-0.6.0
brew install cmake golang meson ninja
VERSION=v0.6.0 make
# result: build/output/libmpv-xcframeworks_v0.6.0_ios-universal-video-default.tar.gz
```

The build downloads the sources listed in `downloads.lock` (the same files as in this release) and verifies their SHA-256. To build offline, place the release tarballs where the build expects its downloads.

Android (Linux host, see the build repository's own workflow):

```sh
tar -xzf libmpv-android-video-build-1.1.7.tar.gz
cd libmpv-android-video-build-1.1.7/buildscripts
./bundle_default.sh   # download.sh + patch.sh + build.sh with flavors/default.sh
```

To replace the library in the app with a modified version, rebuild it and put the resulting framework / `libmpv.so` in place of the one in the app (iOS: the dynamic framework inside `Futigo.app/Frameworks`; Android: `lib/<abi>/libmpv.so` in the APK).

## Source on request

You can also ask for a copy of this source code: **info@futigo.app**. This offer is valid for at least 3 years from the last distribution of the app version that contains the library.

## Licenses

- [`licenses/LGPL-2.1.txt`](licenses/LGPL-2.1.txt)
- [`licenses/mpv-Copyright.txt`](licenses/mpv-Copyright.txt)

---

## Po polsku (skrót)

To repozytorium zawiera pełny kod źródłowy biblioteki mpv (libmpv) (LGPL-2.1-or-later) w wersjach używanych w aplikacji Futigo (iOS i Android), od wersji 2.0.0: niezmienione źródła autorów, poprawki (patche) projektu media-kit i jego skrypty budowania. Aplikacja zawiera gotowe, niezmienione pliki binarne media-kit. Pliki są w wydaniu (release) `app-2.0.0-sources`, z sumami kontrolnymi `SHA256SUMS`. Kod źródłowy można też otrzymać na prośbę: **info@futigo.app** (co najmniej 3 lata od ostatniej dystrybucji danej wersji aplikacji).
