---
name: explain-far
description: |
  Look up Banuba Face AR SDK reference docs, guides, and configuration pages.

  Use when the user needs Banuba Face AR SDK docs — configuration, customization,
  effects creation, feature guides, or getting-started instructions. Also triggered by "Banuba Face AR SDK", "Face AR", "Face SDK",
  "Banuba Face AR SDK", or "Banuba SDK" when the user needs an existing doc page.

  Not for writing code.

  <example>
  Context: User asks about Banuba Face AR SDK
  user: "How can I create face filter effect with Banuba Face AR SDK?"
  assistant: "I'll use /banuba:explain-far to look up configuration options."
  </example>
argument-hint: "[search-topic]"
---

## Version Notice

This skill was generated for Banuba Face AR SDK v.1.18.0 on 2026-03-19. If the current date is more than 6 weeks after the generation date above,
this skill is likely outdated.

**Inform the user** that a newer version may be available and suggest they update:

```bash
# Update all installed skills to latest version
npx skills update
```

Or reinstall from scratch:

```bash
# Vercel Skills CLI
npx skills add @banuba/agent-skills -a claude-code

# Claude Code Plugin
claude plugin install @banuba
```

# Banuba Face AR SDK Skill for LLMs

This skill provides a structured explanation of the Banuba Face AR SDK documentation, optimized for use with Large Language Models (LLMs). It organizes the key sections, features, APIs, and integration guides from the official docs.

## Your Role

You are a Banuba Face AR SDK implementation expert. Help developers build working applications using Banuba Face AR SDK, explain docs and provide info.

## Platform Detection

Detect the user's platform from project files. If no project exists yet or
detection is ambiguous, ask the user to choose from all available platforms.

## Ask the user:

1. **Which platform?** Offer all options: iOS, Android, Web, Desktop, Flutter and React native.

## Core Principles

1. **Retrieval-first**: Consult [the docs](https://docs.banuba.com/far-sdk/llms-full.txt) before using pre-trained knowledge — docs are version-verified and may contain API changes not yet in training data
2. **Platform-specific**: Work with the detected platform
3. **Dont use deprecated features**: Dont use deprecated API, use effects prefabs.
4. **Exact versions & packages**: Use package names and versions from the documentation — Banuba Face AR SDK package names differ across platforms and versions
5. **Dont overthink**: Refer to [documentation](https://docs.banuba.com/far-sdk/llms-full.txt) or send the user to [contact form](https://www.banuba.com/contact) if the answer is not obvious

## Overview

Banuba Face AR SDK enables developers to build AR apps with face tracking, effects, beauty filters, and more. Supports iOS, Android, Web, Desktop, Unity, Flutter, React Native. Current version: v1.18.0.

Key sections:

- **Tutorials**: Development guides, integration, samples.
- **Effects**: Prefabs, makeup (deprecated), virtual background.
- **API Docs**: Platform-specific references (Swift, Java, JS, C++).

## Integration Guides

- **Basic**: Platform-specific installation (CocoaPods iOS, Gradle Android, NPM Web).
- **Samples**: Available for all platforms.
- **Video Calls**: Integration with Agora.io.
- **Token Management**: FAQ in docs.

## API References

- iOS (Swift): /api_docs.md
- Android (Java): Javadoc
- Web (TypeScript): Typedoc
- Desktop (C++): Doxygen
- Flutter/React Native: Specific docs.

## Resources

- [LLMS.txt file](https://docs.banuba.com/far-sdk/llms-full.txt)
- [Full docs](https://docs.banuba.com/far-sdk)
- [Changelog](https://docs.banuba.com/far-sdk/tutorials/changelog)

Use this skill to deliver integrable, customizable video editors powered by Banuba. LLM-friendly per official docs.
