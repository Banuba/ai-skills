---
name: far-general
description: |
  Banuba Face AR SDK skill for three use cases: sales, developer documentation, and integration workflows.

  Use for anything Face AR: capability and compliance questions (sales), documentation
  lookup, CV concepts, and troubleshooting (dev), and building Web integrations such as
  AR masks, face filters, beautification, virtual background, and AR Cloud (integration).
  Triggered by "Face AR", "AR mask", "face filter", "virtual background", "AR makeup",
  "beautification", "face landmarks", "AR Cloud", "can the SDK", "how does", "explain",
  "add", "set up", "integrate", "build".

  Web, Android, iOS, Desktop (C++), Flutter and React Native have full code generation
  support. macOS gets the GitHub sample + code assistance. Unity gets the sample link only.
  For Video/Photo Editor SDK use build-video-editor, build-photo-editor, or explain-video-editor-photo-editor-docs.

  <example>
  Context: Sales-team capability question.
  user: "Can our Face AR SDK detect skin tone, and what data does it store?"
  assistant: "I'll use /far-general in sales mode to check capabilities and compliance."
  </example>

  <example>
  Context: Explain-mode documentation question.
  user: "What is the difference between face landmarks and a face mesh?"
  assistant: "I'll use /far-general in Explain mode to explain the concepts."
  </example>

  <example>
  Context: Web integration request.
  user: "Add background blur to my Face AR web app"
  assistant: "I'll use /far-general in Build mode and follow the build workflow for virtual background."
  </example>
argument-hint: "[question or task]"
---

## Version Notice

Generated for Banuba Face AR SDK v1.18.2 on 2026-06-24. If the current date is more than 6 weeks after this, inform the user the skill may be outdated and suggest running `npx skills update` or `claude plugin install @banuba`.

# Banuba Face AR SDK Skill

## Overview

One skill, three modes:

- **Sales**: capabilities, limitations, and compliance for non-technical users (no code).
- **Explain**: technical documentation, concepts, API guidance, and troubleshooting (no project edits by default).
- **Build**: implementation, setup, integration, scaffolding, and code generation for supported technical platforms.

The SDK provides real-time face tracking, AR masks, beautification, virtual background, hair coloring, and AR Cloud delivery. On Web this is exposed through the `@banuba/webar` NPM package; native, Flutter, and React Native use their own packages/wrappers. Requires a commercial client token (contact sales@banuba.com).

**Request**: $ARGUMENTS

## Step 1: Detect the mode

Do this first, on every message. Classify the request into exactly one mode:

| Signal | Mode |
|---|---|
| "can the SDK...", "what data is stored", "tell our client", pricing/compliance, non-technical, no code context | Sales |
| "how does X work", "why is my effect not loading", troubleshooting, CV concepts, API/docs questions, conceptual or diagnostic requests without file-change intent | Explain |
| "add", "implement", "set up", "integrate", "build", "scaffold", "fix this project", project files plus action/fix intent | Build |

Rules:
- Pick one mode per message. Re-evaluate each message; the mode can change within a session.
- Ambiguous non-technical/business request: ask one question or default to Sales if the user clearly wants a client-facing answer.
- Ambiguous technical request: default to Explain unless the user asks for file changes, code generation, setup, integration, or project fixing.
- Hybrid request ("explain X and add it"): choose Build, read both `reference/explain.md` and `reference/build.md`, then answer in one flow.
- The hard gate is Sales: no code, plain language. Explain and Build cover the technical spectrum.

## Step 2: Apply the mode contract

### Sales mode

