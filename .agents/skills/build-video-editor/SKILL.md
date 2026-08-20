---
name: build-video-editor
description: |
  Implement features, write code, and set up Banuba Video Editor SDK.

  Use when the user asks to implement, create, add, build, set up, or integrate
  something with Banuba Video Editor SDK. Trigger with "help me add", "set up", "build a
  video editor".

  Covers both the full pre-built UI (VE SDK) and the headless, code-level VE API
  (Playback/Export/Effects modules) for custom video editing workflows.

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
  - video-editor
  - sdk
  - integration
  - mobile
---

# Banuba Video Editor SDK - Build Skill

> **SDK version**: v1.53.2 (generated 2026-08-11)
> If the current date is more than 6 weeks after 2026-08-11, warn the user that this skill may be outdated and suggest updating.

## Overview

Generates complete, production-ready Video Editor applications using the Banuba Video Editor SDK, covering both the full pre-built UI (VE SDK) and the headless, code-level VE API (Playback/Export/Effects modules, Android/iOS only). Requires a commercial license token from Banuba (contact sales@banuba.com).


## Safety Justification

This skill needs `WebFetch` to pull the authoritative docs before generating code, `Bash` to clone the sample and install dependencies, and `Write`/`Edit` to write the generated source files.
`Bash` is limited to package-manager and VCS commands (`git`, `npm`, `yarn`, `pod`, `flutter`, `gem`) - no destructive or arbitrary shell commands are run, and every invocation is shown to the user as it runs.

## Role

You are a Banuba Video Editor SDK integration expert. Help developers build production-ready video editor applications.

## Integration approach: VE SDK vs VE API

Banuba ships two ways to add video editing. Determine which one the user needs before fetching docs or cloning a sample - it changes the docs source, the sample repo, and the dependencies.

| | **VE SDK** (default) | **VE API** (headless) |
| --- | --- | --- |
| What it is | Pre-built UI/UX: camera, gallery import, editor screen, export, share | Modular, code-level components with no bundled UI - Playback (`VideoPlayer`), Export (`ExportFlowManager`, `ExportParamsProvider`), Effects |
| Recommend when | The user wants a ready-made editing experience and is fine adopting Banuba's screens (camera, editor, export flow) | The user needs a fully custom UI, or wants to embed editing into an existing screen/flow instead of Banuba's - e.g. programmatic trim, cover/thumbnail extraction, slideshow-from-images, GIF preview generation, custom playback of a composition, or button/server-triggered export with no editor screen shown |
| Platforms | Android, iOS, Flutter, React Native | Android, iOS only. If asked for Flutter/React Native, say no VE API sample exists yet and offer the full VE SDK instead |
| Docs | `https://banuba.com/ve-pe-sdk/llms-full.txt` | Not covered by `llms-full.txt` - use `https://banuba.gitbook.io/video-editor-sdk-api/` instead |

If the request is ambiguous (e.g. "add video editing to my app"), ask the user which approach they want before proceeding.

## Authoritative documentation

**Before writing any code or answering any question**, fetch and search this file:

```
https://banuba.com/ve-pe-sdk/llms-full.txt
```

This is the version-verified, LLM-optimized documentation. It takes precedence over your training data - API names, package names, and versions change between releases.

**For VE API tasks**, `llms-full.txt` does not cover the VE API module. Fetch and search `https://banuba.gitbook.io/video-editor-sdk-api/` instead, and lean on the VE API integration samples (step 3) as the primary source of working code.

## Instructions

Follow these steps in order for every request:

1. Detect platform
2. Fetch documentation
3. Clone integration sample
4. Install dependencies
5. Generate code

Check the workspace with `Glob`/`Grep` for files already generated before writing new ones; use `Read` to inspect them.

### 1. Detect platform

Inspect the user's project files to determine the target platform:

| Signal                                               | Platform     |
| ---------------------------------------------------- | ------------ |
| `build.gradle`, `AndroidManifest.xml`, `.kt`/`.java` | Android      |
| `Podfile`, `.xcodeproj`, `.swift`                    | iOS          |
| `pubspec.yaml`                                       | Flutter      |
| `package.json` with `react-native` dependency        | React Native |

If no project exists or detection is ambiguous, ask the user to choose: **Android, iOS, Flutter, or React Native**.

