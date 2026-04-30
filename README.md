# Banuba Video Editor and Photo Editor SDKs — Agent Skills

[![Version](https://img.shields.io/badge/version-0.8.0-blue.svg)](VERSION)

Banuba Agent Skills are portable knowledge packs that install into Claude Code, Codex, and Qwen Code, giving the AI assistant offline access to Banuba Video Editor & Photo Editor documentation plus autonomous scaffolding for new VE and PE projects.

Give your AI coding assistant expert-level knowledge of Banuba Video Editor and Photo Editor SDKs. Build photo editors and video editors by describing what you want.

## What Are Agent Skills?

[Agent Skills](https://agentskills.io) are portable knowledge packs that plug into AI coding assistants. By installing the Banuba agent skills, you get:

- **Offline documentation** — all guides, API references, and best practices bundled locally — no external API calls
- **Guided code generation** — a step-by-step walkthrough and explanation of SDK implementation
- **Autonomous scaffolding** — a builder agent that creates complete Video Editor or Photo Editor projects from scratch

## Why Agent Skills??

Dropping a raw `llms.txt` dump into your assistant works once — but it bloats every prompt with the entire SDK surface, goes stale the moment Banuba ships a new release, and leaves the assistant to guess at structure. Agent skills are loaded **on demand** via slash commands, so context stays focused on the file you're actually editing. They're **versioned and pinned** to a specific SDK release, so generated code matches the API you're integrating against. And unlike a passive doc dump, skills carry **executable behavior** — the build skills can apply starter-kit templates, wire up dependencies, and scaffold an entire project, not just answer questions about one.

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
