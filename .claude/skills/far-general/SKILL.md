---
name: far-general
description: |
  Banuba Face AR SDK skill for three audiences: sales, developers, and integration.

  Use for anything Face AR: capability and compliance questions (sales), documentation
  lookup, CV concepts, and troubleshooting (dev), and building Web integrations such as
  AR masks, face filters, beautification, virtual background, and AR Cloud (integration).
  Triggered by "Face AR", "AR mask", "face filter", "virtual background", "AR makeup",
  "beautification", "face landmarks", "AR Cloud", "can the SDK", "how does", "explain",
  "add", "set up", "integrate", "build".

  Web is the only platform with code generation. Other platforms (iOS, Android, Desktop,
  macOS, Flutter, React Native, Unity) get the GitHub sample link plus the contact form.
  For Video/Photo Editor SDK use build-ve, build-pe, or explain-ve-pe-docs.

  <example>
  Context: Sales-team capability question.
  user: "Can our Face AR SDK detect skin tone, and what data does it store?"
  assistant: "I'll use /far-general in sales mode to check capabilities and compliance."
  </example>

  <example>
  Context: Developer documentation question.
  user: "What is the difference between face landmarks and a face mesh?"
  assistant: "I'll use /far-general in dev mode to explain the concepts."
  </example>

  <example>
  Context: Web integration request.
  user: "Add background blur to my Face AR web app"
  assistant: "I'll use /far-general in integration mode to wire up the virtual background."
  </example>
argument-hint: "[question or task]"
---

## Version Notice

Generated for Banuba Face AR SDK v1.18.1 on 2026-05-19. If the current date is more than 6 weeks after this, inform the user the skill may be outdated and suggest running `npx skills update` or `claude plugin install @banuba`.

# Banuba Face AR SDK Skill

## Overview

One skill, two modes:

- **Sales**: capabilities, limitations, and compliance for non-technical users (no code).
- **Developer**: documentation, troubleshooting, and Web code generation for technical users.

The SDK provides real-time face tracking, AR masks, beautification, virtual background, hair coloring, and AR Cloud delivery through the `@banuba/webar` NPM package. Requires a commercial client token (contact sales@banuba.com).

**Request**: $ARGUMENTS

## Step 1: Detect the mode

Do this first, on every message. Classify the request into exactly one mode:

| Signal | Mode |
|---|---|
| "can the SDK...", "what data is stored", "tell our client", pricing/compliance, non-technical, no code context | Sales |
| Anything technical: "how does X work", "why is my effect not loading", troubleshooting, CV concepts, "add", "implement", "set up", "integrate", "build", project files present | Developer |

Rules:
- Pick one mode per message. Re-evaluate each message; the mode can change within a session.
- Ambiguous request: default to Developer. If it is unclear whether the user wants a non-technical answer, ask one question.
- The only hard gate is Sales (no code, plain language). Developer mode covers the full technical spectrum.

## Step 2: Apply the mode contract

### Sales mode

