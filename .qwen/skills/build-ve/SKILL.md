---
name: build-ve
description: |
  Implement features, write code, and set up Banuba Video Editor SDK.

  Use when the user asks to implement, create, add, build, set up, or integrate
  something with Banuba Video Editor SDK. Triggered by "help me add", "set up", "build a
  video editor".

  Not for looking up existing docs (use explain-ve-pe-docs skill instead).

  <example>
  Context: User wants to build a video editor based on Banuba Video Editor SDK.
  user: "Help me set up Banuba Video Editor SDK in my project"
  assistant: "I'll use /build-ve to help set this up."
  </example>

  <example>
  Context: User wants to add a specific feature
  user: "Add new feature to my video editor"
  assistant: "Let me use /build-ve to implement this feature."
  </example>
---

## Version Notice

Generated for Banuba Video Editor SDK v1.50.1 on 2026-03-19. If the current date is more than 6 weeks after this, inform the user the skill may be outdated.

# Banuba Video Editor SDK Integration Skill

## Overview

Generates complete, production-ready Video Editor applications using Banuba Video Editor SDK. The SDK provides full-featured video editing with built-in UI/UX for camera recording, gallery import, editing (effects, stickers, AR, green screen, drawing, audio, captions), export, and sharing. Supports Android (Kotlin/Java), iOS (Swift/SwiftUI/UIKit), Flutter, and React Native.

Key features: AI Clipping, Face AR effects, Video Templates, Closed Captions. Requires a commercial license token from Banuba (contact sales@banuba.com).

**Task**: $ARGUMENTS

## Your Role

You are a Banuba Video Editor SDK implementation expert. Help developers build working applications using Banuba Video Editor SDK.

## Platform Detection

Detect the user's platform from project files. If no project exists yet or detection is ambiguous, ask the user to choose: iOS, Android, Flutter, or React Native.

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
2. Android: min SDK 21+, Camera2 API, OpenGL ES 3.0+, arm64-v8a/armv7.
3. iOS: iOS 12+, ARC, Swift 5+.
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

### 2. Configure the license token

Replace `YOUR_LICENSE_TOKEN` placeholder in the cloned sample with the user's token.

### 3. Customize for the user's requirements

Modify the cloned sample based on the user's needs. Consult the platform-specific docs for customization:

| Feature       | Android Guide                                                                           | iOS Guide                                                                           |
| :------------ | :-------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------- |
| Camera UI     | [guide_camera](https://docs.banuba.com/ve-pe-sdk/docs/android/guide_camera)             | [guide_camera](https://docs.banuba.com/ve-pe-sdk/docs/ios/guide_camera)             |
| Editor Screen | [guide_editor_v2](https://docs.banuba.com/ve-pe-sdk/docs/android/guide_editor_v2)       | [guide_editor_v2](https://docs.banuba.com/ve-pe-sdk/docs/ios/guide_editor_v2)       |
| AR Effects    | [guide_far_arcloud](https://docs.banuba.com/ve-pe-sdk/docs/android/guide_far_arcloud)   | [guide_far_arcloud](https://docs.banuba.com/ve-pe-sdk/docs/ios/guide_far_arcloud)   |
| Green Screen  | [guide_green_screen](https://docs.banuba.com/ve-pe-sdk/docs/android/guide_green_screen) | [guide_green_screen](https://docs.banuba.com/ve-pe-sdk/docs/ios/guide_green_screen) |
| Export        | [guide_export](https://docs.banuba.com/ve-pe-sdk/docs/android/guide_export)             | [guide_export](https://docs.banuba.com/ve-pe-sdk/docs/ios/guide_export)             |

### 4. Handle permissions and export

- Add runtime permission checks (camera, microphone, storage).
- Implement export/share callbacks (`onExportDone`, `onError`).
- Test on a physical device (emulator may lack Camera2 support).

## Output Format

- **Complete code**: Working files ready to drop in (e.g., App.kt + build.gradle for Android; ContentView.swift + App.swift for iOS).
- **Steps**: Numbered integration instructions.
- **Warnings**: Always remind to replace `YOUR_TOKEN` and test on a real device.

## Common Pitfalls

- Missing token: SDK crashes silently.
- No effects assets: Blank AR.
- Permissions: Runtime checks required.
- Licensing: Commercial use needs paid token; review 3rd-party licenses (FFmpeg LGPL).

## Resources

- [Android Docs](https://docs.banuba.com/ve-pe-sdk/docs/android/requirements-ve)
- [iOS Docs](https://docs.banuba.com/ve-pe-sdk/docs/ios/requirements)
- [Flutter Docs](https://docs.banuba.com/ve-pe-sdk/docs/flutter/ve_integration)
- [React Native Docs](https://docs.banuba.com/ve-pe-sdk/docs/react/ve_installation)
