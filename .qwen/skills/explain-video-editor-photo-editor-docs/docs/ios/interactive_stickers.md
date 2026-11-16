# Custom Interactive Stickers on iOS (Mentions, Hyperlinks, Product / Location)

> Source: https://vebanuba.notion.site/Custom-Interactive-Stickers-on-iOS-Mentions-Hyperlinks-Product-Location-3aefdb8b445b80d29767d9be340adc35

## Overview

The Video Editor SDK's interactive stickers come in two built-in types, **Mention** and **Link**, and you can add any number of your own client-defined types, for example **Product** or **Location**.

Mention and Link ship with the SDK and are turned on through config. Custom types are registered through a small factory contract. The SDK then renders them everywhere a sticker type shows up: the picker button, the timeline bar icon and the sticker pill on canvas.

## Platform availability

| Platform     | Status |
| ------------ | ------ |
| iOS (native) | Supported since BanubaVideoEditorSDK **1.53.2**. Use **1.54.2+** (see breaking changes below). |
| Android      | Not available. The hyperlink sticker is disabled in the SDK. |
| Flutter      | Not exposed in the Flutter plugin. Needs a custom native iOS bridge. |
| React Native | Not exposed in the RN plugin. Needs a custom native iOS bridge. |

## Requirements

- Interactive stickers (Mentions, Hyperlinks and custom types) are available since BanubaVideoEditorSDK 1.53.2.
- The Mentions contacts API was reworked in 1.54.1 to be search-based. Make sure every Banuba pod resolves to 1.54.1 or newer.
- ⚠️ **Breaking change (1.54.1):** `MentionsContactsProvider` changed from a single `contacts()` async call to a search/push model (`search(query:)` + `onReceive`). Code written for 1.53.2–1.54.0 won't compile.
- ⚠️ **Breaking change (1.54.2):** the sticker-placement `id` moved out of `InteractiveStickerPayload` to the sticker itself: `InteractiveStickerSelectionResult(id:)` when you create it, `InteractiveStickerInfo.id` when you read it back. `InteractiveStickerPayload` now carries only entity data (`url`, `attributes`, `mentions`).
- ⚠️ With 1.54.2, BanubaPhotoEditorSDK is updated to **1.4.3**.
- ❗ Interactive stickers are supported **only in the new editor (Editor V2)**. They don't appear in the legacy editor UI.

### CocoaPods (1.54.2, with Face AR)

```ruby
source 'https://github.com/Banuba/specs.git'
source 'https://github.com/sdk-banuba/banuba-sdk-podspecs.git'
platform :ios, '15.0'
use_frameworks!

target 'Example' do
  banuba_sdk_version = '1.54.2'

  pod 'BanubaVideoEditorSDK', banuba_sdk_version
  pod 'BanubaSDK', banuba_sdk_version

  pod 'BanubaARCloudSDK', banuba_sdk_version      #optional
  pod 'BanubaAudioBrowserSDK', banuba_sdk_version #optional

  pod 'BanubaPhotoEditorSDK', '1.4.3'
end
```

For the build without Face AR, replace `BanubaSDK` with `BanubaSDKSimple` and use `source 'https://github.com/CocoaPods/Specs.git'` as the second source.

## Solution overview

- Mention and Link are built in. Set one config value for each, and they appear in the sticker picker without any screen of your own.
- For custom types, implement one factory (`InteractiveStickerSelectionExternalFactory`) that lists your sticker types and creates a selection screen for each. Register it once on the video editor's view controller factory.
- Everything the user places, of any type, comes back through a single property: `videoEditor.interactiveStickers`.
- Button order in the picker is Mention → Link → your custom types, in the order of `customStickers`.

## Enabling Mentions

```swift
config.interactiveStickersConfiguration.mentionsContactsProvider = MyMentionsProvider()
```

The Mention button appears as soon as `mentionsContactsProvider` is non-nil. There is no separate flag.

```swift
public protocol MentionsContactsProvider: AnyObject {
  /// Set by the SDK before the first `search(query:)` call.
  /// Call from any thread. On `.failure` the SDK shows an error state with retry.
  /// Don't call it for a cancelled request.
  var onReceive: (@Sendable (Result<[MentionContact], Error>) -> Void)? { get set }

  /// Invoked each time the search query changes. First call = picker opened, empty `query`.
  func search(query: String)
}
```

