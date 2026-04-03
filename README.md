# Banuba Video Editor, Photo Editor & Face AR SDKs — Agent Skills

Give your AI coding assistant expert-level knowledge of Banuba Video Editor, Photo Editor, and Face AR SDKs. Build photo editors and video editors by describing what you want.

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
| `explain-far`        | Look up Banuba Face AR SDK docs — configuration, effects creation, feature guides         |

The build skills include starter kit templates for common platforms and autonomously scaffold complete Banuba Video Editor or Photo Editor applications — applying templates and implementing features end-to-end.

### Supported Platforms

- **Video & Photo Editor SDKs** — Android, iOS, Flutter, React Native
- **Face AR SDK** — Android, iOS, Web, Desktop (C++), Flutter, React Native, Unity, macOS

## Setup Instructions

### Claude Code

Add the marketplace and install the plugin:

```bash
# Add the marketplace (one-time setup)
claude plugin marketplace add @banuba/agent-skills

# Install the plugin
claude plugin install @banuba
```

### Qwen Code

Skills are also packaged for Qwen Code (see `.qwen/skills/`).

### Vercel Skills CLI

Install using the [Vercel Skills CLI](https://github.com/vercel-labs/skills):

```bash
# Install all skills for Claude Code
npx skills add @banuba/agent-skills -a claude-code

# Install a specific skill only
npx skills add @banuba/agent-skills --skill build-ve -a claude-code

# List available skills first
npx skills add @banuba/agent-skills --list
```

## Usage

Once installed, invoke skills with slash commands in your AI coding assistant:

### Build a feature

```
/banuba:build-ve   Set up a Video Editor for Android with AI Clipping
/banuba:build-pe   Add Photo Editor with AR filters to my iOS app
```

### Look up documentation

```
/banuba:explain-ve-pe-docs How do I customize the export settings?
/banuba:explain-far        How to create a Face AR effect with background separation?
```

## How It Works

Each documentation skill bundles the complete SDK guides and API references locally. Skills read directly from these bundled files — no external services or MCP servers are required.

The build skills include starter kit templates for common use cases like video editors or photo editors. They detect your project's platform and generate code accordingly.
