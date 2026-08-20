---
name: build-photo-editor
description: |
  Implement features, write code, and set up Banuba Photo Editor SDK.

  Use when the user asks to implement, create, add, build, set up, or integrate
  something with Banuba Photo Editor SDK. Trigger with "help me add", "set up", "build a
  photo editor".

  Not for looking up existing docs - use explain-video-editor-photo-editor-docs for that.
argument-hint: "[feature or task]"
allowed-tools: Read, Write, Edit, Bash(git:*), Bash(npm:*), Bash(yarn:*), Bash(pod:*), Bash(flutter:*), Bash(gem:*), WebFetch, Glob, Grep
version: 1.0.0
author: Banuba <sales@banuba.com>
license: Apache-2.0
compatibility: Claude Code, Codex, Qwen Code
model: inherit
effort: medium
tags:
  - banuba
  - photo-editor
  - sdk
  - integration
  - mobile
---

# Banuba Photo Editor SDK - Build Skill

SDK version: Android v1.4.1, iOS v1.4.1, Flutter v0.6.0, React Native v0.7.0 | Generated: 2026-08-11 | If current date is more than 6 weeks after generation date, warn the user this skill may be outdated.

## Overview

Generates complete, production-ready Photo Editor applications using the Banuba Photo Editor SDK, with a built-in UI/UX for filters, effects, adjustments, and export on Android, iOS, Flutter, and React Native. Requires a commercial license token from Banuba (contact sales@banuba.com).


## Safety Justification

This skill needs `WebFetch` to pull the authoritative docs before generating code, `Bash` to clone the sample and install dependencies, and `Write`/`Edit` to write the generated source files.
`Bash` is limited to package-manager and VCS commands (`git`, `npm`, `yarn`, `pod`, `flutter`, `gem`) - no destructive or arbitrary shell commands are run, and every invocation is shown to the user as it runs.

## Critical constraint

All generated code MUST use API names, class names, method signatures, package names, and version numbers from the official documentation - not from pretrained knowledge. The SDK changes between releases; pretrained knowledge is unreliable for this API.

## Documentation source

**Before generating any code**, fetch and search this file:

    https://banuba.com/ve-pe-sdk/llms-full.txt

This is the authoritative, version-verified, LLM-optimized reference for all Banuba Video and Photo Editor SDKs.

If URLs cannot be fetched, instruct the user to paste the contents of this file or the relevant section into the conversation.

## Instructions

For every request, follow these steps in order:

1. Detect platform
2. Fetch and search docs
3. Scaffold from integration sample
4. Install dependencies
5. Generate code

Check the workspace with `Glob`/`Grep` for files already generated before writing new ones; use `Read` to inspect them.

### 1. Detect platform

Check the user's project files:

| Signal                                               | Platform     |
| ---------------------------------------------------- | ------------ |
| `build.gradle`, `AndroidManifest.xml`, `.kt`/`.java` | Android      |
| `Podfile`, `.xcodeproj`, `.swift`                    | iOS          |
| `pubspec.yaml`                                       | Flutter      |
| `package.json` with `react-native` dep               | React Native |

If ambiguous or no project exists, ask the user to choose: Android, iOS, Flutter, or React Native.