Example provider:

```swift
final class MyMentionsProvider: MentionsContactsProvider {
  var onReceive: (@Sendable (Result<[MentionContact], Error>) -> Void)?
  private var searchTask: Task<Void, Never>?

  private let allContacts: [MentionContact] = [
    MentionContact(id: "user-alli.park", username: "alli.park",
                   displayName: "@alli.park · 6.8K followers",
                   avatarUrl: URL(string: "https://.../avatar.png")),
  ]

  func search(query: String) {
    searchTask?.cancel()
    searchTask = Task { [weak self] in
      guard let self else { return }
      try? await Task.sleep(nanoseconds: 300_000_000) // your server call
      if Task.isCancelled { return }
      let results = allContacts.filter {
        query.isEmpty || $0.username.localizedCaseInsensitiveContains(query)
      }
      onReceive?(.success(results))
    }
  }
}
```

- The SDK calls `search` on every query change. Cancel the previous request, and never deliver results for a cancelled one.
- Results come back through `videoEditor.interactiveStickers` (filter with `$0.type == .mention`). `payload.mentions` holds the selected contacts in order. The SDK generates `sticker.id` for Mention stickers.
- You don't supply an icon. The SDK shows an `@` glyph for a single contact, or an avatar collage for a multi-select.

## Enabling Hyperlinks (Link stickers)

```swift
config.interactiveStickersConfiguration.showHyperlinks = true
```

The SDK owns the whole flow: the URL entry screen, validation and pill rendering. Placed link stickers come back through `videoEditor.interactiveStickers` (filter with `$0.type == .link`). The URL is in `InteractiveStickerInfo.url`.

## Custom sticker types (Product / Location)

### 1. Describe the types

```swift
CustomInteractiveSticker(id: "product",  title: "Product",  icon: UIImage(named: "sticker-product") ?? UIImage())
CustomInteractiveSticker(id: "location", title: "Location", icon: UIImage(named: "sticker-location") ?? UIImage())
```

- ❗ `"link"` and `"mention"` are reserved ids. Don't use them for custom types.
- 💡 Use an SF Symbol or a vector asset with "Preserve Vector Data" enabled. The pill on canvas can be scaled up.

### 2. Implement a selection screen per type

```swift
public protocol InteractiveStickerSelectionViewControllerDelegate: AnyObject {
  func interactiveStickerSelectionViewController(_ vc: InteractiveStickerSelectionViewController,
                                                 didSelect result: InteractiveStickerSelectionResult)
  func interactiveStickerSelectionViewControllerDidCancel(_ vc: InteractiveStickerSelectionViewController)
}

public protocol InteractiveStickerSelectionViewController: UIViewController {
  var interactiveStickerSelectionDelegate: InteractiveStickerSelectionViewControllerDelegate? { get set }
}
```

Report the selection:

```swift
finish(with: InteractiveStickerSelectionResult(
  // new placement → fresh id; editing an existing one → keep the id the SDK gave you
  id: selectedResult?.id ?? UUID().uuidString,
  title: item.title,
  payload: InteractiveStickerPayload(url: item.url, attributes: ["item_id": item.id, "sku": "SNK-42"]),
  icon: item.icon
))
```

- The host app owns the whole screen: list, search, categories and network calls. This is where your own backend or catalog data plugs in.
- ❗ Call `didSelect` or `didCancel` on every way off the screen, including the Close button and swipe-down dismiss. Guard against calling it twice. The SDK dismisses the screen itself.
- `InteractiveStickerSelectionResult.id` is the **placement id**. It identifies this one sticker, not the entity behind it. Don't put your catalog id there. Entity identity goes into `payload` (`url`, `attributes`, `mentions`).
- Optional visual overrides:
  - `icon` replaces the type icon for this instance, for example a product thumbnail.
  - `stickerImage` is a fully custom pill visual on canvas. If it's nil, the SDK draws the standard icon + title pill.

### 3. Implement the factory

```swift
public protocol InteractiveStickerSelectionExternalFactory: AnyObject {
  var customStickers: [CustomInteractiveSticker] { get }
  func makeSelectionViewController(for stickerId: String,
                                   selectedResult: InteractiveStickerSelectionResult?)
    -> InteractiveStickerSelectionViewController?
}
```

