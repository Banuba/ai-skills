# Banuba Video Editor and Photo Editor SDKs — Agent Skills

Give your AI coding assistant expert-level knowledge of Banuba Video Editor and Photo Editor SDKs. Build photo editors and video editors by describing what you want.

## What Are Agent Skills?

[Agent Skills](https://agentskills.io) are portable knowledge packs that plug into AI coding assistants. By installing the Banuba agent skills, you get:

- **Offline documentation** — all guides, API references, and best practices bundled locally — no external API calls
- **Guided code generation** — build and explain skills that walk through SDK implementation step by step
- **Autonomous scaffolding** — a builder agent that creates complete Video Editor or Photo Editor projects from scratch

## Available Skills

| Skill                | Description                                                                               |
| -------------------- | ----------------------------------------------------------------------------------------- |
| `build-ve`           | Implement features, write code, and set up Banuba Video Editor SDK projects               |
| `build-pe`           | Implement features, write code, and set up Banuba Photo Editor SDK projects               |
| `explain-ve-pe-docs` | Look up Banuba Video and Photo Editor SDKs docs — configuration, UI customization, guides |

The build skills include starter kit templates for common platforms and autonomously scaffold complete Banuba Video Editor or Photo Editor applications — applying templates and implementing features end-to-end.

### Supported Platforms

- **Video & Photo Editor SDKs** — Android, iOS, Flutter, React Native

### SDK Versions

- Video Editor SDK v1.51.0
- Photo Editor SDK v1.51.0

## Setup Instructions

### Claude Code

Install using the [Vercel Skills CLI](https://github.com/vercel-labs/skills):

```bash
# Install all skills
npx skills add @banuba/agent-skills -a claude-code

# Or install a specific skill only
npx skills add @banuba/agent-skills --skill build-ve -a claude-code

# List available skills first
npx skills add @banuba/agent-skills --list
```

Skills are installed into the `.claude/skills/` directory and loaded automatically.

### Codex

Install using the [Vercel Skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add @banuba/agent-skills -a codex
```

Skills are installed into the `.codex/skills/` directory.

### Qwen Code

Install using the [Vercel Skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add @banuba/agent-skills -a qwen-code
```

Skills are installed into the `.qwen/skills/` directory.

## Usage

Once installed, invoke skills with slash commands in your AI coding assistant:

### Build a feature

```
/build-ve   Set up a Video Editor for Android with AI Clipping
/build-pe   Add Photo Editor with AR filters to my iOS app
```

### Look up documentation

```
/explain-ve-pe-docs How do I customize the export settings?
```

## How It Works

Each documentation skill bundles the complete SDK guides and API references locally. Skills read directly from these bundled files — no external services or MCP servers are required.

The build skills include starter kit templates for common use cases like video editors or photo editors. They detect your project's platform and generate code accordingly.

## Project Structure

```
.agents/skills/       Portable skill definitions (SKILL.md per skill)
.claude/skills/       Claude Code skills + bundled documentation
.codex/skills/        Codex skills + bundled documentation
.qwen/skills/         Qwen Code skills + bundled documentation
```

The `.agents/` directory contains tool-agnostic skill definitions. The `.claude/`, `.codex/`, and `.qwen/` directories each contain the same skills and documentation packaged for their respective AI coding assistant.
