# Banuba Agent Skills

This repository contains AI coding assistant skills for Banuba Video Editor and Photo Editor SDKs. Skills are portable knowledge packs that provide offline documentation and guided code generation.

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
| `build-ve`           | Builder     | Scaffold and implement Banuba Video Editor SDK projects            |
| `build-pe`           | Builder     | Scaffold and implement Banuba Photo Editor SDK projects            |
| `explain-ve-pe-docs` | Docs lookup | Look up VE/PE SDK configuration, customization, and feature guides |

## SDK Versions

- Video Editor SDK v1.51.0
- Photo Editor SDK v1.51.0

## Supported Platforms

- **Video & Photo Editor SDKs**: Android, iOS, Flutter, React Native

## Key Conventions

- Skills are installed via `npx skills add @banuba/agent-skills -a <assistant>`.
- Each SKILL.md file contains a YAML frontmatter block (`name`, `description`, `argument-hint`) followed by the skill prompt.
- Builder skills (`build-ve`, `build-pe`) fetch live docs from `https://banuba.com/ve-pe-sdk/llms-full.txt` and clone integration samples from GitHub.
- Docs lookup skill (`explain-ve-pe-docs`) read from bundled `./docs/` directories within each skill folder.
- All four skills must stay in sync across `.agents/`, `.claude/`, `.codex/`, and `.qwen/` directories.

## When Editing Skills

- Keep SKILL.md frontmatter format consistent: `name`, `description`, `argument-hint`.
- After changing a skill in one directory, replicate the change to all other assistant directories.
- Do not modify generated API docs under `docs/generated/` — those are auto-generated from SDK source.