`selectedResult` is non-nil when the user edits an existing sticker. Use it to prefill your screen.

### 4. Register the factory

```swift
let viewControllerFactory = ViewControllerFactory() // your ExternalViewControllerFactory
viewControllerFactory.interactiveStickersFactory = MyCustomStickersFactory()

let videoEditor = BanubaVideoEditor(
  // ...
  externalViewControllerFactory: viewControllerFactory
)
```

A button for each custom type appears when `interactiveStickersFactory` is non-nil and `customStickers` isn't empty.

### 5. Read back what the user placed

```swift
let products  = videoEditor.interactiveStickers.filter { $0.type.customId == "product" }
let locations = videoEditor.interactiveStickers.filter { $0.type.customId == "location" }
```

`payload` returns whatever was passed at selection time. **The SDK doesn't act on `url` itself.** It never opens a link, so handling it is up to the app.

## Exported video: metadata, not burned-in

`interactiveStickers` returns each placed sticker **as metadata**. The app draws it, typically as live, tappable overlays on top of the exported video during playback. The overlays aren't composited into the exported file, which is what keeps them tappable. Store the metadata alongside the video so other viewers get the overlays.

`InteractiveStickerInfo` fields:

| Field             | Type                | Meaning |
| ----------------- | ------------------- | ------- |
| `id`              | `String`            | Placement id. Stable across draft save/load. |
| `previewImage`    | `UIImage`           | The sticker exactly as it looked on canvas, ready to draw. |
| `normalizedFrame` | `CGRect`            | Geometry as a 0…1 fraction of the video frame. **`origin` is the CENTER**, size is the rendered size. |
| `transform`       | `CGAffineTransform` | Rotation and scale only. Apply it after setting the frame. |
| `start` / `end`   | `TimeInterval`      | Visibility window in seconds. |
| `zPosition`       | `Int`               | Stacking order. Add overlays in ascending order. |
| `url`, `payload`  |                     | Entity data. |

Overlay layout, based on `AVPlayerLayer.videoRect`:

```swift
let frame = sticker.normalizedFrame
let width  = frame.width  * videoRect.width
let height = frame.height * videoRect.height
let centerX = videoRect.minX + frame.minX * videoRect.width
let centerY = videoRect.minY + frame.minY * videoRect.height

imageView.transform = .identity          // reset before setting frame
imageView.frame = CGRect(x: centerX - width / 2, y: centerY - height / 2, width: width, height: height)
imageView.transform = sticker.transform
```

Toggle `isHidden` from a periodic time observer, based on `start <= t && t <= end`.

## Detecting taps

```swift
extension BanubaVideoEditor {
  public static func hitTestInteractiveStickers(
    point: CGPoint, time: TimeInterval, videoRect: CGRect,
    interactiveStickers: [InteractiveStickerInfo]
  ) -> InteractiveStickerInfo?   // topmost (highest zPosition) match
}
```

```swift
@objc private func tapStickerAction(_ recognizer: UITapGestureRecognizer) {
  guard let t = player?.currentTime().seconds else { return }
  guard let sticker = BanubaVideoEditor.hitTestInteractiveStickers(
    point: recognizer.location(in: playerPreview), time: t,
    videoRect: resolvedVideoRect, interactiveStickers: stickers) else { return }

  if let s = sticker.url, let url = URL(string: s), UIApplication.shared.canOpenURL(url) {
    UIApplication.shared.open(url)
  } else {
    handlePayload(sticker.payload)
  }
}
```

`point` and `videoRect` must be in the same coordinate system. The method already handles the center-origin frame and the transform.

## Optional configuration

- **Per-type limits.** A type with no entry has no limit.
  ```swift
  config.interactiveStickersConfiguration.limits = [.link: 1, .mention: 1, .custom("product"): 3]
  ```
- **Baked icon for drafts.** `CustomInteractiveSticker.icon` is rasterized into the sticker metadata when it's placed. Drafts render correctly even if the factory isn't registered yet.
- **Full-span + follow-trim.** Custom stickers default to spanning the whole video and stay pinned to its start and end when the user trims.

## Open questions (confirm with the VE team before promising to clients)

- Flutter, React Native and Android support timeline.
- Licensing: whether interactive stickers need a separate license feature.
