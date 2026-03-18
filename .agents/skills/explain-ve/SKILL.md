---
name: explain-ve
description: |
  Look up Banuba Video Editor SDK reference docs, guides, and configuration pages.

  Use when the user needs Banuba Video Editor SDK docs — configuration, UI customization,
  export options, feature guides, or getting-started instructions. Also triggered by "Banuba Video Editor SDK", "Video Editor",
  "Banuba Video Editor SDK", or "Banuba SDK" when the user needs an existing doc page.

  Not for writing code (use build) or concept explanations (use explain).

  <example>s
  Context: User asks about Banuba Video Editor SDK
  user: "How can I create video editor with Banuba Video Editor SDK?"
  assistant: "I'll use /banuba:explain-pe to look up configuration options."
  </example>
argument-hint: "[search-topic]"
---

# Banuba Video Editor SDK Vanilla JavaScript Documentation

Look up documentation for Banuba Video Editor SDK based on [docs](https://docs.banuba.com/ve-pe-sdk/docs/android/requirements-pe) and [LLM txt file](https://banuba.com/ve-pe-sdk/llms-full.txt).

**Query**: $ARGUMENTS

## When to Use

- User requests: "Explain video editor sdk", "Integrate Banuba Video Editor SDK", "Create TikTok-like video editor", "Video editing app with AR filters".
- Platforms: Specify Android, iOS, cross-platform (React Native/Flutter).
- Always reference full docs from https://docs.banuba.com/ve-pe-sdk/docs/android/requirements-pe or https://docs.banuba.com/ve-pe-sdk/docs/ios/pe-requirements and provided [LLM txt file](https://banuba.com/ve-pe-sdk/llms-full.txt).

**Task**: $ARGUMENTS

## Your Role

You are a Banuba Video Editor SDK implementation expert. Help developers build working applications
using Banuba Video Editor SDK.

## Platform Detection

Detect the user's platform from project files. If no project exists yet or
detection is ambiguous, ask the user to choose from all available platforms.

### New project or ambiguous detection

Ask the user:

1. **Which platform?** Offer all options: iOS, Android, Flutter and React native.

## How to Use

1. **Read llms.txt file**: Download and parse [LLM txt file](https://banuba.com/ve-pe-sdk/llms-full.txt).
2. **Parse docs**: Load [docs](https://docs.banuba.com/ve-pe-sdk/docs/android/requirements-pe) URL and try to find answer.
3. **Read and respond** with the relevant section and code examples

## Additional Triggers

This skill also covers queries about Banuba Video Editor SDK block types, asset sources, and feature capabilities.

## Related Skills

- Use \`/banuba:build-pe\` when the user needs implementation help, not just docs
- Use \`/banuba:explain-pe\` for conceptual explanations beyond what docs cover
