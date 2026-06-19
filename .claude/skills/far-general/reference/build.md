# Developer mode: build reference

Read this when the Developer-mode request is to add, implement, set up, integrate, or build something with Banuba Face AR SDK. Platform detection and shared principles are in the router (`SKILL.md`); this file covers the full build workflow for Web, Android, iOS, and Desktop platforms.

## Your role

Banuba Face AR SDK implementation expert. Build working applications on Web, Android, iOS, and Desktop (C++). Lead with working code, then explain.

## Platform detection

Detect from workspace files, or ask one question if unclear.

| Signal | Platform |
|---|---|
| `package.json` (no `react-native`), `vite.config.*`, `webpack.config.*`, `index.html` + JS bundler | Web |
| `build.gradle`, `build.gradle.kts`, `AndroidManifest.xml`, `*.kt`, `*.java` | Android |
| `*.xcodeproj`, `*.xcworkspace`, `Podfile`, `*.swift`, `*.m` | iOS |
| `CMakeLists.txt`, `*.cpp`, `*.hpp`, or user says "desktop" / "C++" | Desktop |

---

## Web

### Prerequisites

1. Banuba client token (mandatory).
2. [Node.js](https://nodejs.org/) installed.
3. Browser with [WebGL 2.0](https://caniuse.com/#feat=webgl2) support.
4. A bundler (Vite, Rollup, Webpack) or plain `<script type="module">` via jsDelivr CDN for prototyping.

### Default effects policy

**When the user creates a new project OR asks to add effects without listing specific ones**, include the full set of publicly available demo effects. The canonical source is:

> https://docs.banuba.com/far-sdk/tutorials/capabilities/demo_face_filters

That page is also bundled locally at `docs/tutorials/capabilities/demo_face_filters.md`. It contains the complete list (40+ effects). Use the bundled file first; if it seems outdated, fetch the live URL.

Each effect entry provides:
- **Download URL**: `https://docs.banuba.com/far-sdk/generated/effects/<filename>.zip`
- **Icon URL**: `https://docs.banuba.com/far-sdk/generated/effects/icons/<icon_filename>.png`
- **Description** (Technologies represented column)
- **Required modules** (Required packages column)

When generating the effect list in code, emit every row from the table - not a representative sample. Build the UI so each effect shows its **icon**, **name**, and **description**. Load only the union of required modules for the active effect on swap; do not preload all modules at once.

If the user specifies a subset of effects explicitly, use only that subset. Otherwise, default to the full list.

### Workflow

#### 1. Pick the integration path

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
- `range-requests.sw.js`: only include this if the project loads video textures via external HTTP URLs. Do **not** copy it for projects using zip-packed effects (all demo effects) - combining it with `proxyVideoRequestsTo` breaks blob-URL video textures on Safari.
- Token-in-separate-file pattern (`BanubaClientToken.js` exporting the token).

To inspect a quickstart file without polluting the workspace, read raw GitHub URLs via WebFetch (`https://raw.githubusercontent.com/Banuba/quickstart-web/master/<file>`) or clone into a temp directory outside the user's project. Never clone `Banuba/quickstart-web` into the user's project root.

#### 2. Fetch and search docs

| Topic | Doc |
|---|---|
| Installation, bundler setup | [installation/web](https://docs.banuba.com/far-sdk/tutorials/development/installation/web) |
| Basic integration | [basic_integration/web](https://docs.banuba.com/far-sdk/tutorials/development/basic_integration/web) |
| Web API overview | [api_overview/web](https://docs.banuba.com/far-sdk/tutorials/development/api_overview/web) |
| AR Cloud | [guides/ar_cloud](https://docs.banuba.com/far-sdk/tutorials/development/guides/ar_cloud) |
| Performance, SIMD | [guides/optimization](https://docs.banuba.com/far-sdk/tutorials/development/guides/optimization) |
| Effects and prefabs | [effects/overview](https://docs.banuba.com/far-sdk/effects/overview) |
| Virtual background | [effects/virtual_background](https://docs.banuba.com/far-sdk/effects/virtual_background) |
| Known issues | [known_issues/web](https://docs.banuba.com/far-sdk/tutorials/development/known_issues/web) |

#### 3. Configure the client token

Replace the placeholder in `BanubaClientToken.js`. Never check tokens into source control.

#### 4. Generate code

- Read `BanubaPlayer.js` in quickstart-web for the Player lifecycle and adapt it.
- Use the bundler section in [installation/web](https://docs.banuba.com/far-sdk/tutorials/development/installation/web): Vite uses `?url` imports, Rollup needs `@rollup/plugin-url`, Webpack needs `file-loader` for `.wasm`.
- Load only the modules the feature needs. `face_tracker` is required for any face feature; add `background`, `lips`, `hair`, `skin`, `eyes`, `hands`, `body` as needed.
- `reloadConfig` requires a base effect applied first via `player.applyEffect(new Effect(url))`.
- For runtime feature params (density sliders, background blur, face morphing, beautification), build a prefab config object, mutate the field on change, then call `player._effectManager.reloadConfig(JSON.stringify(config))`. Do not call deprecated `effect.evalJs("Skin.softening(n)")`.
- **Sound check**: if an effect's `config.json` has a `sounds` array, call `player.setVolume(1)` **before** `player.applyEffect(...)`.
- Wrap `Player.create` in try/catch with a non-AR fallback for unsupported browsers.
- Call `player.destroy()` on session end.

### Prefab config examples

Background blur:
```json
{ "scene": "blur_bg", "version": "2.0.0", "camera": {}, "background": { "blur": 0.5 } }
```

Background replacement:
```json
{ "scene": "virtual_bg", "version": "2.0.0", "camera": {}, "background": { "texture": "office.jpg", "content_mode": "scale_to_fill" } }
```

Beautification:
```json
{
  "scene": "beauty", "version": "2.0.0", "camera": {},
  "faces": [{ "makeup_base": { "mode": "quality", "smooth": "0.8 0.6" }, "teeth_whitening": { "strength": 0.7 }, "eyes_whitening": { "strength": 0.5 } }]
}
```

Hair coloring:
```json
{ "scene": "hair", "version": "2.0.0", "camera": {}, "faces": [{ "hair": { "color": ["0.19 0.06 0.25 1.0"] } }] }
```

Morphing:
```json
{ "scene": "morph", "version": "2.0.0", "camera": {}, "faces": [{ "morphing": { "eyes": { "enlargement": 0.4 }, "nose": { "width": -0.3 } } }] }
```

### Web pitfalls

- Missing `face_tracker` module: every face-related effect renders blank without it.
- Skipping `player.destroy()`: accumulates dangling Player instances and crashes after a few cycles.
- Wrong WASM path: pass `locateFile` to `Player.create()` when using Vite/Rollup/Webpack.
- Safari animation delay / `proxyVideoRequestsTo` trap: do **not** enable `proxyVideoRequestsTo` for projects using standard zip-packed effects - it breaks blob-URL video textures on Safari.
- `Image` constructor shadowing: rename on import (`import { Image as BanubaImage }`) if the app also creates HTML `<img>` elements.
- Multi-face: confirm token includes Max Faces support - no JS workaround exists without it.
- Background config must not include `lut` - it is a global color-grade unrelated to background features.

---

## Android

### Prerequisites

1. Banuba client token (mandatory).
2. Android Studio installed.
3. Android API level 26+ (Android 8.0 minimum). See `docs/tutorials/capabilities/system_requirements.md`.
4. `local.properties` must exist in the project root with the Android SDK path. Check with Bash - create if missing:

```bash
test -f local.properties || echo "sdk.dir=$ANDROID_HOME" > local.properties
```

If `$ANDROID_HOME` is not set, ask the user for the path (e.g. `/Users/<name>/Library/Android/sdk`).

### Agent action checklist

Execute these steps in order. Each step specifies which tool to use.

> **Never modify or generate `res/mipmap/ic_launcher*` files.** Touching launcher icons causes resource merge crashes. Leave them as-is.

#### 1. Pick the integration path

**Path A - new project** (Bash):

```bash
git clone https://github.com/Banuba/banuba-sdk-android-samples
```

Open the `camera` sample in Android Studio. Replace the client token, then click Run.

**Path B - existing project**: do not clone over it. Continue with steps 2–6 below.

#### 2. Add the Banuba Maven repository (Edit tool)

In `settings.gradle.kts`, add the Banuba Maven repository inside `dependencyResolutionManagement`:

```kotlin
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        maven {
            name = "BanubaMaven"
            url = uri("https://nexus.banuba.net/repository/maven-releases")
        }
    }
}
```

#### 3. Add SDK dependencies (Edit tool)

In `app/build.gradle.kts`, add dependencies. Use `bnbVersion = "1.18.2"`. Only include modules needed for active effects:

```kotlin
val bnbVersion = "1.18.2"
val bnbComSdk = "com.banuba.sdk"

dependencies {
    api("$bnbComSdk:sdk_api:$bnbVersion")
    api("$bnbComSdk:face_tracker:$bnbVersion")   // required for face effects
    api("$bnbComSdk:background:$bnbVersion")      // required for background effects
    api("$bnbComSdk:makeup:$bnbVersion")          // required for makeup/beauty
    api("$bnbComSdk:eyes:$bnbVersion")
}
```

Then trigger Gradle sync (Bash):

```bash
./gradlew dependencies --configuration releaseRuntimeClasspath
```

#### 4. Download effects (Bash)

**Always do this step - do not skip it even if the project already has some effects in `assets/`.**

Effect bundles are platform-agnostic: the same zip files work on Web, Android, iOS, and Desktop. The canonical source is `docs/tutorials/capabilities/demo_face_filters.md` (40+ effects). Each entry provides:
- **Download URL**: `https://docs.banuba.com/far-sdk/generated/effects/<filename>.zip`
- **Required modules** (Required packages column) - add to dependencies in step 3

**When the user does not specify effects explicitly**, download the full list. Read `docs/tutorials/capabilities/demo_face_filters.md`, then for each effect (Bash):

```bash
mkdir -p app/src/main/assets/effects
cd app/src/main/assets/effects

# repeat for each effect from demo_face_filters.md:
curl -L "https://docs.banuba.com/far-sdk/generated/effects/<filename>.zip" -o <filename>.zip
unzip -q <filename>.zip
rm <filename>.zip

cd -
```

> After unzipping, verify immediately (Bash):
> ```bash
> ls app/src/main/assets/effects/Afro/
> # Must show: config.json  images/  shaders/  ...
> # Must NOT show a subfolder named "Afro"
> ```
> If you see `effects/Afro/Afro/`, fix with: `mv effects/Afro/Afro/* effects/Afro/ && rmdir effects/Afro/Afro`

**Required folder structure** - one flat level only, no nesting:
```
assets/effects/
  Afro/          ← effect folder directly here
    config.json
    ...
  TrollGrandma/
    config.json
    ...
```

Never create `assets/effects/effects/` or any intermediate folder - the SDK resolves effect names directly against the `effects/` directory. Verify after unzip (Bash):

```bash
ls app/src/main/assets/effects/
```

Each entry must be a folder (one level), not a zip or a nested subdirectory.

If the user specifies a subset, download only that subset. Effects must live in `assets/effects/` - wrong path causes silent no-load.

#### 5. Configure the client token (Write tool)

Write the token to `BanubaClientToken.kt`:

```
<#Place your token here#>
```

Then initialize in `Application.onCreate()` (Edit tool):

```kotlin
BanubaSdkManager.initialize(this, banubaClientToken)
```

#### 6. Core integration pattern (Write tool)

Write `MainActivity.kt`:

```kotlin
class MainActivity : BaseActivity(R.layout.main) {

    private val surfaceView by lazy(LazyThreadSafetyMode.NONE) {
        findViewById<SurfaceView>(R.id.surfaceView)
    }

    private val player by lazy(LazyThreadSafetyMode.NONE) { Player() }
    private val cameraDevice by lazy(LazyThreadSafetyMode.NONE) {
        CameraDevice(requireNotNull(this.applicationContext), this@MainActivity)
    }
    private val surfaceOutput by lazy(LazyThreadSafetyMode.NONE) {
        SurfaceOutput(surfaceView.holder)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        player.use(CameraInput(cameraDevice), surfaceOutput)
        surfaceView.setOnTouchListener(PlayerTouchListener(this, player))
    }

    override fun onStart() { super.onStart(); cameraDevice.start() }
    override fun onResume() { super.onResume(); player.play() }
    override fun onPause() { super.onPause(); player.pause() }
    override fun onStop() { cameraDevice.stop(); super.onStop() }

    override fun onDestroy() {
        cameraDevice.close(); surfaceOutput.close(); player.close()
        super.onDestroy()
    }
}
```

Loading / clearing effects:

```kotlin
player.loadAsync("effects/TrollGrandma")  // load
player.loadAsync("")                       // clear
```

### Android pitfalls

- Always call `close()` on `cameraDevice`, `surfaceOutput`, and `player` in `onDestroy()`.
- Wrong effect path: effects must be in `assets/` - wrong path causes silent no-load.
- Unused modules increase APK size - only add what your effects need.
- Multi-face: confirm Max Faces is in the token first.

---

## iOS

### Prerequisites

1. Banuba client token (mandatory).
2. Mac with Xcode installed.
3. iOS 15.0+ deployment target.
4. CocoaPods - install if missing:
```bash
sudo gem install cocoapods
```

### Agent action checklist

Execute these steps in order. Each step specifies which tool to use.

#### 1. Pick the integration path

**Path A - new project** (Bash):

```bash
git clone https://github.com/Banuba/banuba-sdk-ios-samples
```

Open the `camera` sample workspace in Xcode. Insert the token in `common/common/AppDelegate.swift`, then Run. Study this sample when something seems off - it is the canonical reference.

**Path B - existing project or from scratch**: do not clone over it. Continue with steps 2–6 below.

#### 2. Create the Podfile (Write tool → then Bash)

Detect the Xcode target name from `*.xcodeproj` or ask the user. Then **write** `Podfile` to the project root:

```ruby
source 'https://github.com/sdk-banuba/banuba-sdk-podspecs.git'
source 'https://cdn.cocoapods.org/'

platform :ios, '15.0'
use_frameworks!

target 'YourTargetName' do
  pod 'BNBEffectPlayer', '1.18.2'
  pod 'BNBSdkApi',       '1.18.2'
  pod 'BNBSdkCore',      '1.18.2'
  pod 'BNBFaceTracker',  '1.18.2'
  pod 'BNBBackground',   '1.18.2'   # required for background effects
  pod 'BNBMakeup',       '1.18.2'   # required for makeup/beauty effects
end
```

> **Critical**: correct pod names are `BNBEffectPlayer`, `BNBSdkApi`, `BNBSdkCore`, `BNBFaceTracker`, `BNBBackground`, `BNBMakeup`. Names like `BanubaEffectPlayer` or `BanubaSdk` do not exist - `pod install` will fail.

Then run (Bash):

```bash
pod install
```

Always open `.xcworkspace` (not `.xcodeproj`) afterward.

> **SPM alternative**: `https://github.com/Banuba/BNBSdkApi-iOS-spm` and `https://github.com/Banuba/BNBSdkCore-iOS-spm`. Do **not** mix SPM and CocoaPods for Banuba packages in the same target.

#### 3. Create the client token file (Write tool)

Write `BanubaClientToken.swift` to the Swift sources folder:

```swift
let banubaClientToken = "<#Place your token here#>"
```

#### 4. Download effects (Bash)

Clone the samples repo into a temp directory and copy the effects folder to the project root as a sibling of the Swift sources folder - **not inside it**:

```bash
git clone --depth 1 https://github.com/Banuba/banuba-sdk-ios-samples /tmp/banuba-ios-samples
cp -r /tmp/banuba-ios-samples/camera/effects ./effects
rm -rf /tmp/banuba-ios-samples
```

The resulting layout must be:

```
MyApp.xcodeproj
MyApp/               ← Swift sources: *.swift, *.storyboard, Info.plist
effects/             ← sibling of MyApp/, NOT inside it
  Afro/
  blur_bg/
```

Placing effects inside the sources folder causes "Multiple commands produce … config.json" build errors.

#### 5. Register effects folder in the Xcode project (Bash)

The `effects/` folder must be added as a **folder reference** (blue folder, `lastKnownFileType = folder`) - not a group. Do this automatically via the `xcodeproj` Ruby gem:

```bash
gem install xcodeproj 2>/dev/null || true
```

Then write and run a Ruby script (Write tool → Bash):

```ruby
# add_effects_ref.rb
require 'xcodeproj'

proj_path = Dir.glob('*.xcodeproj').first
abort 'No .xcodeproj found' unless proj_path

project = Xcodeproj::Project.open(proj_path)
target  = project.targets.first

# Remove any existing stale effects reference
project.main_group.files.select { |f| f.path == 'effects' }.each(&:remove_from_project)

# Add effects as a folder reference
ref = project.main_group.new_file('effects', :folder)
ref.last_known_file_type = 'folder'

# Add to Copy Bundle Resources of every app target
project.targets.select { |t| t.is_a?(Xcodeproj::Project::Object::PBXNativeTarget) }.each do |t|
  phase = t.resources_build_phase
  phase.add_file_reference(ref) unless phase.files.map(&:file_ref).include?(ref)
end

project.save
puts "Done - effects folder reference added to #{proj_path}"
```

```bash
ruby add_effects_ref.rb
```

After running, verify (Bash):

```bash
grep -r "lastKnownFileType = folder" *.xcodeproj/project.pbxproj | grep effects
```

If the grep returns a line, the reference is correct.

> **xcodegen projects**: skip the Ruby script. Instead use `type: folder` + `buildPhase: resources` under `sources:`:
> ```yaml
> targets:
>   MyApp:
>     sources:
>       - path: MyApp
>         excludes: [effects/**]
>       - path: effects
>         type: folder
>         buildPhase: resources
> ```

#### 6. Initialize the SDK in AppDelegate (Write tool)

Write or update `AppDelegate.swift`:

```swift
import UIKit
import BNBSdkApi

@main
class AppDelegate: UIResponder, UIApplicationDelegate {
    var window: UIWindow?

    func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
    ) -> Bool {
        BanubaSdkManager.initialize(
            resourcePath: [
                Bundle.main.bundlePath + "/effects",
                Bundle.main.bundlePath
            ],
            clientTokenString: banubaClientToken
        )
        return true
    }

    func applicationWillTerminate(_ application: UIApplication) {
        BanubaSdkManager.deinitialize()
    }
}
```

#### 7. Wire up EffectPlayerView and implement the view controller (Write tool)

**Critical**: `EffectPlayerView` **must** be the root view in the storyboard, connected as an `@IBOutlet`. Do **not** instantiate it with `EffectPlayerView()` in code - that produces `frame = .zero` and effects are never rendered (camera shows, AR is invisible).

Write `Main.storyboard` (or patch the existing one) so the view controller's root view has `customClass="EffectPlayerView"` and `customModule="BNBSdkApi"`:

```xml
<viewController customClass="ViewController" customModule="MyApp" customModuleProvider="target">
    <view key="view"
          customClass="EffectPlayerView"
          customModule="BNBSdkApi">
        <autoresizingMask key="autoresizingMask" widthSizable="YES" heightSizable="YES"/>
    </view>
    <connections>
        <outlet property="effectView" destination="<view-id>" id="<outlet-id>"/>
    </connections>
</viewController>
```

Add `NSCameraUsageDescription` to `Info.plist` (Write or Edit tool).

Write `ViewController.swift` - includes camera setup **and** an interactive effect picker (horizontal scroll at the bottom). Populate `effects` from the actual folder names copied in step 4; always include a `("", "None")` entry first so the user can clear the effect.

```swift
import UIKit
import BNBSdkApi
import BNBSdkCore

class ViewController: UIViewController {

    @IBOutlet weak var effectView: EffectPlayerView!

    // Populate from the effects/ folder copied in step 4.
    // First entry ("", "None") clears the active effect.
    private let effects: [(name: String, label: String)] = [
        ("", "None"),
        ("CartoonOctopus", "Octopus"),
        ("Clubs", "Clubs"),
        ("TrollGrandma", "Troll"),
        ("RainbowBeauty", "Rainbow"),
        ("RegularBlur", "Blur"),
        ("Sunset", "Sunset"),
        // add remaining effect folder names here
    ]

    private var selectedIndex = 0

    private let cameraDevice = CameraDevice(
        cameraMode: .FrontCameraSession,
        captureSessionPreset: .hd1280x720
    )
    private var player: Player?

    private lazy var collectionView: UICollectionView = {
        let layout = UICollectionViewFlowLayout()
        layout.scrollDirection = .horizontal
        layout.itemSize = CGSize(width: 72, height: 72)
        layout.minimumLineSpacing = 8
        layout.sectionInset = UIEdgeInsets(top: 0, left: 16, bottom: 0, right: 16)
        let cv = UICollectionView(frame: .zero, collectionViewLayout: layout)
        cv.backgroundColor = UIColor.black.withAlphaComponent(0.45)
        cv.layer.cornerRadius = 16
        cv.showsHorizontalScrollIndicator = false
        cv.register(EffectCell.self, forCellWithReuseIdentifier: EffectCell.id)
        cv.dataSource = self
        cv.delegate = self
        cv.translatesAutoresizingMaskIntoConstraints = false
        return cv
    }()

    override func viewDidLoad() {
        super.viewDidLoad()
        setupPlayer()
        setupCollectionView()
    }

    private func setupPlayer() {
        player = Player()
        player?.use(input: Camera(cameraDevice: cameraDevice))
        player?.use(outputs: [effectView])
        player?.play()
        cameraDevice.start()
    }

    private func setupCollectionView() {
        view.addSubview(collectionView)
        NSLayoutConstraint.activate([
            collectionView.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 12),
            collectionView.trailingAnchor.constraint(equalTo: view.trailingAnchor, constant: -12),
            collectionView.bottomAnchor.constraint(equalTo: view.safeAreaLayoutGuide.bottomAnchor, constant: -12),
            collectionView.heightAnchor.constraint(equalToConstant: 88),
        ])
    }

    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)
        player?.pause()
        cameraDevice.stop()
    }
}

extension ViewController: UICollectionViewDataSource, UICollectionViewDelegate {

    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        effects.count
    }

    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: EffectCell.id, for: indexPath) as! EffectCell
        cell.configure(label: effects[indexPath.item].label, selected: indexPath.item == selectedIndex)
        return cell
    }

    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        guard indexPath.item != selectedIndex else { return }
        selectedIndex = indexPath.item
        collectionView.reloadData()
        player?.load(effect: effects[indexPath.item].name)
    }
}

// MARK: - Cell

private final class EffectCell: UICollectionViewCell {
    static let id = "EffectCell"

    private let label: UILabel = {
        let l = UILabel()
        l.textAlignment = .center
        l.font = .systemFont(ofSize: 11, weight: .medium)
        l.textColor = .white
        l.numberOfLines = 2
        l.translatesAutoresizingMaskIntoConstraints = false
        return l
    }()

    override init(frame: CGRect) {
        super.init(frame: frame)
        contentView.layer.cornerRadius = 10
        contentView.clipsToBounds = true
        contentView.addSubview(label)
        NSLayoutConstraint.activate([
            label.leadingAnchor.constraint(equalTo: contentView.leadingAnchor, constant: 4),
            label.trailingAnchor.constraint(equalTo: contentView.trailingAnchor, constant: -4),
            label.centerYAnchor.constraint(equalTo: contentView.centerYAnchor),
        ])
    }

    required init?(coder: NSCoder) { fatalError() }

    func configure(label text: String, selected: Bool) {
        label.text = text
        contentView.backgroundColor = selected
            ? UIColor.systemBlue
            : UIColor.white.withAlphaComponent(0.15)
    }
}
```

> **Effect picker rule**: always generate the `effects` array from the actual folder names present in `effects/` (discovered in step 4 via `ls`). Never hardcode a partial list. Include `("", "None")` as the first entry.

#### 8. Verify the build (Bash)

After building in Xcode, check the effects folder is present in the app bundle:

```bash
find ~/Library/Developer/Xcode/DerivedData -name "effects" -type d 2>/dev/null | grep -v Intermediates | head -5
```

If nothing is found, the folder reference was not registered correctly - re-run step 5.

### iOS pitfalls

| Pitfall | Symptom | Fix |
|---|---|---|
| Wrong pod names (`BanubaEffectPlayer`, `BanubaSdk`) | `pod not found` on `pod install` | Use `BNBEffectPlayer`, `BNBSdkApi`, `BNBSdkCore`, `BNBFaceTracker`, `BNBBackground`, `BNBMakeup` |
| Banuba pod source missing or after CDN source | pod not found | Add `source 'https://github.com/sdk-banuba/banuba-sdk-podspecs.git'` as the **first** source line |
| `EffectPlayerView` created programmatically | Camera shows, effects invisible | Always use storyboard IBOutlet; the root view must be `EffectPlayerView` |
| `effects/` folder inside the Swift sources folder | "Multiple commands produce … config.json" build error | Move `effects/` to a sibling folder; never nest it inside the folder Xcode scans for Swift files |
| Wrong xcodegen syntax for folder reference | Effects folder absent from bundle, all effects silently fail | Use `type: folder` + `buildPhase: resources` under `sources:`, not `resources:` with `type: folder-reference` |
| Effects folder not in bundle | SDK initializes, camera shows, `player.load(effect:)` returns `nil` silently | Verify `<App>.app/effects/` exists in DerivedData after build; if absent, fix the Copy Bundle Resources phase |
| Opening `.xcodeproj` instead of `.xcworkspace` after `pod install` | Linker errors, missing Banuba frameworks | Always open `.xcworkspace` |
| Mixing SPM and CocoaPods for Banuba pods | Duplicate symbol errors | Pick one package manager for all Banuba pods |
| Resource paths missing from `BanubaSdkManager.initialize` | Effects silently fail to load | Include both `Bundle.main.bundlePath + "/effects"` and `Bundle.main.bundlePath` |
| Multi-face without token support | Only one face tracked, no error | Confirm token includes Max Faces before implementing multi-face UI |
| Wrong `targetRuntime` in storyboard | "The document could not be opened. (com.apple.InterfaceBuilder error -1.)" | Use `targetRuntime="iOS.CocoaTouch"` - never `"AppleCocoa"` (that is macOS). Copy the exact header from Xcode's own template at `Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates/Project Templates/iOS/Application/Cocoa Touch App Base.xctemplate/LaunchScreen.storyboard` |
| `customModuleProvider="target"` on `EffectPlayerView` in storyboard | Black screen, camera not visible, AR never renders | Remove `customModuleProvider="target"` from the `<view>` element - this attribute is only for app-target classes. Pod classes only need `customClass="EffectPlayerView" customModule="BNBSdkApi"` |

---

## Desktop (C++)

### Prerequisites

1. Banuba client token (mandatory - ask the user for it before starting).
2. CMake 3.15+.
3. Windows: Visual Studio 2019+ with C++ tools. macOS: Xcode command line tools.

### Workflow

**Path A - new project (recommended)**: clone the quickstart — it is the fastest path to a running C++ app and is the recommended scaffold (a minimal working app, not a large template). Proceed with steps 1–7 below.

**Path B - new project from scratch**: if the user explicitly wants an empty project without the quickstart scaffold, you may set one up using the quickstart as a structural reference (CMakeLists.txt layout, SDK include/link pattern, main loop structure). Use WebFetch to read `https://raw.githubusercontent.com/Banuba/quickstart-desktop-cpp/master/CMakeLists.txt` and `main.cpp` before generating. Still execute steps 3–4 (SDK download and token) as written.

For all other cases ("integrate into existing C++ project", user already has a project): skip the clone step and start from step 3.

#### 1. Ask three questions upfront

Before doing anything, ask:
- Banuba client token?
- Where to clone the project? (default: current directory)
- Target OS - macOS or Windows?

The target OS is what the app will run on, not necessarily the machine you're working on right now. All steps below run only the branch for that OS.

> "Don't download the Windows SDK" means Microsoft's Visual C++ toolchain - not the Banuba SDK. Always download the Banuba Face AR SDK binaries (step 3) regardless.

#### 2. Clone the quickstart (Bash)

```bash
git clone --recursive https://github.com/Banuba/quickstart-desktop-cpp "<project_dir>"
```

If the folder already exists and isn't empty, clone into `<project_dir>/quickstart-desktop-cpp` instead.

#### 3. Download Banuba Face AR SDK binaries (Bash)

**Execute this step immediately after cloning - do not describe it, do not ask the user to do it manually.** The project will not build without these binaries.

GitHub's API is unreliable for this repo (rate limits, auth). Parse the releases page directly instead:

```bash
curl -s https://github.com/Banuba/FaceAR-SDK-desktop-releases/releases/latest \
  | grep -o 'href="[^"]*bnb_sdk[^"]*"' | sed 's/href="//;s/"//'
```

Pick the URL matching the target OS, prepend `https://github.com` if it's relative, then download:

**macOS:**
```bash
mkdir -p "<project_dir>/bnb_sdk"
curl -L "https://github.com<url>" -o /tmp/bnb_sdk_mac.tar.gz
tar -xzf /tmp/bnb_sdk_mac.tar.gz -C "<project_dir>/bnb_sdk" --strip-components=1
rm /tmp/bnb_sdk_mac.tar.gz
```

**Windows:**
```bash
mkdir -p "<project_dir>/bnb_sdk"
curl -L "https://github.com<url>" -o /tmp/bnb_sdk_win.zip
unzip -q /tmp/bnb_sdk_win.zip -d /tmp/bnb_sdk_win
cp -r /tmp/bnb_sdk_win/bin/. "<project_dir>/bnb_sdk/bin/"
cp -r /tmp/bnb_sdk_win/include/. "<project_dir>/bnb_sdk/include/"
cp -r /tmp/bnb_sdk_win/resources/. "<project_dir>/resources/"
rm -rf /tmp/bnb_sdk_win /tmp/bnb_sdk_win.zip
```

Check it worked:
```bash
ls "<project_dir>/bnb_sdk"
# macOS: should have include/ and lib/
# Windows: should have bin/ and include/
```

If the page scrape returns nothing, ask the user to open https://github.com/Banuba/FaceAR-SDK-desktop-releases/releases and paste the direct download URL - then run the curl yourself.

#### 4. Insert the client token (Edit tool)

Edit `<project_dir>/helpers/src/BanubaClientToken.hpp` - replace the placeholder with the token the user provided in step 1:

```cpp
#define BNB_CLIENT_TOKEN "<user_token>"
```

Never check this file into source control. Add it to `.gitignore` (Edit tool):
```
helpers/src/BanubaClientToken.hpp
```

#### 5. Generate the Xcode / VS project (Bash)

**macOS**:
```bash
cd "<project_dir>"
mkdir -p build && cd build
cmake -G Xcode ..
```

**Windows** (x64):
```bash
cd "<project_dir>"
mkdir build && cd build
cmake -A x64 ..
```

Verify (Bash):
```bash
# macOS
ls "<project_dir>/build" | grep xcodeproj
# Windows
ls "<project_dir>/build" | grep sln
```

#### 6. Copy SDK DLLs next to the executable - Windows only (Bash)

On Windows, the app crashes at launch if the SDK DLLs are not next to the `.exe`. Copy them now - before the user tries to run:

```bash
# Debug build
cp "<project_dir>/bnb_sdk/bin/"*.dll "<project_dir>/build/bin/Debug/"
# Release build
cp "<project_dir>/bnb_sdk/bin/"*.dll "<project_dir>/build/bin/Release/"
```

Verify that `avcodec-61.dll` (and other Banuba DLLs) are present:
```bash
ls "<project_dir>/build/bin/Debug/" | grep dll
```

If the `bin/Debug/` path doesn't exist yet (project not built), copy to both possible output dirs preemptively - VS may use `x64/Debug/` or `out/build/x64-Debug/` depending on the generator:
```bash
find "<project_dir>/build" -name "*.exe" -exec dirname {} \; 2>/dev/null | sort -u
```
Then copy to each found directory.

#### 7. Open the project for the user (Bash)

**macOS** - open the generated Xcode project:
```bash
open "<project_dir>/build/quickstart-desktop-cpp.xcodeproj"
```

**Windows** - open the solution:
```bash
start "<project_dir>/build/quickstart-desktop-cpp.sln"
```

Then tell the user:
> "Project is ready. On Windows, open the solution in Visual Studio, select the `realtime-camera-preview` project, set it as startup project, and run. SDK DLLs are already copied next to the executable. Your token is already inserted."

#### 7. Core integration pattern (reference only - quickstart already contains this)

```cpp
#include <bnb/player_api/interfaces/render_target/metal_render_target.hpp>
using namespace bnb::interfaces;

namespace {
    render_backend_type render_backend = BNB_APPLE
        ? render_backend_type::metal
        : render_backend_type::opengl;
}

int main() {
    // Initialize SDK - must be first
    bnb::utility utility({bnb::sdk_resources_path(), BNB_RESOURCES_FOLDER}, BNB_CLIENT_TOKEN);

    auto renderer = std::make_shared<GLFWRenderer>(render_backend);

    bnb::player_api::render_target_sptr render_target;
    if (render_backend == render_backend_type::opengl)
        render_target = bnb::player_api::opengl_render_target::create();
    else
        render_target = bnb::player_api::metal_render_target::create();

    auto player = bnb::player_api::player::create(30, render_target, renderer);
    auto input  = bnb::player_api::live_input::create();
    auto output = bnb::player_api::window_output::create(renderer->get_native_surface());

    player->use(input).use(output);
    player->load_async("effects/TrollGrandma");

    auto camera = bnb::create_camera_device([input](bnb::full_image_t image) {
        auto now_us = std::chrono::duration_cast<std::chrono::microseconds>(
            std::chrono::high_resolution_clock::now().time_since_epoch()).count();
        input->push(image, now_us);
    }, 0);

    renderer->get_window()->set_callbacks(
        [output](uint32_t w, uint32_t h) { output->set_frame_layout(0, 0, w, h); },
        nullptr
    );

    renderer->get_window()->show_window_and_run_events_loop();
    return 0;
}
```

Loading / clearing effects:

```cpp
player->load_async("effects/TrollGrandma");  // load
player->load_async("");                       // clear
```

### Desktop pitfalls

- `bnb::utility` must be the first SDK call - it validates the token and loads resources.
- Windows: app crashes at launch with `avcodec-61.dll not found` if DLLs from `bnb_sdk/bin/` are not copied next to the `.exe` - do this before the user runs (step 6).
- macOS: use Metal; Windows: use OpenGL.
- Always clone with `--recursive` - helpers are submodules. If cmake fails with git-related errors on Windows, run `git submodule update --init --recursive` inside the project dir.
- If SDK download fails: ask the user to open https://github.com/Banuba/FaceAR-SDK-desktop-releases/releases and paste the direct download URL.
- Multi-face: confirm Max Faces is in the token first.

---

## Flutter

### Prerequisites

1. Banuba client token (mandatory).
2. Flutter SDK installed.
3. Android: API level 26+. iOS: 14.0+.

### Workflow

#### 1. Pick the integration path

**Path A - new project** (Bash):
```bash
git clone https://github.com/Banuba/banuba-sdk-flutter
```
Open `example/` in your IDE. Insert the token in `example/lib/main.dart`, run `flutter pub get && flutter run`.

**Path B - new project from scratch** (Bash):
```bash
flutter create <appname>
cd <appname>
```
Then continue with steps 2–8 below.

**Path C - existing project**: continue with steps 2–8 below.

#### 2. Add SDK packages (Bash)

```bash
flutter pub add banuba_sdk
flutter pub add permission_handler
```

#### 3. Configure Android (Edit tool)

In `android/build.gradle`, add `bnb_sdk_version` to the `ext` block:

```groovy
ext {
    bnb_sdk_version = '1.18.+'
}
```

In `android/app/build.gradle`:

```groovy
android {
    defaultConfig {
        minSdkVersion 26
        ndk { abiFilters 'armeabi-v7a', 'arm64-v8a' }
    }
}

dependencies {
    implementation "com.banuba.sdk:face_tracker:$project.bnb_sdk_version"
    implementation "com.banuba.sdk:background:$project.bnb_sdk_version"
}

task copyEffects {
    copy {
        from flutter.source + '/effects'
        into 'src/main/assets/bnb-resources/effects'
    }
}

gradle.projectsEvaluated {
    preBuild.dependsOn(copyEffects)
}
```

> Note: `flutter.source` resolves correctly only after the `flutter { source '../..' }` block is present in `android/app/build.gradle`. The Flutter Gradle plugin adds it automatically when you run `flutter pub add banuba_sdk`.

#### 4. Configure iOS (Edit tool → Bash)

Replace the contents of `ios/Podfile` with (Edit tool):

```ruby
ENV['COCOAPODS_DISABLE_STATS'] = 'true'

source 'https://github.com/CocoaPods/Specs.git'
source 'https://github.com/sdk-banuba/banuba-sdk-podspecs.git'
$bnb_sdk_version = '~> 1.18.0'

platform :ios, '14.0'

inhibit_all_warnings!

target 'Runner' do
  use_frameworks!
  use_modular_headers!

  pod 'BNBFaceTracker', $bnb_sdk_version
  pod 'BNBBackground',  $bnb_sdk_version

  flutter_install_all_ios_pods File.dirname(File.realpath(__FILE__))
end

post_install do |installer|
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)
    target.build_configurations.each do |config|
      config.build_settings['GCC_PREPROCESSOR_DEFINITIONS'] ||= [
        '$(inherited)',
        'PERMISSION_CAMERA=1',
        'PERMISSION_MICROPHONE=1',
      ]
    end
  end
end
```

> `GCC_PREPROCESSOR_DEFINITIONS` with `PERMISSION_CAMERA=1` and `PERMISSION_MICROPHONE=1` is required for `permission_handler` to work on iOS - without it the plugin compiles but permission requests silently fail.

Then run (Bash):
```bash
cd ios && pod install && cd ..
```

#### 5. Add permissions (Edit tool)

**Android** - add to `android/app/src/main/AndroidManifest.xml` inside `<manifest>`:

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

**iOS** - add to `ios/Runner/Info.plist`:

```xml
<key>NSCameraUsageDescription</key>
<string>Camera access required to render AR effects</string>
<key>NSMicrophoneUsageDescription</key>
<string>Micro access required to record video</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>Gallery access required to pick image for processing</string>
```

#### 6. Download effects (Bash)

```bash
mkdir -p effects
# repeat for each effect from docs/tutorials/capabilities/demo_face_filters.md:
curl -L "https://docs.banuba.com/far-sdk/generated/effects/<filename>.zip" -o /tmp/<filename>.zip
unzip -q /tmp/<filename>.zip -d effects/<filename>
rm /tmp/<filename>.zip
```

Place `effects/` at the Flutter project root (sibling of `lib/`). The Gradle `copyEffects` task copies it into Android assets automatically. For iOS, add via Xcode (`File → Add Files to 'Runner'...`) as a folder reference.

#### 7. Write lib/main.dart (Write tool)

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'dart:async';
import 'dart:io';
import 'package:permission_handler/permission_handler.dart';
import 'package:banuba_sdk/banuba_sdk.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> with WidgetsBindingObserver {
  final _banubaSdkManager = BanubaSdkManager();
  final _epWidget = EffectPlayerWidget(key: null);

  @override
  void initState() {
    super.initState();
    initPlatformState();
  }

  Future<void> initPlatformState() async {
    await _banubaSdkManager.initialize(
        [], '<#Place your token here#>', SeverityLevel.info);

    if (!mounted) return;
    setState(() {});

    requestPermissions().then((granted) {
      if (granted) {
        openCamera();
      } else {
        SystemNavigator.pop();
      }
    });
  }

  Future<void> openCamera() async {
    await _banubaSdkManager.openCamera();
    await _banubaSdkManager.attachWidget(_epWidget.banubaId);
    await _banubaSdkManager.startPlayer();
    await _banubaSdkManager.loadEffect('effects/TrollGrandma', false);
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(home: _epWidget);
  }

  @override
  void didChangeAppLifecycleState(AppLifecycleState state) {
    if (state == AppLifecycleState.resumed) {
      _banubaSdkManager.startPlayer();
    } else {
      _banubaSdkManager.stopPlayer();
    }
  }
}

Future<bool> requestPermissions() async {
  final perms = [Permission.camera, Permission.microphone];
  for (final p in perms) {
    if (!await p.isGranted && !await p.request().isGranted) return false;
  }
  return true;
}
```

Replace `'<#Place your token here#>'` with the actual client token.

#### 8. Verify build and run (Bash)

First verify the project compiles without errors:

```bash
flutter pub get
flutter build apk   # Android — catches compile errors before device needed
```

Then run on device:
```bash
flutter run
```

If `flutter run` fails on iOS with pod issues, run:
```bash
cd ios && pod install && cd .. && flutter run
```

### Flutter pitfalls

- `permission_handler` must be added explicitly - `banuba_sdk` does not pull it in.
- `minSdkVersion` must be 26+ - lower values cause build failure.
- `bnb_sdk_version` must be declared in `android/build.gradle` ext block - without it `$project.bnb_sdk_version` is undefined.
- Effects folder must be at the Flutter project root - Gradle copies from `flutter.source + '/effects'`.
- On iOS, Banuba pod source must come **before** CocoaPods CDN source in `Podfile`.
- On iOS, add `effects/` as a **folder reference** in Xcode (blue folder icon) - not as a group.
- Missing `NSCameraUsageDescription` in Info.plist causes a crash on iOS 14+.
- If the SDK has more capabilities than the Flutter package exposes, go with native Android/iOS integration.

---

## React Native

### Prerequisites

1. Banuba client token (mandatory).
2. React Native 0.73+ environment (Node, Watchman, Android Studio / Xcode).
3. Android: API level 26+. iOS: 15.0+.

### Workflow

#### 1. Pick the integration path

**Path A - clone sample** (Bash):
```bash
git clone https://github.com/Banuba/banuba-sdk-react-native
```
Open `example/` in your IDE. Insert the token in `example/src/App.tsx`, run `yarn install && yarn android` or `yarn ios`.

**Path B - new project from scratch** (Bash):
```bash
npx react-native init <AppName>
cd <AppName>
```
Then continue with steps 2–9 below.

**Path C - existing project**: continue with steps 2–9 below.

#### 2. Add the SDK package (Bash)

```bash
yarn add @banuba/react-native
```

> Do not pin a version number — `@banuba/react-native` versioning is independent from the Video Editor SDK (`0.50.0` is VE, not FAR). Let yarn resolve the latest.

#### 3. Configure Android root build.gradle (Edit tool)

In `android/build.gradle`, add `bnb_sdk_version` to the `ext` block and the Banuba Maven repo to `allprojects`:

```groovy
buildscript {
    ext {
        minSdkVersion = 26
        compileSdkVersion = 35
        targetSdkVersion = 35
        bnb_sdk_version = '1.18.+'
    }
    repositories {
        google()
        mavenCentral()
    }
}

allprojects {
    repositories {
        maven { url "https://nexus.banuba.net/repository/maven-releases" }
    }
}
```

> The `allprojects` block is required — without it Gradle cannot resolve `com.banuba.sdk` dependencies in a freshly created RN project.

#### 4. Configure Android app build.gradle (Edit tool)

In `android/app/build.gradle`, add dependencies and effects copy task:

```groovy
dependencies {
    implementation "com.banuba.sdk:face_tracker:$project.bnb_sdk_version"
    implementation "com.banuba.sdk:background:$project.bnb_sdk_version"
}

task copyEffects {
    copy {
        from '../../effects'
        into 'src/main/assets/bnb-resources/effects'
    }
}

gradle.projectsEvaluated {
    preBuild.dependsOn(copyEffects)
}
```

#### 5. Configure iOS (Edit tool → Bash)

Add the Banuba source and pod declarations to `ios/Podfile`. The sources go before the `target` block; the pods go inside it alongside `use_react_native!`:

```ruby
source 'https://github.com/CocoaPods/Specs.git'
source 'https://github.com/sdk-banuba/banuba-sdk-podspecs.git'
$bnb_sdk_version = '~> 1.18.0'

target 'YourApp' do
  config = use_native_modules!

  pod 'BNBFaceTracker', $bnb_sdk_version
  pod 'BNBBackground',  $bnb_sdk_version

  use_react_native!(
    :path => config[:reactNativePath],
    :app_path => "#{Pod::Config.instance.installation_root}/.."
  )
end
```

Then run (Bash):
```bash
cd ios && pod install && cd ..
```

#### 6. Add permissions (Edit tool)

**Android** - add to `android/app/src/main/AndroidManifest.xml` inside `<manifest>`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

> `@banuba/react-native` declares CAMERA and RECORD_AUDIO in its own merged manifest - no need to add them manually.

**iOS** - add to `ios/<AppName>/Info.plist`:

```xml
<key>NSCameraUsageDescription</key>
<string>We use camera to render AR effects</string>
<key>NSMicrophoneUsageDescription</key>
<string>We use microphone to capture audio during video recording</string>
```

#### 7. Download effects (Bash)

```bash
mkdir -p effects
# repeat for each effect from docs/tutorials/capabilities/demo_face_filters.md:
curl -L "https://docs.banuba.com/far-sdk/generated/effects/<filename>.zip" -o /tmp/<filename>.zip
unzip -q /tmp/<filename>.zip -d effects/<filename>
rm /tmp/<filename>.zip
```

Place `effects/` at the project root (sibling of `src/`). Gradle copies from `../../effects` into Android assets automatically. For iOS, add `effects/` via Xcode (`File → Add Files to '<Your project>'...`) as a **folder reference**.

#### 8. Write src/App.tsx (Write tool)

```tsx
import React, {useEffect, useRef} from 'react';
import {View} from 'react-native';
import BanubaSdkManager, {EffectPlayerView} from '@banuba/react-native';

const BANUBA_TOKEN = '<#Place your token here#>';
const EFFECT_PATH = 'effects/TrollGrandma';

export default function App() {
  const initializedRef = useRef(false);

  useEffect(() => {
    if (!initializedRef.current) {
      BanubaSdkManager.initialize([], BANUBA_TOKEN);
      initializedRef.current = true;
    }
  }, []);

  useEffect(() => {
    BanubaSdkManager.attachView();
    BanubaSdkManager.openCamera();
    BanubaSdkManager.startPlayer();
    BanubaSdkManager.loadEffect(EFFECT_PATH);

    return () => {
      BanubaSdkManager.stopPlayer();
    };
  }, []);

  return (
    <View style={{flex: 1}}>
      <EffectPlayerView style={{flex: 1}} />
    </View>
  );
}
```

Replace `'<#Place your token here#>'` with the actual client token.

#### 9. Run (Bash)

```bash
yarn android   # or yarn ios
```

### React Native pitfalls

- `minSdkVersion` must be 26+ - lower values cause build failure.
- Missing `NSCameraUsageDescription` in Info.plist causes a crash on iOS 14+.
- Banuba pod source must be in `Podfile` - without it `pod install` fails.
- On iOS, effects must be added as a **folder reference** in Xcode (blue folder icon), not as a group.
- Effects folder must be at the project root - Gradle copies from `../../effects` relative to `android/app/`.
- If more SDK capabilities are needed than the package exposes, go with native Android/iOS integration.

---

## Shared principles (all platforms)

- **Multi-face**: always confirm the client token includes Max Faces support before implementing multi-face UI. Without it, the SDK silently tracks only one face - no runtime workaround exists on any platform. Banuba Studio produces single-face effects only. For multi-face effects, direct to the [contact form](https://www.banuba.com/contact).
- **AR Cloud**: for remote effect delivery on any platform, read `docs/tutorials/development/guides/ar_cloud.md`.
- **Custom effects**: the agent configures prefab parameters at runtime - it cannot author effect bundles. Custom AR masks, makeup looks, and 3D assets are created in [Banuba Studio](https://studio.banuba.com/). 3D avatars/models are not supported in Studio - direct to the [contact form](https://www.banuba.com/contact).
- **Token security**: never check the client token into source control on any platform.
- **Output format**: complete files or diffs, numbered steps, working code first then explanation.

## Further reading

- Full docs: [docs.banuba.com/far-sdk](https://docs.banuba.com/far-sdk)
- LLM-optimized docs: [llms-full.txt](https://docs.banuba.com/far-sdk/llms-full.txt)
- Samples: [quickstart-web](https://github.com/Banuba/quickstart-web) · [android](https://github.com/Banuba/banuba-sdk-android-samples) · [ios](https://github.com/Banuba/banuba-sdk-ios-samples) · [desktop](https://github.com/Banuba/quickstart-desktop-cpp) · [flutter](https://github.com/Banuba/banuba-sdk-flutter) · [react-native](https://github.com/Banuba/banuba-sdk-react-native)
