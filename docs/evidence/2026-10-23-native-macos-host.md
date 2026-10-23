# Native macOS host and on-screen gamepad

This is a work-in-progress host record. No IPA or phone gameplay is certified
by these checks. The full build and device gates remain pending.

## Host checks

- Apple Silicon, Xcode's iPhoneOS 27.1/macOS 27.0 SDKs, native LLVM 23.1.4,
  llvm-mingw 20260922 macOS universal with the documented arm64ec CRT rebuild,
  and pinned Rust 1.99.0.
- xtool 1.23.0 builds with the existing increased-memory-limit patch unchanged.
  Apple ID login succeeded after the upstream fixes for `-80045`/`-80046`.
- HostIOKit: 49 tests passed, including finger ownership, cancellation/reset,
  slot-one merging, physical stick preservation and the existing menu gate.
- The new overlay and dev touch driver passed iPhoneOS SDK type checking with
  API stubs for the rest of the app. This is not a full app compile.
- The fast host run passed all 591 Python tests. The remaining C portability
  changes were then checked with the complete WoW64 runner and the registry
  test. All passed. The full `pp test` run is in progress.
- The 13 import tests pass with explicit copy-path testing on APFS and a ZIP
  fixture for names that a case-insensitive host filesystem cannot represent.
- Wine's native host tools and the iOS ntdll/win32u archives built; win32u
  reported 46 compiled units and no failures. The ws4 link check passed.
  GStreamer and the remaining pipeline stages are still in progress.

Logs are under `.work/`: `touch-tests-final.log`, `game-import-tests.log`,
`quick-tests-macos.log`, `wow64-macos-tests.log`, `full-host-tests.log`,
`native-unix-headers.log`, `native-unix-gaps.log` and `native-gstreamer.log`.

## Device gates

USB discovery sees one iPhone on iOS 27.0.1; Developer Mode is enabled.
The initial install preflight stops at the phone's missing team registration.
The documented unsigned-release bootstrap must register it before dev signing.
No app was uninstalled, no game was played and no compatibility claim is made.

Pending: full host suite, native IPA build/verification, Linux CI, bootstrap
and dev installation, the Controllers settings page, and a Hollow Knight run
through first frame + 10 seconds with the on-screen controls.
