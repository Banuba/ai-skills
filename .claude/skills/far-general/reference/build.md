# Developer mode: Web build reference

Read this when the Developer-mode request is to add, implement, set up, integrate, or build something with Banuba Face AR SDK on the Web. Platform detection, the non-Web hand-off, and shared principles are in the router (`SKILL.md`); this file covers the Web build workflow.

## Your role

Banuba Face AR SDK implementation expert. Build working Web applications using `@banuba/webar`. Lead with working code, then explain.

## Prerequisites

1. Banuba client token (mandatory; SDK will not run without it).
2. [Node.js](https://nodejs.org/) installed.
3. Browser with [WebGL 2.0](https://caniuse.com/#feat=webgl2) support.
4. A bundler that imports binary assets as URLs (Vite, Rollup, Webpack), or plain `<script type="module">` via the jsDelivr CDN for prototyping.

## Default effects policy

**When the user creates a new project OR asks to add effects without listing specific ones**, include the full set of publicly available demo effects. The canonical source is:

> https://docs.banuba.com/far-sdk/tutorials/capabilities/demo_face_filters

That page is also bundled locally at `docs/tutorials/capabilities/demo_face_filters.md`. It contains the complete list (40+ effects). Use the bundled file first; if it seems outdated, fetch the live URL.

Each effect entry provides:
- **Download URL**: `https://docs.banuba.com/far-sdk/generated/effects/<filename>.zip`
- **Icon URL**: `https://docs.banuba.com/far-sdk/generated/effects/icons/<icon_filename>.png`
- **Description** (Technologies represented column)
- **Required modules** (Required packages column)

When generating the effect list in code, emit every row from the table — not a representative sample. Build the UI so each effect shows its **icon**, **name**, and **description**. Load only the union of required modules for the active effect on swap; do not preload all modules at once.

If the user specifies a subset of effects explicitly, use only that subset. Otherwise, default to the full list.

## Workflow

### 1. Pick the integration path

Detect from workspace files, or ask if unclear.

**Path A, new project** (no `package.json`): clone the quickstart as the scaffold.

```bash
git clone https://github.com/Banuba/quickstart-web
cd quickstart-web && npm install
```

Replace the placeholder in `BanubaClientToken.js` with the real token, run `npm start`, verify the demo loads.

**Path B, existing project** (already has `package.json`, source files, bundler config): install the SDK into the user's project. Do not clone over it.

```bash
npm install @banuba/webar
```

Treat [quickstart-web](https://github.com/Banuba/quickstart-web) as a reference for patterns. Files worth mirroring:

- `BanubaPlayer.js`: the Player lifecycle (Player.create, Module.preload, applyEffect, Webcam, Dom.render). Adapt to the user's structure and drop unused modules.
- `range-requests.sw.js`: only include this if the project loads video textures via external HTTP URLs. Do **not** copy it for projects using zip-packed effects (all demo effects) — combining it with `proxyVideoRequestsTo` breaks blob-URL video textures on Safari. See the "Safari animation delay" pitfall.
- Token-in-separate-file pattern (`BanubaClientToken.js` exporting the token), kept out of source control via `.gitignore`.

To inspect a quickstart file without polluting the workspace, read raw GitHub URLs via WebFetch (`https://raw.githubusercontent.com/Banuba/quickstart-web/master/<file>`) or clone into a temp directory outside the user's project. Never clone `Banuba/quickstart-web` into the user's project root; it shadows their files or creates a nested repo.

### 2. Fetch and search docs

Read the bundled `docs/` directory and find sections matching the request. If a topic is missing locally, fetch [`https://docs.banuba.com/far-sdk/llms-full.txt`](https://docs.banuba.com/far-sdk/llms-full.txt). Common entry points:

| Topic | Doc |
|---|---|
| Installation, bundler setup (Vite, Rollup, Webpack, plain HTML, CDN) | [installation/web](https://docs.banuba.com/far-sdk/tutorials/development/installation/web) |
| Basic integration (init, camera, first effect) | [basic_integration/web](https://docs.banuba.com/far-sdk/tutorials/development/basic_integration/web) |
| Web API overview (Player, Effect, Module, Webcam, Image, Dom, ImageCapture, VideoRecorder) | [api_overview/web](https://docs.banuba.com/far-sdk/tutorials/development/api_overview/web) |
| Resource modules table | [installation/web#resources-modules](https://docs.banuba.com/far-sdk/tutorials/development/installation/web#resources-modules) |
| AR Cloud integration | [guides/ar_cloud](https://docs.banuba.com/far-sdk/tutorials/development/guides/ar_cloud) |
| Performance, SIMD, frame size, DPR | [guides/optimization](https://docs.banuba.com/far-sdk/tutorials/development/guides/optimization) |
| Face landmarks | [guides/landmarks](https://docs.banuba.com/far-sdk/tutorials/development/guides/landmarks) |
| Effects and prefabs (background, face, makeup, sprites, top_level) | [effects/overview](https://docs.banuba.com/far-sdk/effects/overview) |
| Virtual background (blur, replacement, transparency) | [effects/virtual_background](https://docs.banuba.com/far-sdk/effects/virtual_background) |
| Known issues (Safari, memory leaks) | [known_issues/web](https://docs.banuba.com/far-sdk/tutorials/development/known_issues/web) |
| Migration and changelog | [guides/migration](https://docs.banuba.com/far-sdk/tutorials/development/guides/migration), [changelog](https://docs.banuba.com/far-sdk/tutorials/changelog) |

Base all generated code on these sources, not on pre-trained knowledge.

### 3. Configure the client token

Replace the placeholder in `BanubaClientToken.js` (or the equivalent) with the real client token. Never check tokens into source control.

### 4. Generate code

- Read `BanubaPlayer.js` in quickstart-web for the Player lifecycle and adapt it. Do not reinvent the call order.
- Use the bundler section in [installation/web](https://docs.banuba.com/far-sdk/tutorials/development/installation/web): Vite uses `?url` imports, Rollup needs `@rollup/plugin-url`, Webpack needs `file-loader` for `.wasm`. For projects with no bundler, use the `<script type="module">` plus dynamic CDN import pattern from `BanubaPlayer.js`.
- Load only the modules the feature needs. `face_tracker` is required for any face feature; add `background`, `lips`, `hair`, `skin`, `eyes`, `hands`, `body` as needed. See the resource modules table in the docs.
- `reloadConfig` requires a base effect; it does not render from cold. Apply one via `player.applyEffect(new Effect(url))` first. An empty declarative effect from Banuba Studio (`{ scene, version: "2.0.0", camera: {}, files: [] }`) suffices; the loaded resource modules render the prefab features.
- For runtime feature params (density sliders, background blur, face morphing, beautification), build a prefab config object, keep it in app state, mutate the field on change, then call `player._effectManager.reloadConfig(JSON.stringify(config))` with the full config. Parameter names live in [effects/prefabs/face](https://docs.banuba.com/far-sdk/effects/prefabs/face) (`teeth_whitening.strength`, `eyes_whitening.strength`, `morphing.{eyebrows,eyes,nose,lips,face}`), [effects/prefabs/makeup](https://docs.banuba.com/far-sdk/effects/prefabs/makeup) (`makeup_base.smooth`), and [effects/prefabs/top_level](https://docs.banuba.com/far-sdk/effects/prefabs/top_level) (`background.blur`, `background.texture`). Do not call `effect.evalJs("Skin.softening(n)")`, `Teeth.whitening`, or `Background.blur`; those are the deprecated Face Beauty API. `effect.evalJs(...)` is only for invoking custom JS defined in an effect's `config.js`.
- For selectable effect lists, use the swap pattern from `BanubaPlayer.js`: `currentEffect = new Effect(name); await player.applyEffect(currentEffect)`.
- **Sound check — do this every time an effect or mask is added**: inspect the effect's `config.json` for a top-level `sounds` array. If it exists and is non-empty, call `player.setVolume(1)` **before** `player.applyEffect(...)`. Without this call the audio in the prefab is silenced on Web regardless of the effect config. If the user has not provided the effect's `config.json`, ask them to share it, or remind them to add `player.setVolume(1)` and note why it is needed.
- Wrap `Player.create` in try/catch with a non-AR fallback for unsupported browsers (no WebGL 2.0, blocked WASM, missing token).
- Provide an HTML container for the SDK to render into (for example `<div id="webar"></div>`) and pass its selector to `Dom.render(player, "#webar")`.
- Call `player.destroy()` on session end, or cache the `Player` instance for repeated start/stop flows.

## Configuring a custom effect

The agent cannot author an effect bundle (scene engine, shaders, and module glue are compiled by Banuba Studio). It can set prefab parameters at runtime over a Studio-exported base effect:

1. Direct the user to [Banuba Studio](https://studio.banuba.com/) for a base effect. An empty declarative effect (`{ scene, version: "2.0.0", camera: {}, files: [] }`) suffices.
2. Apply it once: `await player.applyEffect(new Effect("./base-effect.zip"))`.
3. Load the modules the prefabs require (`hair`, `background`, etc.).
4. On each change, rebuild the full config and call `player._effectManager.reloadConfig(JSON.stringify(config))`.

The SDK compiles the declarative schema at runtime against the loaded modules. Example (hair color; intensity is the alpha component):

```js
const config = {
  scene: "hair", version: "2.0.0", camera: {},
  faces: [{ hair: { color: [`${rgb} ${intensity.toFixed(2)}`] } }],
};
player._effectManager.reloadConfig(JSON.stringify(config));
```

## Prefab config examples

Verified against the prefab docs. Apply over a base effect with `player._effectManager.reloadConfig(JSON.stringify(config))`.

Background blur:

```json
{ "scene": "blur_bg", "version": "2.0.0", "camera": {}, "background": { "blur": 0.5 } }
```

Background replacement:

```json
{ "scene": "virtual_bg", "version": "2.0.0", "camera": {}, "background": { "texture": "office.jpg", "content_mode": "scale_to_fill" } }
```

Beautification (skin smoothing, teeth and sclera whitening):

```json
{
  "scene": "beauty",
  "version": "2.0.0",
  "camera": {},
  "faces": [
    {
      "makeup_base": { "mode": "quality", "smooth": "0.8 0.6" },
      "teeth_whitening": { "strength": 0.7 },
      "eyes_whitening": { "strength": 0.5 }
    }
  ]
}
```

Slider 0-100% to API 0-1 (skin smoothing example):

```js
slider.oninput = (e) => {
  const v = Number(e.target.value) / 100
  config.faces[0].makeup_base.smooth = `${v} ${v}`
  player._effectManager.reloadConfig(JSON.stringify(config))
}
```

`makeup_base.smooth` takes two values: whole-face and under-eyes.

Hair coloring (module `hair`; alpha is intensity, one color or up to five for a gradient):

```json
{ "scene": "hair", "version": "2.0.0", "camera": {}, "faces": [{ "hair": { "color": ["0.19 0.06 0.25 1.0"] } }] }
```

AR makeup (module `lips`; full parameter list in the makeup docs):

```json
{ "scene": "makeup", "version": "2.0.0", "camera": {}, "faces": [{ "makeup_foundation": { "color": "0.95 0.70 0.54", "finish": "natural" } }] }
```

Foreground overlay (requires an image or video file):

```json
{ "scene": "fg", "version": "2.0.0", "camera": {}, "foreground": { "filename": "overlay.png", "@blend": "alpha", "content_mode": "scale_to_fill" } }
```

Morphing (all sub-params optional; ranges in [effects/prefabs/face](https://docs.banuba.com/far-sdk/effects/prefabs/face)):

```json
{ "scene": "morph", "version": "2.0.0", "camera": {}, "faces": [{ "morphing": { "eyes": { "enlargement": 0.4 }, "nose": { "width": -0.3 } } }] }
```

GLTF-dependent prefabs (`action_units`, `lights`, `gltf` models) require a GLTF 3D model; the config references the asset but cannot generate the mesh. Banuba Studio does not create 3D avatars/models — direct avatar requests to the [contact form](https://www.banuba.com/contact). See [effects/prefabs/face](https://docs.banuba.com/far-sdk/effects/prefabs/face) and [effects/prefabs/top_level](https://docs.banuba.com/far-sdk/effects/prefabs/top_level).

## Output format

- Complete files ready to drop in (for example `index.html`, `main.js`, `vite.config.js`, `BanubaClientToken.js`).
- Numbered integration steps.
- Lead with working code, then explain.
- Always remind the user to replace the placeholder token and to call `player.destroy()` on shutdown.
- When the project includes demo effects (see "Default effects policy"): the generated effects array must contain every entry from the demo_face_filters table — each with `name`, `description`, `zip` URL, `icon` URL, and `modules`. Never truncate the list.

## Upgrading between SDK versions

Consult the [migration guide](https://docs.banuba.com/far-sdk/tutorials/development/guides/migration) and [changelog](https://docs.banuba.com/far-sdk/tutorials/changelog) for breaking API changes. Prefer the bundled `docs/tutorials/development/guides/migration.md` and `docs/tutorials/changelog.md`.

## Common pitfalls

- Missing token: SDK fails to initialize. Always warn the user to replace the placeholder with a real client token.
- Missing `face_tracker` module: every face-related effect renders blank without it.
- Skipping `player.destroy()`: apps with "start, stop, start" flows accumulate dangling `Player` instances and crash after a few cycles. Destroy on stop, or cache the player and reuse it.
- Deprecated Face Beauty API for prefab params: `effect.evalJs("Skin.softening(n)")`, `Teeth.whitening(n)`, `Background.blur(n)` and similar calls are deprecated and flagged with a `danger` block in the docs. Change prefab parameters with `player._effectManager.reloadConfig(JSON.stringify(config))` instead. `effect.evalJs(...)` is fine for invoking custom JS inside an effect's `config.js`.
- Wrong WebAssembly path: by default the SDK expects `BanubaSDK.data`/`.wasm`/`.simd.wasm` at the application root. With Vite, Rollup, or Webpack, pass `locateFile` to `Player.create()` (object form for per-file mapping, string form for a base path), or it 404s on startup.
- Safari `MediaStream` freeze: from Safari 15.3, `canvas.captureStream()` pauses when the tab is hidden. No SDK-side workaround; document the limitation.
- Safari animation delay / `proxyVideoRequestsTo` trap: Safari has [a video range-request bug](https://bugs.webkit.org/show_bug.cgi?id=232076) that delays effect animations for video loaded over HTTP. The quickstart-web fix registers `range-requests.sw.js` and passes `proxyVideoRequestsTo: "___range-requests___/"` to `Player.create()`. **Do NOT enable this by default.** Effects that contain video textures packed inside their `.zip` (e.g. `smoke.mp4` in `BulldogHarlamov`) are unpacked into `blob:` URLs at runtime. `proxyVideoRequestsTo` prefixes every video URL the SDK requests — including those blob URLs — producing a broken URL like `http://localhost/___range-requests___/blob%3A...` that the service worker cannot resolve. Result: video textures silently fail to play on Safari. Enable `proxyVideoRequestsTo` only when the project loads video textures via external HTTP URLs. For projects using the standard zip-packed demo effects, omit `proxyVideoRequestsTo` entirely.
- SIMD vs non-SIMD bundle: expose both `BanubaSDK.wasm` (fallback) and `BanubaSDK.simd.wasm` (fast path), or you get silent runtime fallbacks or 404s.
- Bundling `.zip` modules as code: import resource module `.zip` archives as URLs (Vite `?url`, Rollup `@rollup/plugin-url`, Webpack `file-loader`), not as JS. Otherwise the bundler tries to parse a binary archive as JS.
- `new Module(url)` vs `Module.preload(url)`: both work. quickstart-web uses `Module.preload(url)` plus `player.addModule(module)` inside `Promise.all` for per-module error handling. The older `new Module(url)` plus `addModule` form loads lazily inside `addModule`.
- `Image` constructor shadowing: importing `Image` from `@banuba/webar` shadows the global `Image`. Rename on import (`import { Image as BanubaImage }`) if the app also creates HTML `<img>` programmatically.
- Effect sound silenced on Web: if an effect's `config.json` has a `sounds` array but no audio plays, the cause is almost always a missing `player.setVolume(1)` call. Add it before `player.applyEffect(...)`. This is a Web-only requirement documented in the Sounds Prefab page ([effects/prefabs/sounds](https://docs.banuba.com/far-sdk/effects/prefabs/sounds)).
- Multi-face on Web: before integrating, confirm with the user that their client token includes Max Faces support — without it the SDK tracks only one face even with a multi-face effect, and there is no JS workaround. There is also no JS API to set max faces from the app. **Important**: Banuba Studio produces single-face effects only — it cannot author multi-face effects. For multi-face effects, direct the user to the [contact form](https://www.banuba.com/contact).
- Custom effect requests: the agent cannot author an effect bundle (compiled engine + shaders come from Banuba Studio). But it can configure prefab parameters at runtime over a Studio-exported base effect via `reloadConfig` (see "When the user wants a custom-configured effect"). Hand off to Studio only for the base effect and any art assets.
- Background examples must not include `lut`: when generating code or config examples in response to background-related questions (blur, virtual background, bokeh, texture replacement), do not add a `lut` block to the config. `lut` is a global color-grade applied to the entire rendered frame — it is unrelated to background effects. Mixing it into a "background showcase" config misrepresents what is a background feature and what is not.

## Demo and reference

- [quickstart-web (GitHub)](https://github.com/Banuba/quickstart-web): production sample
- [Web AR live demo](https://docs.banuba.com/far-sdk): try the SDK in a browser
