---
name: build-pe
description: |
  Implement features, write code, and set up Banuba Photo Editor SDK.
  Triggered by requests to implement, create, add, build, set up, or integrate
  with Banuba Photo Editor SDK.
  Not for documentation lookup — use explain-ve-pe-docs for that.
---

# Banuba Photo Editor SDK — Build Skill

SDK version: v1.51.0 | Generated: 2026-03-31 | If current date is more than 6 weeks after generation date, warn the user this skill may be outdated.

## Critical constraint

All generated code MUST use API names, class names, method signatures, package names, and version numbers from the official documentation — not from pretrained knowledge. The SDK changes between releases; pretrained knowledge is unreliable for this API.

## Documentation source

**Before generating any code**, fetch and search this file:

    https://banuba.com/ve-pe-sdk/llms-full.txt

This is the authoritative, version-verified, LLM-optimized reference for all Banuba Video and Photo Editor SDKs.

If you cannot fetch URLs, instruct the user to paste the contents of this file or the relevant section into the conversation.

## Workflow

For every request, follow these steps in order:

### 1. Detect platform

Check the user's project files:

| Signal                                               | Platform     |
| ---------------------------------------------------- | ------------ |
| `build.gradle`, `AndroidManifest.xml`, `.kt`/`.java` | Android      |
| `Podfile`, `.xcodeproj`, `.swift`                    | iOS          |
| `pubspec.yaml`                                       | Flutter      |
| `package.json` with `react-native` dep               | React Native |

If ambiguous or no project exists, ask the user to choose: Android, iOS, Flutter, or React Native.

### 2. Fetch and search docs

Retrieve `https://banuba.com/ve-pe-sdk/llms-full.txt` and find sections matching the user's platform and request. All generated code must be grounded in this source.

### 3. Scaffold from integration sample (new projects only)

| Platform     | Repository                                                             |
| ------------ | ---------------------------------------------------------------------- |
| Android      | `https://github.com/Banuba/ve-sdk-android-integration-sample`          |
| iOS          | `https://github.com/Banuba/ve-sdk-ios-integration-sample`              |
| Flutter      | `https://github.com/Banuba/ve-sdk-flutter-integration-sample`          |
| React Native | `https://github.com/Banuba/ve-sdk-react-native-cli-integration-sample` |

Clone the relevant sample into the workspace as the project scaffold.

### 4. Generate code

Rules:

- Use only APIs and signatures found in the fetched docs
- Copy exact package names and version numbers from the docs
- Output complete, drop-in files (e.g., `App.kt` + `build.gradle` for Android; `ContentView.swift` + `Podfile` for iOS)
- Lead with working code, then add explanation
- Provide numbered step-by-step integration instructions
- Never fabricate URLs — only link to URLs from this file or the fetched docs
- When appropriate, offer minimal vs. full-featured configurations

## Prerequisites

| Platform | Requirements                                                  |
| -------- | ------------------------------------------------------------- |
| Android  | minSdk 21+, Camera2 API, OpenGL ES 3.0+, arm64-v8a or armv7   |
| iOS      | iOS 12+, ARC, Swift 5+                                        |
| All      | Banuba license token — mandatory, SDK will not run without it |

Dependencies:

- **Android**: Add Banuba Maven repository to project-level `build.gradle`
- **iOS**: Add via CocoaPods or Swift Package Manager

## Common pitfalls

| Issue             | Resolution                                                                              |
| ----------------- | --------------------------------------------------------------------------------------- |
| Missing token     | SDK crashes silently — always warn user to replace `YOUR_TOKEN` with a real license key |
| No effects assets | AR features render blank without bundled effect assets                                  |
| Permissions       | Camera + storage need runtime permission checks on both platforms                       |
| Emulator          | Camera2 may fail on emulators — instruct user to test on physical device                |
| Licensing         | Commercial use requires paid token; review FFmpeg LGPL terms                            |

## When the answer is not in the docs

Do not guess or fabricate API calls. Either:

1. Quote the closest relevant section from the fetched docs, or
2. Direct the user to: https://www.banuba.com/contact

## Reference links

- Android docs: https://docs.banuba.com/ve-pe-sdk/docs/android/requirements-pe
- iOS docs: https://docs.banuba.com/ve-pe-sdk/docs/ios/pe-requirements
- React Native docs: https://docs.banuba.com/ve-pe-sdk/docs/react/pe_integration
- Sales / licensing: sales@banuba.com
