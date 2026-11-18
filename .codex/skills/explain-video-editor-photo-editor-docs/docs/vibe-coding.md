# Agent Skills

> Agent Skills

# Agent Skills

# Build a Banuba Video Editor with AI


**In about an hour, you'll have a working Banuba Video or Photo Editor running in a local demo app on your laptop.**

You don't need to write code yourself. You'll use the **Claude desktop app** (in its **Code** mode), tell it what to build in plain English, and Banuba's official **AI Skills** will guide it to set up the SDK correctly.

**Difficulty:** ●●○○○ Beginner-friendly  
**Time:** ~1 hour  
**Works on:** Mac and Windows.

:::tip First time doing something like this?
That's exactly who this guide is for. You'll go through five prep steps and five build steps.

Every step ends with three checks so you always know whether you're on track:
- **Where am I?** What you just did.
- **Is it OK?** A clear finish line you can verify before moving on.
- **What's going on?** A short explanation of why this step matters.

If anything doesn't match what you see on your screen, you're not stuck, just **ask your AI agent (Claude Code, ChatGPT, Claude.ai, or Gemini) in plain English what to do.** That's a normal part of the process.

Stuck? Jump to [Troubleshooting](#troubleshooting).
:::

## What you'll have at the end

A working demo you can show to stakeholders, hand to a developer, or use to sharpen product requirements. Concretely:

- **A starter app** in your `banuba-demo` folder, ready to open in Android Studio or Xcode.
- **A running Banuba editor** on your simulator or emulator, where you can trim clips, apply filters, add AR effects, and export the result.
- **SDK-aligned demo code** generated with Banuba's official AI Skill instructions, released with every SDK update so the agent stays aligned with the latest integration flow.

### Two common scenarios

* **No app yet?**  
  This guide assumes this scenario. You'll create a new starter app from scratch with help from the AI agent.

* **Already have an app?**  
  Point the agent at your existing project folder and ask it to add Banuba Video Editor or Photo Editor as a module inside your app.

## Who this guide is for

| If you're a… | This guide helps you… |
|---|---|
| **Application owner** evaluating Banuba | See a working demo before committing engineering resources |
| **Product manager** scoping a video or photo editing feature | Validate the user flow and understand what is required for a working integration |
| **Project manager** planning the integration | Estimate scope, prepare the right inputs, and brief engineering more clearly |
| **Developer** with the integration ticket | Get SDK-specific answers faster, avoid long documentation searches, and start from guided integration steps instead of a blank project |

## How it works

Three pieces work together:

1. **Banuba SDK**. The Video Editor or Photo Editor that runs inside your app. Requires a Banuba license key. Get a free trial with no payment required.
2. **AI coding agent**. A tool that can create and edit code based on your instructions. We recommend **Claude Code**. **Codex** and **Qwen Code** can also work; see [Alternative agents](#alternative-agents).
3. **Banuba's AI Skills**. Official instructions that teach the agent how to integrate Banuba SDKs correctly. Released with every SDK update.

You install the agent once, install Banuba's AI Skills with **one short command** (the only command-line step in this guide), and then ask in plain English:

> Set up a Banuba Video Editor demo for iOS and/or Android.

The agent creates the project and guides you through the build.

To get there, you'll use **Terminal** a few times at the beginning. Don't worry, you won't need to understand coding commands. You'll only copy commands from this guide, paste them into Terminal, and press Enter.

:::tip What's "Terminal"?
Terminal is a small text-window app that already comes with your computer. On Mac, it is called **Terminal**. On Windows, you can use **PowerShell (as admin)** instead.

You type a command, press Enter, and the command runs. That's all you need for this guide. If something goes wrong, nothing in this guide is designed to change your personal files or system settings without you seeing it first. You can stop at any time and jump to [Troubleshooting](#troubleshooting).
:::

---

## Prep - 15 minutes

:::tip Never used Terminal before?
Terminal is the text-window app where you type commands.
- **Mac:** Press `⌘ + Space`, type `Terminal`, press Enter.
- **Windows:** Press the Windows key, type `Terminal` or `PowerShell`, press Enter.

You type a command, press Enter, and the command runs. That's all you need for this guide.
:::

### 1. Get a Banuba license key

Without a license key, the SDK won't run, even in a trial build. Get yours first.

1. Open [Video Editor SDK for Android & iOS](https://www.banuba.com/video-editor-sdk#form) (the page will scroll directly to the trial form).
2. Click the **Get a Free Trial** button. No payment information is required.
3. Fill in the short form (name, email).
4. **You'll receive a license key token immediately on the next page.** It's a long alphanumeric string - copy it and save it somewhere safe (a password manager or a private note).

:::note
Don't worry, submitting the form gives you the token right away. You won't get a sales call before you can try the SDK.
:::

**Where am I?** You have your Banuba license key saved somewhere safe.  
**Is it OK?** Yes, if you have a long alphanumeric token in hand.  
**What's going on?** The token authorizes the SDK to run during your trial.

### 2. Pick your target platform

Choose one platform to start with. You can repeat the process for another platform later.

| Platform | What you need | Pick if… |
|---|---|---|
| **Android** | Android Studio (free), about 30 GB of free disk space | You do not have a Mac, or you want the simplest first setup |
| **iOS** | A Mac and Xcode | You are targeting iPhone or iPad first |
| **Flutter** | Flutter SDK + Android Studio and/or Xcode | You want one codebase for iOS and Android |
| **React Native** | Node.js + Android Studio and/or Xcode | You already have a React Native app |

:::warning Using an older Mac?
Banuba SDKs currently require **Xcode 15 or higher**, which needs **macOS Ventura 13.5 or newer**. If your Mac runs an older macOS, you can still build for **Android** without limitations.
:::

### 3. Install the platform toolchain

Install only the toolchain for the platform you picked.

- **Android Studio:** [https://developer.android.com/studio](https://developer.android.com/studio)
- **Xcode:** Open the App Store on Mac, search for Xcode, and install it
- **Flutter:** [https://docs.flutter.dev/get-started/install](https://docs.flutter.dev/get-started/install)
- **React Native:** [https://reactnative.dev/docs/environment-setup](https://reactnative.dev/docs/environment-setup)

**Where am I?** You have the IDE for your platform installed.  
**Is it OK?** Open it once and accept any first-launch prompts. On Android Studio, let it download the default Android SDK when it asks.  
**What's going on?** This is the visual app where you'll run your demo at the end.

**Done when:** you can run a sample project on a simulator or emulator.

### 4. Install Node.js

Node.js is required to run the Banuba AI Skills installer in the next section.

1. Open [https://nodejs.org](https://nodejs.org) and click the **LTS** download button for your OS.
2. Run the installer. **Accept the default options**, they're correct.
3. When the installer finishes, **close the Node.js installer window**. You won't type any commands in it.

Now open your Terminal (Mac) or PowerShell-as-admin (Windows) and run:

```bash
node --version
```

:::tip Heads up for Windows users
If you opened Terminal before installing Node.js, close it and reopen it (still as admin). Windows needs a fresh Terminal window to "see" the new install.
:::

**Where am I?** You have Node.js installed.  
**Is it OK?** Terminal returns a version like `v20.x.x` or higher.  
**What's going on?** Node.js gives you `npm` and `npx` - small tools we'll use to install the Banuba AI Skills in the next steps.

**Done when:** Terminal returns a version number, for example `v20.x.x` or higher.

### 5. Install Claude Code desktop app

The Claude desktop app is the visual app where you'll do all the actual building. It includes a **Code** mode that wraps Claude Code's full capabilities in a friendly chat interface - no Terminal required.

1. Download the Claude desktop app from [https://claude.ai/download](https://claude.ai/download) (Mac and Windows versions are available).
2. Install it, sign in with your Anthropic account, and open it once to confirm it launches.
3. In the top-left of the app window, click the **Code** tab. You should see a clean dashboard with **New session**, **Routines**, **Customize**, and **More** in the left sidebar.

**Where am I?** The Claude desktop app is installed and you can see the Code tab.  
**Is it OK?** The Code tab opens without errors. A model selector (e.g., Sonnet 4.6) is visible at the bottom-right.  
**What's going on?** This is the surface where you'll chat with the AI agent, **pick the project folder**, and watch the build happen. You won't touch Terminal again after one more step.

---

## ✅ Prep checkpoint

You're ready to build when **all four** of these are true:

- Node.js installed: `node --version` returns a version number
- Android Studio / Xcode / Flutter / React Native tooling launches without errors
- The Claude desktop app launches and the Code tab is visible
- You have your Banuba license key saved somewhere you can paste from later

If any of these fails, don't push through - **ask the Claude desktop app, ChatGPT, Claude.ai, or Gemini what's wrong** in plain English. Paste the error message you see. They'll usually identify the issue in one round.

---

## Build - five steps to a working Video and Photo Editor

### 1. Create a project folder *(1 min)*

Create an empty folder where Claude will build your demo. **Use your normal file manager, no command line.**

**Mac (Finder):**
1. Open **Finder**.
2. Press `⇧ + ⌘ + H` to go to your home folder (the one named after your username).
3. Choose **File → New Folder**, name it `banuba-demo`.

**Windows (File Explorer):**
1. Open **File Explorer**.
2. Navigate to your user folder (e.g., `C:\Users\YOUR-NAME\`).
3. Right-click in an empty area → **New → Folder**, name it `banuba-demo`.

**Where am I?** An empty `banuba-demo` folder exists in your home folder.  
**Is it OK?** You can see the folder in Finder / File Explorer.  
**What's going on?** This is where Claude will write your demo app. Remember the path - you'll point both the Claude desktop app and Android Studio / Xcode at it.

### 2. Install Banuba's AI Skills *(2 min)*

This is the **last command-line step in the entire guide.** One short command, paste-and-press-Enter. After this, you're back in the visual Claude desktop app for everything else.

**Mac (Terminal):**
1. Press `⌘ + Space`, type `Terminal`, press Enter.
2. Paste this single line and press Enter:

```bash
npx skills add Banuba/ai-skills -a claude-code
```

**Windows (PowerShell as administrator):**
1. Press **Win + R**, type `powershell`, then press **Ctrl + Shift + Enter** to launch PowerShell **as administrator**. Click **Yes** on the permission prompt. Without admin rights, the installer can be blocked by Windows.
2. Paste this single line and press Enter:

```bash
npx skills add Banuba/ai-skills -a claude-code
```

**When the interactive list of skills appears**, the installer is asking you which ones to install:
- **↑ / ↓ arrows** - move between skills
- **Space** - select / deselect a skill (a checkmark appears next to selected ones)
- **a** - select all (Recommended)
- **Enter** - confirm

✅ **Recommendation: select all skills.** They're small, and you get maximum flexibility: Video Editor, Photo Editor, and inline docs lookup. Press `a`, then Enter.

The installed skills include:

- `build-video-editor` - helps set up Banuba **Video Editor** SDK
- `build-photo-editor` - helps set up Banuba **Photo Editor** SDK
- `explain-video-editor-photo-editor-docs` - helps the agent answer SDK documentation questions

The skills are downloaded into a hidden `.claude/skills/` folder inside your project.

**Where am I?** Banuba's AI Skills are installed on your computer, where the Claude desktop app can find them.  
**Is it OK?** Terminal printed a confirmation listing the skills you selected. No red error text.  
**What's going on?** You just finished the only command-line step. From now on, everything happens inside the Claude desktop app: chat input, slash commands, file changes - all visual.

You can close the Terminal / PowerShell window now. You won't need it again.

:::tip Want to try skipping Terminal entirely?
Open the Claude desktop app → **Code** tab → **New session**. In the chat, type: "Install Banuba's AI Skills from the `Banuba/ai-skills` repo so I can use the build-video-editor skill." If Claude installs them and confirms success, you can skip the Terminal step above. If it can't (you'll see an error or a message that it lacks the tools), no problem - fall back to the one-line command above. Either path lands you in the same place.
:::

:::warning Windows users - a quick note on command syntax
If you ever need to chain Windows commands together, **PowerShell / CMD don't accept `&&`** (it's a Unix-only operator). Use separate lines, or `;` in PowerShell. This guide gives you commands one at a time so you won't hit this, but if you find other guides online that use `&&`, that's why it fails on Windows.
:::

### 3. Open the Claude desktop app and point it at your folder *(1 min)*

Back in the visual world. From here, you live in the Claude desktop app.

1. Open the **Claude desktop app** and click the **Code** tab in the top-left.
2. At the bottom of the chat area, click **Select folder…** and choose your `banuba-demo` folder.
3. Click **New session** in the left sidebar.

**Where am I?** A new Claude Code session is open, pointed at your `banuba-demo` folder. The chat input is ready for you to type.  
**Is it OK?** The folder name appears near the chat input (next to the **Local** label). The model selector at the bottom-right shows the model you're using, for example, **Sonnet 4.6**.  
**What's going on?** Because the session is pointed at `banuba-demo`, the agent can see Banuba's AI Skills you installed in Step 2 and will use them automatically when you ask it to build.

### 4. Ask the agent to build *(10–30 min)*

In the chat input, type one of the prompts below. Replace `[Android]` / `[iOS]` with your platform, and `[paste your token]` with your real Banuba license key.

**For Video Editor:**

```
/build-video-editor Set up a Banuba Video Editor demo for [Android].
Banuba license key: [paste your token]
```

**For Photo Editor:**

```
/build-photo-editor Set up a Banuba Photo Editor demo for [iOS] with photo effects and AR filters.
Banuba license key: [paste your token]
```

:::tip
Start typing `/` in the chat input - the desktop app will list available slash commands, including the Banuba ones, so you don't have to remember them.
:::

:::tip Expect Claude to ask you questions, that's normal.
It might ask things like:
- "Which export resolution do you want?"
- "Should the editor open the camera by default, or open the gallery?"
- "May I create this file? May I run this command?"

Answer in plain English. If you're not sure, use one of these replies:
- "Use the defaults."
- "Pick what you recommend for a first demo."
- "Yes, you can create that file." / "Yes, you can run that command."

If a question is confusing, ask Claude to explain it in simpler words. You can write: `Explain this in simpler words. I'm not technical.`
:::

Claude Code may also ask for permission to create files, edit files, or run commands. Review the request and approve it if it matches the setup you asked for. In Claude Code, the **Accept edits** button is where you confirm file changes.

**Where am I?** Claude has scaffolded a starter project and written the integration code into your `banuba-demo` folder.  
**Is it OK?** Claude reports that the build is finished and tells you how to run the app. There are no red error messages. The folder now contains real project files, such as `gradle`, `app`, `ios`, or similar names depending on your platform.  
**What's going on?** Banuba AI Skills guided Claude on which starter template to use and how to configure the SDK. Your Banuba license key has been added to the project configuration so the demo can run.

### 5. Run it *(5 min)*

🎯 **You're done with Claude Code for now.** The final step happens in Android Studio or Xcode.

#### 5a. Open the project in your IDE

Open the IDE that matches your platform:

- **Android, Flutter, React Native (Android side):** Android Studio
- **iOS, Flutter (iOS side), React Native (iOS side):** Xcode

In the IDE, choose **File → Open**, then navigate to your `banuba-demo` folder.

If you created the folder in the default location, it is usually here:

**Mac:** `/Users/YOUR-NAME/banuba-demo`  
**Windows:** `C:\Users\YOUR-NAME\banuba-demo`

Select the folder and click **Open**.

#### 5b. Wait for the project to load

The first load may take a few minutes. The IDE needs to download dependencies and prepare the project before you can run it.

- **Android Studio:** Look at the bottom status bar. It may say **Gradle sync in progress**. Wait until sync finishes. This usually takes 1–5 minutes the first time.
- **Xcode:** Wait for **Indexing** and package resolution to finish. Let Xcode finish preparing the project before clicking Run.

**Where am I?** The IDE is showing your project files on the left and source code in the middle.  
**Is it OK?** The status bar says sync, indexing, or package resolution is finished. There are no red error messages at the bottom.  
**What's going on?** The IDE is downloading and preparing the libraries your project needs, including Banuba SDK dependencies.

#### 5c. Create or select a virtual device

To run the demo, you need a virtual phone on your laptop.

- For Android, this is called an **emulator**.
- For iOS, this is called a **simulator**.

**Android Studio - Android emulator:**
1. Click **Tools → Device Manager**.
2. Click **Create device**.
3. Choose a recent phone, such as **Pixel 9**.
4. Click **Next**.
5. Choose a recent system image, such as **API 34 / Android 14**.
6. Click **Download** if needed.
7. Click **Next**, then **Finish**.

**Xcode - iOS simulator:**
1. Open **Window → Devices and Simulators**.
2. Check that at least one iPhone simulator is available.
3. If no simulator is listed, click **+** and add a recent iPhone, such as **iPhone 16**.

**Where am I?** You have at least one virtual device available in your IDE.  
**Is it OK?** The device appears in the device dropdown near the **Run** button.  
**What's going on?** When you click Run, the IDE will install your demo app onto this virtual device.

#### 5d. Click Run

At the top of the IDE, find the green **Run** button.

1. Select your virtual device in the dropdown next to the Run button.
2. Click **Run**.

The first build usually takes a few minutes. After that, the simulator or emulator opens and your Banuba editor launches inside it.

**Where am I?** The Banuba editor is running on a virtual phone on your laptop.  
**Is it OK?** You can record or import media, trim, apply filters or effects, add AR effects, and export the result.  
**What's going on?** This is a working demo of Banuba Video Editor or Photo Editor SDK running inside a starter app. It is suitable for stakeholder review, product scoping, and developer handoff before production integration.

---

## 🎉 You now have a working Banuba editor.

Congrats! The app running on your simulator or emulator is a real, end-to-end Banuba editor. You can:

- Record or import a video or photo
- Trim clips, apply filters, and add AR effects
- Export the result

**What this is:** A working demo. The integration code is real, the SDK is the real Banuba SDK, and the editor is functional.

**What this is not yet:** Your production app. Right now, it is a starter app with Banuba Video Editor or Photo Editor inside.

## From here, you can start shaping the demo into your real product.

Go back to your **Claude Code session**. It should still be open. Keep talking to the agent and ask for small changes, one step at a time:

- Add a music library to the editor.
- Remove the beauty filters tab.
- Make export the primary action.
- Add a sign-in screen before the editor opens.
- Wire this editor into a new screen inside my existing app at `[path-to-your-app]`.

Each prompt moves the demo closer to your product. Repeat as many times as you need.

:::tip Not sure what to ask next?
Ask Claude directly:
- "I want this editor to feel like a modern short-form video editor. What should I change first?"
- "What is the simplest way to add my logo to this editor?"
- "What should I customize before showing this demo to stakeholders?"
:::

---

## What to do next

| If you're a… | Do this |
|---|---|
| **Application owner** | Record a short screen capture and align stakeholders on what matters before scoping production work. |
| **Product manager** | Iterate by re-prompting the agent. For example: "Add a music library," "Remove the beauty filters tab," or "Make export the primary action." |
| **Project manager** | Use the [handoff template](#developer-handoff-template) below to brief engineering clearly. |
| **Developer** | Use inline docs lookup when needed: `/explain-video-editor-photo-editor-docs How does export work?` Then connect the editor to the real app flow. |

### Developer handoff template

Paste this into a ticket or Slack message:

>
>
> **Banuba Video/Photo Editor demo is ready for engineering review**
>
> **What's done:**  
> A working demo was scaffolded with Banuba's `[build-video-editor / build-photo-editor]` AI Skill for `[platform]`.
>
> **Source:**  
> `[path or repo link]`
>
> **License key:**  
> Stored in `[vault or secure location]`.  
> Do not paste the license key directly into the ticket or Slack thread.
>
> **Engineering, please review and complete:**
>
> - Review the generated integration against our app architecture
> - Replace the trial license with the correct production token when appropriate
> - Wire the editor into `[feature / screen / user flow]`
> - Confirm required permissions, media access, and export behavior
> - Run our security, privacy, accessibility, and QA review
> - Confirm the integration matches Banuba's latest SDK documentation
>
> **References:**
>
> - Banuba SDK documentation: [https://docs.banuba.com/ve-pe-sdk](https://docs.banuba.com/ve-pe-sdk)
> - For inline documentation lookups in Claude Code, use: `/explain-video-editor-photo-editor-docs [your question]`

---

## Troubleshooting

Most issues come from one of these: the one Terminal step was skipped or blocked, the Claude desktop app isn't pointed at the right folder, or no virtual device exists yet.

| Symptom | Cause | Fix |
|---|---|---|
| `npx: command not found` (during the one Terminal step) | Node.js not installed, or Terminal hasn't refreshed | Install Node.js from [https://nodejs.org](https://nodejs.org). **Close and reopen Terminal** (Windows: as admin) and try the command again. |
| Windows: `'&&' is not a valid statement separator` | A command used `&&`, which Windows PowerShell / CMD doesn't accept | Run each command on its own line. Or use `;` in PowerShell. |
| Windows: installer says "access denied" or "permission denied" | Terminal wasn't launched as administrator | Close Terminal. Press `Win + R` → type `powershell` → press **Ctrl + Shift + Enter** to relaunch as admin. |
| You asked Claude to install the skills, but it said it couldn't | The agent may not have the system-level tools to install skills in your setup | Fall back to the one-line Terminal command in Step 2. Reliable on every machine. |
| `/build-video-editor` doesn't appear in the slash command list | Claude desktop app session isn't pointed at the folder where the skills were installed | In the desktop app, click **Select folder…** at the bottom and pick `banuba-demo`. Start a new session. |
| Banuba AI Skills installed, but Claude says it can't find them | The session may have started before the skills were installed | Close the session, then start a new session pointed at `banuba-demo`. Skills are picked up at session start. |
| IDE opens the project, but Run button is greyed out / "no device selected" | No virtual device created yet | **Android Studio:** Tools → Device Manager → Create Virtual Device. **Xcode:** Window → Devices and Simulators → add a simulator. Then pick it in the Run dropdown. |
| Build done, but the simulator won't launch | Platform toolchain isn't fully set up, or Gradle/Pod sync didn't finish | Wait for sync to fully finish, then click Run again. If still failing, open Build → Build output and read the error. Paste it back into the Claude desktop app. |
| License error at runtime | Token pasted wrong or trial expired | Request a fresh key from [banuba.com/video-editor-sdk](https://www.banuba.com/video-editor-sdk#form). Then tell Claude: "Update the Banuba license key in this project." Paste the new key only when Claude asks. |
| Where's my project folder? | You don't remember where `banuba-demo` was created | Check `~/banuba-demo` (Mac) or `C:\Users\YOUR-NAME\banuba-demo` (Windows). Or use Finder/File Explorer's search. |
| App builds, but camera or media access doesn't work | Permissions missing or blocked | In the Claude desktop app, ask: "Check camera, microphone, and media permissions for this Banuba editor demo." |
| Export doesn't work | Export config or simulator limitation | In the Claude desktop app, ask: "Check why export isn't working and verify the export configuration." |

**Still stuck?**  
[Open an issue](https://github.com/Banuba/ai-skills/issues) or [contact Banuba support](https://www.banuba.com/contact).

Include:

- Your target platform: Android, iOS, Flutter, or React Native
- Your SDK version, if known
- The command or prompt you used
- The full error message or screenshot
- Whether you are using a simulator, emulator, or real device

Do **not** paste your Banuba license key into public issues, Slack channels, or support screenshots.

---

## Reference

### All available skills

| Skill | What it does | Example prompt |
|---|---|---|
| `build-video-editor` | Sets up Banuba Video Editor SDK end-to-end | "Set up a short-form video editor with trimming, filters, AR effects, and export." |
| `build-photo-editor` | Sets up Banuba Photo Editor SDK end-to-end | "Set up a photo editor with effects, filters, and AR features." |
| `explain-video-editor-photo-editor-docs` | Answers questions using Banuba Video Editor and Photo Editor SDK documentation | "How do I change the export resolution?" |

The build skills include starter templates for **Android, iOS, Flutter, and React Native**. The agent can detect the platform from your prompt or project structure.

The skills run locally inside your AI coding agent. You do not need to paste long SDK documentation into every prompt, and the skills are kept aligned with Banuba SDK releases.

### Alternative agents {#alternative-agents}

| Agent | Install command (use in Terminal once) |
|---|---|
| **Claude Code** *(recommended)* | `npx skills add Banuba/ai-skills -a claude-code` |
| Codex | `npx skills add Banuba/ai-skills -a codex` |
| Qwen Code | `npx skills add Banuba/ai-skills -a qwen-code` |

Skills are installed into the agent-specific skills folder, such as `.claude/skills/`, `.codex/skills/`, or `.qwen/skills/`.

### No agent setup?

**Use the LLM-ready documentation files instead:**

- [llms.txt](https://docs.banuba.com/ve-pe-sdk/llms.txt) - shorter version for quick context
- [llms-full.txt](https://docs.banuba.com/ve-pe-sdk/llms-full.txt) - complete version for deeper analysis

You can paste these files into ChatGPT, Claude.ai, Gemini, or another LLM.

This option is slower and will not create or edit files on your computer, but it is useful for a first look, product scoping, or asking documentation questions without setting up an AI coding agent.

### Glossary

- **Agent skill** - Packaged instructions that teach an AI coding agent how to complete a specific task correctly.
- **License key** - The Banuba credential that unlocks the SDK. It is required to run the SDK, including trial builds.
- **npx** - A tool that comes with Node.js. It can run an installer command without permanently installing that installer on your computer. In this guide, you use it to install Banuba AI Skills.
- **Repo (GitHub repo)** - A folder of code hosted on GitHub. For this guide, you do not need to open GitHub manually - the install command fetches the required files for you.
- **Simulator / emulator** - A virtual phone running on your laptop. It lets you test an app without using a real device.
- **Slash command** - A command inside an AI agent starting with `/`, like `/build-video-editor`.
- **Terminal** - The text-window app where you type commands. On Mac, it is called Terminal. On Windows, you can use Terminal or PowerShell.
- **IDE** - An app developers use to open, run, and edit projects. Examples include Android Studio, Xcode, Visual Studio Code, and WebStorm.