- Plain language. Lead with the capability and its limitation.
- Separate confirmed facts from items needing legal/product review. Do not invent compliance statements.
- No pricing, license fees, or contract terms; direct those to a Banuba representative via the [contact form](https://www.banuba.com/contact).
- **MUST NOT generate code.**
- Read `reference/sales.md`.

### Explain mode

Use for technical explanation, documentation lookup, concepts, API guidance, and troubleshooting when the user is not asking for project edits.

- Read `reference/explain.md`, then search/read the relevant bundled Markdown docs.
- Lead with the explanation when the user asks how or why.
- If the user turns the explanation into an implementation request, switch to Build and carry forward the established context.

### Build mode

Use for adding, implementing, setting up, integrating, scaffolding, code generation, prefab configs, or fixing project files.

- Read `reference/build.md`, then search/read the relevant bundled Markdown docs.
- For hybrid requests ("explain X and add it"), also read `reference/explain.md` and answer in one flow.
- When citing a source, link the public web doc (see shared principle 2), not an internal path. Lead with working code when the user asks to build; lead with the explanation when they ask how or why.

## Carrying context across modes

- **Sticky context**: facts established earlier in the session (platform, feature, bundler, chosen effect) carry forward. Do not re-ask what is already known.
- **Explain → Build**: a follow-up like "now add it" after an explanation means "build what we just discussed", not "which feature?".
- Switch modes only on a genuine change of intent, not on every message. When unsure, ask one clarifying question.

## Platform scope (all modes)

Web, Android, iOS, Desktop (C++), Flutter, and React Native have full coverage and code generation. For **macOS**: clone the official GitHub sample, then assist with code questions based on that sample — do not scaffold from scratch. For **Unity**: clone the sample only and direct to the [contact form](https://www.banuba.com/contact) — no code generation.

| Platform | Coverage | Sample |
|---|---|---|
| Web | ✅ Full - read `reference/build.md` (Web section) | [quickstart-web](https://github.com/Banuba/quickstart-web) |
| Android | ✅ Full - read `reference/build.md` (Android section) | [banuba-sdk-android-samples](https://github.com/Banuba/banuba-sdk-android-samples) |
| iOS | ✅ Full - read `reference/build.md` (iOS section) | [banuba-sdk-ios-samples](https://github.com/Banuba/banuba-sdk-ios-samples) |
| Desktop (C++) | ✅ Full - read `reference/build.md` (Desktop section) | [quickstart-desktop-cpp](https://github.com/Banuba/quickstart-desktop-cpp) |
| macOS | ⚠️ Clone sample + code help - clone sample, then assist with code questions | [quickstart-macos-swift](https://github.com/Banuba/quickstart-macos-swift) |
| Flutter | ✅ Full - read `reference/build.md` (Flutter section) | [banuba-sdk-flutter](https://github.com/Banuba/banuba-sdk-flutter) |
| React Native | ✅ Full - read `reference/build.md` (React Native section) | [banuba-sdk-react-native](https://github.com/Banuba/banuba-sdk-react-native) |
| Unity | 🚫 No code generation - clone sample only | [quickstart-unity](https://github.com/Banuba/quickstart-unity) |

**Platform detection:**
- Web: `package.json` (no `react-native`), `vite.config.*`, `webpack.config.*`, `rollup.config.*`, or `index.html` + JS bundler
- Android: `build.gradle`, `build.gradle.kts`, `AndroidManifest.xml`, or `*.kt` / `*.java` files
- iOS: `*.xcodeproj`, `*.xcworkspace`, `Podfile`, or `*.swift` / `*.m` files
- Desktop (C++): `CMakeLists.txt`, `*.cpp`, `*.hpp` files, or user explicitly says "desktop" / "C++"
- Flutter: `pubspec.yaml`, `lib/main.dart`, or user explicitly says "flutter"
- React Native: `package.json` with `react-native` dependency, `metro.config.*`, or user explicitly says "react native"
- If unclear, ask one question: "Which platform are you targeting?"

## Shared principles (all modes)

1. **Retrieval-first**: search the bundled Markdown docs and read only the relevant files before using pre-trained knowledge. Use `docs/llms-full.txt` as fallback. If a topic is missing locally, fetch [`https://docs.banuba.com/far-sdk/llms-full.txt`](https://docs.banuba.com/far-sdk/llms-full.txt).
2. **Cite public docs, not internal files**: when pointing the user to a source, link the public web doc (`https://docs.banuba.com/far-sdk/<path>`, dropping the `.md`). Never surface internal paths such as `docs/...md` or this skill's `reference/...md` files; they mean nothing to the user.
3. **Don't fabricate**: if the answer is not in the docs, point to [docs.banuba.com/far-sdk](https://docs.banuba.com/far-sdk/) or the [contact form](https://www.banuba.com/contact). Never invent APIs, URLs, or compliance claims.
4. **Generate config, not art**: the skill assembles prefab configuration; it does not create art assets. Custom AR masks, effects, and makeup looks are made in [Banuba Studio](https://studio.banuba.com/) ([docs](https://studio.banuba.com/docs)). Studio does not create 3D avatars/models - direct avatar requests to the [contact form](https://www.banuba.com/contact).
5. **GenAI APIs are separate products**: Wig try-on, PD Measurements, Video Generation, and Video Context Detection are not part of `@banuba/webar`. Direct to the [contact form](https://www.banuba.com/contact).
6. **No images**: do not embed or attempt to render images (no markdown image tags, no `[Image]` placeholders) - they will not display. Describe the visual in words, or link the public doc page that contains it (e.g. the landmarks or glossary page).
7. **Search hygiene**: prefer Markdown docs and `llms-full.txt` for retrieval. Use generated HTML/API docs only for exact class or method lookup, because broad search over `docs/generated/` creates noisy matches.
8. **Version discipline**: distinguish native FAR SDK versions from wrapper package versions. Android/iOS native modules use the FAR SDK line in this skill; Flutter `banuba_sdk` and React Native `@banuba/react-native` have independent package versions. When the user asks for "latest", "current", or whether to pin a wrapper version, verify against the official package registry or docs before answering.

## Reference files

- `reference/sales.md`: Sales mode. Capabilities, compliance, plain-language CV glossary.
- `reference/explain.md`: Explain mode. Use-case to doc map, troubleshooting, technical CV concepts.
- `reference/build.md`: Build mode - Web, Android, iOS, Desktop, Flutter, and React Native. Integration workflow per platform, prefab config, pitfalls, output format.
- `docs/`: bundled SDK documentation (single source for all modes).

## Related Skills

- For Video Editor / Photo Editor SDK: `/build-video-editor`, `/build-photo-editor`, `/explain-video-editor-photo-editor-docs`.

## Resources

- [Full documentation](https://docs.banuba.com/far-sdk/)
- [LLM-optimized docs](https://docs.banuba.com/far-sdk/llms-full.txt)
- [quickstart-web on GitHub](https://github.com/Banuba/quickstart-web)
- [@banuba/webar on NPM](https://www.npmjs.com/package/@banuba/webar)
- [Banuba Studio](https://studio.banuba.com/)
- [Contact form](https://www.banuba.com/contact)
- Sales and licensing: sales@banuba.com
