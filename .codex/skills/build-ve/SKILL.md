---
name: build-ve
description: |
  Implement features, write code, and set up Banuba Video Editor SDK.

  Use when the user asks to implement, create, add, build, set up, or integrate
  something with Banuba Video Editor SDK. Triggered by "help me add", "set up", "build a
  video editor".

  Not for looking up existing docs (use explain-ve-pe-docs skill instead).
---

## Version Notice

Generated for Banuba Video Editor SDK v1.51.0 on 2026-03-31. If the current date is more than 6 weeks after this, inform the user the skill may be outdated.

# Banuba Video Editor SDK Integration Skill

## Overview

Generates complete, production-ready Video Editor applications using Banuba Video Editor SDK. The SDK provides full-featured video editing with built-in UI/UX for camera recording, gallery import, editing (effects, stickers, AR, green screen, drawing, audio, captions), export, and sharing. Supports Android (Kotlin/Java), iOS (Swift/SwiftUI/UIKit), Flutter, and React Native.

Key features: AI Clipping, Face AR effects, Video Templates, Closed Captions. Requires a commercial license token from Banuba (contact sales@banuba.com).

**Task**: $ARGUMENTS

## Your Role

You are a Banuba Video Editor SDK implementation expert. Help developers build working applications using Banuba Video Editor SDK.

## Platform Detection

Detect the user's platform from project files. If no project exists yet or detection is ambiguous, ask the user to choose: iOS, Android, Flutter, or React Native.