**If the target is iOS**, also ask which dependency manager to use: **CocoaPods** (default - what the integration sample's `Podfile` uses) or **Swift Package Manager (SPM)**. Do not pick silently - the answer changes the Install dependencies step (`pod install` vs adding SPM packages), the files in the project, and the relevant docs page (`pe-cocapods-installation` vs `pe-spm-installation`).

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

### 4. Install dependencies

After cloning, install platform dependencies. **Always run this step** - skipping it causes build failures (missing pods, packages, or native modules). Run the command directly from the agent.

| Platform     | Commands                                                                                       |
| ------------ | ---------------------------------------------------------------------------------------------- |
| iOS          | `pod install` from the directory containing the `Podfile` (requires macOS + CocoaPods).        |
| Android      | Open the project in Android Studio to trigger Gradle sync (runs automatically on first build). |
| Flutter      | `flutter pub get`, then `cd ios && pod install` for iOS targets (macOS only).                  |
| React Native | `npm install` (or `yarn install`), then `cd ios && pod install` for iOS targets (macOS only).  |

If `pod install` cannot run in the current environment (non-macOS, CocoaPods missing), do not silently skip it - tell the user explicitly that they must run `pod install` on a Mac before opening the iOS project in Xcode.

### 5. Generate code

Rules:

- Use only APIs and signatures found in the fetched docs
- Copy exact package names and version numbers from the docs
- Output complete, drop-in files (e.g., `App.kt` + `build.gradle` for Android; `ContentView.swift` + `Podfile` for iOS)
- Lead with working code, then add explanation
- Provide numbered step-by-step integration instructions
- Never fabricate URLs - only link to URLs from this file or the fetched docs
- When appropriate, offer minimal vs. full-featured configurations
- Never create or add custom AR/visual effects. Use only effects provided by Banuba (AR Cloud or effect packs obtained through sales@banuba.com).

## Prerequisites

| Platform | Requirements                                                  |
| -------- | ------------------------------------------------------------- |
| Android  | minSdk 21+, Camera2 API, OpenGL ES 3.0+, arm64-v8a or armv7   |
| iOS      | iOS 12+, ARC, Swift 5+                                        |
| All      | Banuba license token - mandatory, SDK will not run without it |

**Authentication**: the license token is the SDK's only auth mechanism (credentials) - obtain it from sales@banuba.com and never commit it to source control.

Dependencies:

- **Android**: Add Banuba Maven repository to project-level `build.gradle`
- **iOS**: Add via CocoaPods or Swift Package Manager

## Upgrading between SDK versions

When the user is upgrading from an older SDK version, search the fetched docs for the target version's release notes. Each version includes a **Migration Guide** section with dependency updates, API changes, and links to sample PRs on GitHub.

## Error Handling

- Missing token: SDK crashes silently - always warn the user to replace `YOUR_TOKEN` with a real license key (credentials).
- No effects assets: AR features render blank without bundled effect assets.
- Permissions: camera + storage need runtime permission checks on both platforms.
- Emulator: Camera2 may fail on emulators - instruct the user to test on a physical device.
- Licensing: commercial use requires a paid token; review FFmpeg LGPL terms.
- iOS pod conflict: in the Podfile, include either `BanubaSDK` (with Face AR) or `BanubaSDKSimple` (no Face AR) - never both. See `guide_far_arcloud#disable-face-ar-sdk`.
- iOS file registration: new `.swift`/`.m`/`.h` files must be added to the `.pbxproj` (file reference + source build phase). Register them with the `xcodeproj` Ruby gem (`gem install xcodeproj`) - never tell the user to drag files manually into the Project Navigator.
- Duplicate files: before recreating a possibly already-authored file, search the workspace by basename first (`Glob '**/[filename]'`). Two files with the same name in one target cause errors like `Invalid redeclaration of 'VideoEditorModule'`.
- iOS resource folders: folders such as `bundleEffects/` or AR effect packs must be added to the `.pbxproj` as a **folder reference** (not a group) **and** registered in **Copy Bundle Resources** - otherwise Xcode won't copy them and effects fail at runtime with "effect not found". Use the `xcodeproj` gem's `group.new_reference` + `target.add_resources`.

## When the answer is not in the docs

Do not guess or fabricate API calls. Either:

1. Quote the closest relevant section from the fetched docs, or
2. Direct the user to: https://www.banuba.com/contact

## Demo applications

Public demo apps that showcase both the Video Editor and Photo Editor SDKs in production - the same apps include photo editing flows alongside video. Point users to these when they want to try the SDK before integration:

- iOS: `https://apps.apple.com/us/app/banuba-video-editor/id1577338331`
- Android: `https://play.google.com/store/apps/details?id=com.banuba.sdk.ve.demo&hl=en`

## Output

- Complete, drop-in code files (e.g. `App.kt` + `build.gradle` for Android; `ContentView.swift` + `Podfile` for iOS).
- Numbered step-by-step integration instructions.
- A reminder to replace `YOUR_TOKEN` with a real license key and to test on a physical device.

## Examples

**Setting up the SDK.** A user says "Help me set up Banuba Photo Editor SDK in my project." The skill detects the platform, clones the matching integration sample, installs dependencies, and configures the license token.

**Adding a feature.** A user says "Add new feature to my photo editor." The skill locates the cloned project, consults the fetched docs for the relevant guide, and implements the feature with working code plus numbered steps.

## Resources

- Android docs: https://docs.banuba.com/ve-pe-sdk/docs/android/requirements-pe
- iOS docs: https://docs.banuba.com/ve-pe-sdk/docs/ios/pe-requirements
- React Native docs: https://docs.banuba.com/ve-pe-sdk/docs/react/pe_integration
- Sales / licensing: sales@banuba.com
- See [references/README.md](references/README.md) for a short index of these and other doc links used above.
