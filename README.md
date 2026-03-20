# Banuba Video and Photo Editor SDKs Agent Skills

Give your AI coding assistant expert-level knowledge of Banuba Video and Photo Editor SDKs. Build photo editors and video editors by describing what you want.

## What Are Agent Skills?

[Agent Skills](https://agentskills.io) are portable knowledge packs that plug into AI coding assistants. By installing the Banuba Video and Photo Editor SDKs skills, you get:

- **Offline documentation**: All guides, API references, and best practices bundled locally — no external API calls
- **Guided code generation**: Build and explain skills that walk through Banuba Video and Photo Editor SDKs implementation step by step
- **Autonomous scaffolding**: A builder agent that creates complete Banuba Video and Photo Editor SDKs projects from scratch

## Available Skills

| Skill                | Description                                                                               |
| -------------------- | ----------------------------------------------------------------------------------------- |
| `build-ve`           | Implement features, write code, and set up Banuba Video Editor SDK projects               |
| `build-pe`           | Implement features, write code, and set up Banuba Photo Editor SDK projects               |
| `build-pe-with-docs` | Implement features, write code, and set up Banuba Photo Editor SDK projects by using docs |
| `build-ve-with-docs` | Implement features, write code, and set up Banuba Photo Editor SDK projects by using docs |
| `explain-ve`         | Explain how Banuba Video Editor SDK features work — concepts, architecture, workflows     |
| `explain-pe`         | Explain how Banuba Photo Editor SDK features work — concepts, architecture, workflows     |

The plugin also includes a **builder** agent that autonomously scaffolds complete Banuba Video Editor or Banuba Photo Editor applications — applying starter kit templates, and implementing features end-to-end.

## Setup Instructions

### Claude Code Plugin

Add the marketplace and install the plugin:

```bash
# Add the marketplace (one-time setup)
claude plugin marketplace add @banuba/agent-skills

# Install the plugin
claude plugin install @banuba
```

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

### Look up documentation

```
/banuba:explain-ve explain Banuba Video Editor SDK
/banuba:explain-pe explain Banuba Photo Editor
```

### Build a feature

```
/banuba:build-ve
/banuba:build-pe
/banuba:build-ve-with-docs
/banuba:build-pe-with-docs
```

### Explain a concept

```
/banuba:explain-ve how to create Banuba Video Editor application
/banuba:explain-pe how to create Banuba Photo Editor application
```

## How It Works

Each documentation skill bundles the complete Banuba Video and Photo Editor SDKs guides and Banuba Face AR SDK guide. Skills read directly from these local files — no external services or MCP servers are required.

The build skill includes starter kit templates for common use cases like video editors or photo editors. It detects your project's type and generates code accordingly.