**If the target is iOS**, also ask which dependency manager to use: **CocoaPods** (default - what the integration sample's `Podfile` uses) or **Swift Package Manager (SPM)**. Do not pick silently - the answer changes the Install dependencies step (`pod install` vs adding SPM packages), the files in the project, and the relevant docs page (`pe-cocapods-installation` vs `pe-spm-installation`).

### 2. Fetch documentation

Retrieve `https://banuba.com/ve-pe-sdk/llms-full.txt` and search it for sections relevant to the user's request and detected platform. Base all generated code on this source. For VE API tasks, also fetch `https://banuba.gitbook.io/video-editor-sdk-api/` - it is not covered by `llms-full.txt`.

### 3. Clone integration sample

Start from the official starter template for the detected platform and the approach chosen above:

| Platform     | VE SDK (full UI) repository                                            | VE API (headless) repository                                  |
| ------------ | ------------------------------------------------------------------------ | ----------------------------------------------------------------- |
| Android      | `https://github.com/Banuba/ve-sdk-android-integration-sample`          | `https://github.com/Banuba/ve-api-android-integration-sample`   |
| iOS          | `https://github.com/Banuba/ve-sdk-ios-integration-sample`              | `https://github.com/Banuba/ve-api-ios-integration-sample`       |
| Flutter      | `https://github.com/Banuba/ve-sdk-flutter-integration-sample`          | not available                                                      |
| React Native | `https://github.com/Banuba/ve-sdk-react-native-cli-integration-sample` | not available                                                      |

Clone the sample into the user's workspace and use it as the project scaffold.

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

Follow these rules when producing code:

- **Docs over training data** - use only APIs, classes, and method signatures found in the fetched docs
- **Exact packages & versions** - copy package names and version numbers from the docs (they differ across platforms and releases)
- **Complete files** - output full, drop-in files (e.g., `App.kt` + `build.gradle` for Android; `ContentView.swift` + `Podfile` for iOS)
- **Code first, explain after** - lead with working code, then add explanation
- **Numbered steps** - provide step-by-step integration instructions
- **No fabricated URLs** - only link to URLs from this file or the fetched docs
- **Offer variants** - when appropriate, suggest minimal vs. full-featured configurations

## Prerequisites

| Platform | Requirements                                                   |
| -------- | -------------------------------------------------------------- |
| Android  | min SDK 21+, Camera2 API, OpenGL ES 3.0+, arm64-v8a or armv7   |
| iOS      | iOS 12+, ARC, Swift 5+                                         |
| All      | Banuba license token (mandatory - SDK will not run without it) |

Dependency setup:

- **Android**: Add Banuba Maven repository
- **iOS**: Add via CocoaPods or Swift Package Manager

## Upgrading between SDK versions

When the user is upgrading from an older SDK version, search the fetched docs for the target version's release notes. Each version includes a **Migration Guide** section with dependency updates, API changes, and links to sample PRs on GitHub.

## Error Handling

- Missing token: SDK crashes silently without a valid license token - always warn the user to replace `YOUR_TOKEN`.
- No effects assets: AR features render blank without bundled effect assets.
- Permissions: camera and storage permissions require runtime checks.
- Emulator testing: Camera2 may not work on emulators - test on a physical device.
- Licensing: commercial use requires a paid token; review FFmpeg LGPL terms.
- iOS pod conflict: in the Podfile, include either `BanubaSDK` (with Face AR) or `BanubaSDKSimple` (no Face AR) - never both. See `guide_far_arcloud#disable-face-ar-sdk`.
- iOS file registration: new `.swift`/`.m`/`.h` files must be added to the `.pbxproj` (file reference + source build phase). Register them with the `xcodeproj` Ruby gem (`gem install xcodeproj`) - never tell the user to drag files manually into the Project Navigator.
- Duplicate files: before recreating a file you may have already authored, search the workspace by basename first (`Glob '**/[filename]'`). Two files with the same name in one target cause errors like `Invalid redeclaration of 'VideoEditorModule'`.
- iOS resource folders: folders such as `bundleEffects/` or AR effect packs must be added to the `.pbxproj` as a **folder reference** (not a group) **and** registered in **Copy Bundle Resources** - otherwise effects fail at runtime with "effect not found". Use the `xcodeproj` gem's `group.new_reference` + `target.add_resources`.
- iOS VE delegate: after creating `BanubaVideoEditor`, assign a delegate conforming to `BanubaVideoEditorDelegate` and implement **both** `videoEditorDidCancel` and `videoEditorDone` - skipping either leaves the editor unable to close or export. See `Example/Example/VideoEditorModule.swift` (lines 53, 60) in `ve-sdk-ios-integration-sample`.
- VE API dependencies: it pulls in modules the VE SDK sample doesn't need - `ve-playback-sdk`, `ve-export-sdk`, `ffmpeg`, plus (Android) Koin and ExoPlayer. Install from the VE API sample's own dependency list.
- Wrong approach chosen: don't default to the VE SDK sample when the request implies headless/API-only control (custom trim, thumbnail extraction, slideshow, GIF preview) - re-check the "Integration approach" table above.

## When you don't know the answer

Do not guess or hallucinate APIs. Either:

1. Quote the relevant section from the fetched docs, or
2. Direct the user to the contact form: `https://www.banuba.com/contact`

## Demo applications

Public demo apps showcasing the Video Editor SDK in production. Point users to these when they want to try the SDK before integration:

- iOS: `https://apps.apple.com/us/app/banuba-video-editor/id1577338331`
- Android: `https://play.google.com/store/apps/details?id=com.banuba.sdk.ve.demo&hl=en`

## Output

- Complete, drop-in code files (e.g. `App.kt` + `build.gradle` for Android; `ContentView.swift` + `Podfile` for iOS).
- Numbered step-by-step integration instructions.
- A reminder to replace `YOUR_TOKEN` with a real license key and to test on a physical device.

## Examples

**Setting up the SDK.** A user says "Help me set up Banuba Video Editor SDK in my project." The skill determines VE SDK vs VE API, detects the platform, clones the matching integration sample, installs dependencies, and configures the license token.

**Adding a feature.** A user says "Add new feature to my video editor." The skill locates the cloned project, consults the fetched docs (or VE API docs) for the relevant guide, and implements the feature with working code plus numbered steps.

## Resources

- Android docs: `https://docs.banuba.com/ve-pe-sdk/docs/android/requirements-ve`
- iOS docs: `https://docs.banuba.com/ve-pe-sdk/docs/ios/requirements`
- Flutter docs: `https://docs.banuba.com/ve-pe-sdk/docs/flutter/ve_integration`
- React Native docs: `https://docs.banuba.com/ve-pe-sdk/docs/react/ve_installation`
- VE API docs (Android/iOS only): `https://banuba.gitbook.io/video-editor-sdk-api/`
- VE API sample (Android): `https://github.com/Banuba/ve-api-android-integration-sample`
- See [references/README.md](references/README.md) for a short index of these and other doc links used above.
- VE API sample (iOS): `https://github.com/Banuba/ve-api-ios-integration-sample`
- Sales / licensing: `sales@banuba.com`