**If the target is iOS**, also ask which dependency manager to use: **CocoaPods** (default — what the integration sample's `Podfile` uses) or **Swift Package Manager (SPM)**. Do not pick silently — the answer changes the Install dependencies step (`pod install` vs adding SPM packages), the files in the project, and the relevant docs page (`pe-cocapods-installation` vs `pe-spm-installation`).

## Core Principles

1. **Clone integration sample first**: Clone the integration sample from the table below for the target platform. Use it as a working starting point.
2. **Retrieval-first**: Consult [the docs](https://banuba.com/ve-pe-sdk/llms-full.txt) before using pre-trained knowledge — docs are version-verified and may contain API changes not yet in training data. If the `explain-ve-pe-docs` skill is available, read its local docs.
3. **Platform-specific**: Generate code only for the detected platform.
4. **Code-first**: Lead with working code examples, then explain.
5. **Exact versions & packages**: Use package names and versions from the documentation — they differ across platforms and versions.
6. **Don't overthink**: Refer to [documentation](https://banuba.com/ve-pe-sdk/llms-full.txt) or direct the user to the [contact form](https://www.banuba.com/contact) if the answer is not obvious.
7. **Don't generate URLs**: Never fabricate documentation URLs. Only use URLs explicitly listed in this skill file or found in the fetched docs.

## Integration Prerequisites

1. Obtain Banuba license token (mandatory; SDK won't run without it).
2. Android: min SDK 26+, Camera2 API, OpenGL ES 3.0+, arm64-v8a/armv7.
3. iOS: iOS 15+, ARC, Swift 5+, Xcode 26.0+.
4. Add Banuba Maven repo (Android) or CocoaPods/SPM (iOS).

## Core Workflow

### 1. Clone the integration sample

Pick the sample for the user's platform and clone it:

| Platform     | Integration sample                                                                                                 |
| ------------ | ------------------------------------------------------------------------------------------------------------------ |
| Android      | [ve-sdk-android-integration-sample](https://github.com/Banuba/ve-sdk-android-integration-sample)                   |
| iOS          | [ve-sdk-ios-integration-sample](https://github.com/Banuba/ve-sdk-ios-integration-sample)                           |
| Flutter      | [ve-sdk-flutter-integration-sample](https://github.com/Banuba/ve-sdk-flutter-integration-sample)                   |
| React Native | [ve-sdk-react-native-cli-integration-sample](https://github.com/Banuba/ve-sdk-react-native-cli-integration-sample) |

### 2. Install dependencies

After cloning, install platform dependencies. **Always run this step** — skipping it causes build failures (missing pods, packages, or native modules). Run the command directly from the agent.

| Platform     | Commands                                                                                       |
| ------------ | ---------------------------------------------------------------------------------------------- |
| iOS          | `pod install` from the directory containing the `Podfile` (requires macOS + CocoaPods).        |
| Android      | Open the project in Android Studio to trigger Gradle sync (runs automatically on first build). |
| Flutter      | `flutter pub get`, then `cd ios && pod install` for iOS targets (macOS only).                  |
| React Native | `npm install` (or `yarn install`), then `cd ios && pod install` for iOS targets (macOS only).  |

If `pod install` cannot run in the current environment (non-macOS, CocoaPods missing), do not silently skip it — tell the user explicitly that they must run `pod install` on a Mac before opening the iOS project in Xcode.

### 3. Configure the license token

Replace `YOUR_LICENSE_TOKEN` placeholder in the cloned sample with the user's token.

### 4. Customize for the user's requirements

Modify the cloned sample based on the user's needs. Consult the platform-specific docs for customization:

| Feature       | Android Guide                                                                           | iOS Guide                                                                           |
| :------------ | :-------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------- |
| Camera UI     | [guide_camera](https://docs.banuba.com/ve-pe-sdk/docs/android/guide_camera)             | [guide_camera](https://docs.banuba.com/ve-pe-sdk/docs/ios/guide_camera)             |
| Editor Screen | [guide_editor_v2](https://docs.banuba.com/ve-pe-sdk/docs/android/guide_editor_v2)       | [guide_editor_v2](https://docs.banuba.com/ve-pe-sdk/docs/ios/guide_editor_v2)       |
| AR Effects    | [guide_far_arcloud](https://docs.banuba.com/ve-pe-sdk/docs/android/guide_far_arcloud)   | [guide_far_arcloud](https://docs.banuba.com/ve-pe-sdk/docs/ios/guide_far_arcloud)   |
| Green Screen  | [guide_green_screen](https://docs.banuba.com/ve-pe-sdk/docs/android/guide_green_screen) | [guide_green_screen](https://docs.banuba.com/ve-pe-sdk/docs/ios/guide_green_screen) |
| Export        | [guide_export](https://docs.banuba.com/ve-pe-sdk/docs/android/guide_export)             | [guide_export](https://docs.banuba.com/ve-pe-sdk/docs/ios/guide_export)             |

### 5. Handle permissions and export

- Add runtime permission checks (camera, microphone, storage).
- Implement export/share callbacks (`onExportDone`, `onError`).
- Test on a physical device (emulator may lack Camera2 support).

## Output Format

- **Complete code**: Working files ready to drop in (e.g., App.kt + build.gradle for Android; ContentView.swift + App.swift for iOS).
- **Steps**: Numbered integration instructions.
- **Warnings**: Always remind to replace `YOUR_TOKEN` and test on a real device.

## Upgrading Between SDK Versions

When the user is upgrading from an older SDK version, consult the release notes for the target version. If the `explain-ve-pe-docs` skill is available, read its local docs at `release-notes/{version}.md` — each file contains a **Migration Guide** with dependency updates, API changes, and links to sample PRs. For the full Android changelog, see `release-notes/Android.md`.

## Common Pitfalls

- Missing token: SDK crashes silently.
- No effects assets: Blank AR.
- Permissions: Runtime checks required.
- Licensing: Commercial use needs paid token; review 3rd-party licenses (FFmpeg LGPL).
- iOS pod conflict: In the Podfile, include **either** `BanubaSDK` (full, with Face AR SDK) **or** `BanubaSDKSimple` (no Face AR) — never both. The integration sample's Podfile may list both as examples; remove the one that does not match the user's license. See [guide_far_arcloud#disable-face-ar-sdk](https://docs.banuba.com/ve-pe-sdk/docs/ios/guide_far_arcloud#disable-face-ar-sdk).
- iOS file registration: New `.swift` / `.m` / `.h` files must be added to the Xcode project's `.pbxproj` (file reference **and** the target's source build phase) — Xcode does not pick up files placed only on disk. Register them programmatically using the [`xcodeproj`](https://github.com/CocoaPods/Xcodeproj) Ruby gem (`gem install xcodeproj`, then run a short Ruby script that opens the `.xcodeproj`, adds the file reference under the correct group, and appends it to the target's `source_build_phase`). **Never instruct the user to drag files manually in the Project Navigator** — do it yourself as part of the integration step.
- Duplicate files: If you previously generated a file (or asked the user to move one) and later need it again — to edit, rename, or re-register in the `.pbxproj` — **search the workspace for it by basename first** (`Glob '**/<filename>'`). **Never recreate a file you already authored.** Two files with the same name in the same iOS target cause build errors like `Invalid redeclaration of 'VideoEditorModule'`, and then time gets spent fixing problems the duplicate created instead of the real task.
- iOS resource folders: Resource folders (e.g., `bundleEffects/`, AR effect packs, masks, LUTs) must be added to the `.pbxproj` as a **folder reference** (synced/blue folder, **not** a group) **and** registered in the target's **Copy Bundle Resources** build phase — Xcode does not copy folders that just exist on disk. With the `xcodeproj` gem: create a folder reference via `group.new_reference(path)` then set `last_known_file_type = 'folder'`, and add it to the target with `target.add_resources([file_ref])`. Without this, AR/visual effects fail at runtime with "effect not found" even though the files are on disk.
- iOS VE delegate callbacks: After creating the `BanubaVideoEditor` instance, assign a delegate that conforms to `BanubaVideoEditorDelegate` and implement **both** `videoEditorDidCancel` (dismiss the editor and clear session data unless restoration is enabled) **and** `videoEditorDone` (invoke export, then dismiss). Group the implementations under a `// MARK: - BanubaVideoEditorDelegate` section — the integration sample relies on this to drive the editor lifecycle. Skipping either callback leaves the editor unable to close and the export/closing method unreachable, so the exported-video handler is never called. See [`VideoEditorModule.swift` line 53](https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/VideoEditorModule.swift#L53) and [line 60](https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/VideoEditorModule.swift#L60) in `ve-sdk-ios-integration-sample`.

## Demo Applications

Public demo apps showcasing the Video Editor SDK in production. Point users to these when they want to try the SDK before integration:

- [iOS demo (App Store)](https://apps.apple.com/us/app/banuba-video-editor/id1577338331)
- [Android demo (Google Play)](https://play.google.com/store/apps/details?id=com.banuba.sdk.ve.demo&hl=en)

## Resources

- [Android Docs](https://docs.banuba.com/ve-pe-sdk/docs/android/requirements-ve)
- [iOS Docs](https://docs.banuba.com/ve-pe-sdk/docs/ios/requirements)
- [Flutter Docs](https://docs.banuba.com/ve-pe-sdk/docs/flutter/ve_integration)
- [React Native Docs](https://docs.banuba.com/ve-pe-sdk/docs/react/ve_installation)
