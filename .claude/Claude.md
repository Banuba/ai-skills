# Banuba Agent Skills

This repository contains AI coding assistant skills for Face AR SDKs, Banuba Video Editor and Photo Editor SDKs. Skills are portable knowledge packs that provide offline documentation and guided code generation.

## Project Structure

```
.agents/skills/       Portable, tool-agnostic skill definitions (SKILL.md per skill)
.claude/skills/       Claude Code skills + bundled documentation
.codex/skills/        Codex skills + bundled documentation
.qwen/skills/         Qwen Code skills + bundled documentation
```

Each skill is defined once in `.agents/` and replicated with platform-specific packaging into `.claude/`, `.codex/`, and `.qwen/`.

## Skills

| Skill                | Type        | Purpose                                                            |
| -------------------- | ----------- | ------------------------------------------------------------------ |
| `far-general`        | Unified     | Banuba Face AR SDK: sales, dev (docs), and integration (Web, Android, iOS, Desktop) modes |
| `build-video-editor`           | Builder     | Scaffold and implement Banuba Video Editor SDK projects            |
| `build-photo-editor`           | Builder     | Scaffold and implement Banuba Photo Editor SDK projects            |
| `explain-video-editor-photo-editor-docs` | Docs lookup | Look up VE/PE SDK configuration, customization, and feature guides |

## SDK Versions

- Face AR SDK: v1.18.2 (native: Web/Android/iOS/Desktop), Flutter: v3.1.1, React Native: v2.0.1
- Video Editor SDK
  - Android: v1.53.2
  - iOS: v1.53.2
  - Flutter: v0.45.0
  - React Native: v0.52.0
- Photo Editor SDK
  - Android: v1.4.1
  - iOS: v1.4.1
  - Flutter: v0.6.0
  - React Native: v0.7.0

## Supported Platforms

- **Video & Photo Editor SDKs**: Android, iOS, Flutter, React Native
- **Face AR SDK** - Web, Android, iOS, Desktop C++, Flutter, React Native

## Key Conventions

- Skills are installed via `npx skills add Banuba/ai-skills -a <assistant>`.
- Each SKILL.md file contains a YAML frontmatter block (`name`, `description`, `argument-hint`) followed by the skill prompt.
- Builder skills (`build-video-editor`, `build-photo-editor`) fetch live docs from `https://banuba.com/ve-pe-sdk/llms-full.txt` and clone integration samples from GitHub.
- Docs lookup skill (`explain-video-editor-photo-editor-docs`) read from bundled `./docs/` directories within each skill folder.
- All four skills must stay in sync across `.agents/`, `.claude/`, `.codex/`, and `.qwen/` directories.

## When Editing Skills

- Keep SKILL.md frontmatter format consistent: `name`, `description`, `argument-hint`.
- After changing a skill in one directory, replicate the change to all other assistant directories.
- Do not modify generated API docs under `docs/generated/` - those are auto-generated from SDK source.