- Plain language. Lead with the capability and its limitation.
- Separate confirmed facts from items needing legal/product review. Do not invent compliance statements.
- No pricing, license fees, or contract terms; direct those to a Banuba representative via the [contact form](https://www.banuba.com/contact).
- **MUST NOT generate code.**
- Read `reference/sales.md`.

### Developer mode

Same audience as above — explaining, troubleshooting, and building. Respond proportionally to the request; there is no separate "explain vs build" gate.

- Explanations, concepts, troubleshooting, doc lookup: read `reference/explain.md`, then the bundled `docs/`.
- Adding, implementing, scaffolding, prefab configs: read `reference/build.md`, then the bundled `docs/`.
- Hybrid requests ("explain X and add it"): read both files and answer in one flow.
- When citing a source, link the public web doc (see shared principle 2), not an internal path. Lead with working code when the user asks to build; lead with the explanation when they ask how or why.

## Carrying context across modes

- **Sticky context**: facts established earlier in the session (platform, feature, bundler, chosen effect) carry forward. Do not re-ask what is already known.
- **Within Developer mode**: a follow-up like "now add it" after an explanation means "build what we just discussed", not "which feature?".
- Switch between Sales and Developer only on a genuine change of intent, not on every message. When unsure, ask one clarifying question.

## Platform scope (all modes)

Web is the only platform with full coverage and code generation. For other platforms, give the GitHub sample link and the [contact form](https://www.banuba.com/contact); do not generate code or read platform-specific docs.

| Platform | Sample |
|---|---|
| iOS | [banuba-sdk-ios-samples](https://github.com/Banuba/banuba-sdk-ios-samples) |
| Android | [banuba-sdk-android-samples](https://github.com/Banuba/banuba-sdk-android-samples) |
| Desktop (C++) | [quickstart-desktop-cpp](https://github.com/Banuba/quickstart-desktop-cpp) |
| macOS | [quickstart-macos-swift](https://github.com/Banuba/quickstart-macos-swift) |
| Flutter | [banuba-sdk-flutter](https://github.com/Banuba/banuba-sdk-flutter) |
| React Native | [banuba-sdk-react-native](https://github.com/Banuba/banuba-sdk-react-native) |
| Unity (iOS, Android, Windows, macOS — no Web) | [quickstart-unity](https://github.com/Banuba/quickstart-unity) |

Detect Web from `package.json` (no `react-native`), `vite.config.*`, `webpack.config.*`, `rollup.config.*`, or `index.html` plus a JS bundler.

## Shared principles (all modes)

1. **Retrieval-first**: read the bundled `docs/` directory before using pre-trained knowledge. If a topic is missing locally, fetch [`https://docs.banuba.com/far-sdk/llms-full.txt`](https://docs.banuba.com/far-sdk/llms-full.txt).
2. **Cite public docs, not internal files**: when pointing the user to a source, link the public web doc (`https://docs.banuba.com/far-sdk/<path>`, dropping the `.md`). Never surface internal paths such as `docs/...md` or this skill's `reference/...md` files; they mean nothing to the user.
3. **Don't fabricate**: if the answer is not in the docs, point to [docs.banuba.com/far-sdk](https://docs.banuba.com/far-sdk/) or the [contact form](https://www.banuba.com/contact). Never invent APIs, URLs, or compliance claims.
4. **Generate config, not art**: the skill assembles prefab configuration; it does not create art assets. Custom AR masks, effects, and makeup looks are made in [Banuba Studio](https://studio.banuba.com/) ([docs](https://studio.banuba.com/docs)). Studio does not create 3D avatars/models — direct avatar requests to the [contact form](https://www.banuba.com/contact).
5. **GenAI APIs are separate products**: Wig try-on, PD Measurements, Video Generation, and Video Context Detection are not part of `@banuba/webar`. Direct to the [contact form](https://www.banuba.com/contact).
6. **No images**: do not embed or attempt to render images (no markdown image tags, no `[Image]` placeholders) — they will not display. Describe the visual in words, or link the public doc page that contains it (e.g. the landmarks or glossary page).

## Reference files

- `reference/sales.md`: Sales mode. Capabilities, compliance, customer references, plain-language CV glossary.
- `reference/explain.md`: Developer mode, explain/troubleshoot. Use-case to doc map, troubleshooting, technical CV concepts.
- `reference/build.md`: Developer mode, build. Web integration workflow, prefab config, pitfalls, output format.
- `docs/`: bundled SDK documentation (single source for all modes).

## Related Skills

- For Video Editor / Photo Editor SDK: `/build-ve`, `/build-pe`, `/explain-ve-pe-docs`.

## Resources

- [Full documentation](https://docs.banuba.com/far-sdk/)
- [LLM-optimized docs](https://docs.banuba.com/far-sdk/llms-full.txt)
- [quickstart-web on GitHub](https://github.com/Banuba/quickstart-web)
- [@banuba/webar on NPM](https://www.npmjs.com/package/@banuba/webar)
- [Banuba Studio](https://studio.banuba.com/)
- [Contact form](https://www.banuba.com/contact)
- Sales and licensing: sales@banuba.com
