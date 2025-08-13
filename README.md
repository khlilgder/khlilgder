# PS3 iOS Emulator Starter (XcodeGen)

This is a **starter scaffold** to kick off an iOS PS3-emulator app project. It does **not** include RPCS3 or any copyrighted code. It gives you:
- iOS app target (SwiftUI) with a Metal renderer placeholder
- C++ core library target (`EmuCore`) for porting/embedding your emulator core
- Objective-C++ bridge target (`EmuBridge`) to talk between Swift and C++
- Game controller input stub
- Xcode project generation using **XcodeGen**

> ⚠️ **Reality check**: PS3 emulation on iOS is extremely challenging (JIT, performance, GPU). This scaffold is for experimentation and learning and won't run PS3 games by itself.

## Prerequisites
- macOS + Xcode (latest)
- Homebrew
- XcodeGen

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install xcodegen
```

## Generate Xcode project
From the repo root:
```bash
xcodegen generate
open PS3iOS-Starter.xcodeproj
```

## Build & Run
1. Select a **real device** (iPhone/iPad) and your Team in Signing (or disable signing to export unsigned IPA via Archive).
2. Build → Run. You should see a blank Metal view with a simple frame counter in the log.

## Where to put your emulator core
- Add/clone your core into `ThirdParty/` (e.g., `ThirdParty/rpcs3`).
- Wrap the parts you need in `Sources/EmuCore/` (C++). Keep platform code minimal here.
- Expose C-interface (or thin ObjC++ wrappers) in `Sources/EmuBridge/`.
- Call into the bridge from Swift UI/renderer layer.

## MoltenVK / Vulkan
If you want Vulkan:
- Use MoltenVK (Vulkan-on-Metal). Add it as a package/binary and link in `project.yml`.
- Or rewrite critical GPU paths using native Metal (the sample shows a Metal renderer stub).

## JIT note
JIT is restricted on iOS. For any meaningful performance, you'll likely need:
- Developer provisioning and specific entitlements, and/or
- Alternative approaches (interpreter, offline recompiler), with massive perf trade-offs.

## Folder structure
```
Sources/
  PS3iOS/            # SwiftUI app + Metal renderer + input
  EmuCore/           # C++ core stubs (place/port your emulator here)
  EmuBridge/         # ObjC++ bridge exposing C/ObjC funcs to Swift
Resources/
Scripts/
ThirdParty/          # put external deps here (e.g., RPCS3, MoltenVK)
project.yml          # XcodeGen config (app + libs)
```

Good luck! Build iteratively: get the app booting → render something → input → load minimal test → iterate.
