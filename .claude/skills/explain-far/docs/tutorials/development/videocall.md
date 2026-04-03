# Using video calls with the Banuba SDK

In our example, **AgoraRTC SDK** is used for video streaming. But the integration can be done based on any video streaming library.

important

You should have client tokens for both **AgoraRTC SDK** and **Banuba SDK**.<br /><!-- -->To receive **Banuba SDK** token, fill in the [form on banuba.com](https://www.banuba.com/facear-sdk/face-filters#form), or contact us via <info@banuba.com>.<br /><!-- -->To generate an **AgoraRTC SDK** tokens, visit [Agora website](https://www.agora.io/).

* iOS
* Android
* Web
* Flutter
* ReactNative

[Example of using video calls with the Banuba SDK](https://github.com/Banuba/banuba-sdk-ios-samples/tree/master/videocall)

<br />

<br />

## Installation[​](#installation "Direct link to Installation")

1. Add `banuba-sdk-podspecs` repo along with `AgoraRtcEngine_iOS` and `BanubaSdk` packages into the your `Podfile`. Alternatively you may use our [SPM modules](/tutorials/development/installation.md#spm-packages)

info

See the details about the **Banuba SDK** packages in [Installation](/tutorials/development/installation.md).

## Integration[​](#integration "Direct link to Integration")

1. Setup client tokens

videocall/videocall/ViewModel.swift

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

2. Initialize `BanubaSdkManager`

common/common/AppDelegate.swift

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

3. Initialize `AgoraRtcEngineKit`, setup video/audio encoders and join the channel

videocall/videocall/ViewModel.swift

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

4. Setup `Player`, load the effect and start `Camera` frames forwarding

videocall/videocall/ViewModel.swift

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

5. Run the application! 🎉 🚀 💅

[Example of using video calls with the Banuba SDK](https://github.com/Banuba/banuba-sdk-android-samples/tree/master/videocall)

<br />

<br />

For a video call, you need to receive frames as an array of pixels frame by frame in RGBA format. This can be done using `FrameOutput`. Just create a variable called `frameOutput` and add a callback that will receive an array of pixels `framePixelBuffer`:

videocall/src/main/java/com/banuba/sdk/example/videocall/MainActivity.kt

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

Now, when initializing the player, the created variable is set to `player.use(myInput, frameOutput)`.

## Follow these steps to configure Videocall[​](#follow-these-steps-to-configure-videocall "Direct link to Follow these steps to configure Videocall")

important

To get started, add the **Banuba SDK** [integration code](/tutorials/development/basic_integration.md#integration) to your project.

Add the **AgoraRTC SDK** dependency to your `build.gradle.kts`:

videocall/build.gradle.kts

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

This example is based on the [**Player API**](/tutorials/development/basic_integration.md#integration). First, create the **Banuba SDK** core - `player`. Create a `surfaceOutput` that will draw the processed image from the **Banuba SDK**. Create a `frameOutput` that will produce the processed image and transfer it to **Agora** as an array of pixels. And create a camera `cameraDevice` and manage it yourself. **Agora** also has its own camera module, but in this example the **Agora** camera is not used, so `setExternalVideoSource(...)` is called to disable the **Agora** camera:

videocall/src/main/java/com/banuba/sdk/example/videocall/MainActivity.kt

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

Create the **Agora** core with which the videocall will be made - `agoraRtc`, inside we indicate where the **Agora** will draw the received frames:

videocall/src/main/java/com/banuba/sdk/example/videocall/MainActivity.kt

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

And then initialize everything and set up a video call in the` onCreate(...)` method.

## How it works[​](#how-it-works "Direct link to How it works")

Frames from the **Banuba** camera are processed in the `player`, and then the result is passed to the `onFrame(...)` handler. In the handler, frames are passed to the **Agora** module via `agoraRtc.pushExternalVideoFrame(...)`. And then **Agora** transmits the launched frame to the server, and this is how the video call works.

## Fully working code[​](#fully-working-code "Direct link to Fully working code")

MainActivity.kt

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

* Agora
* OpenTok
* WebRTC

### Agora[​](#agora "Direct link to Agora")

tip

Check the official **[Banuba Agora Extension](https://github.com/Banuba/agora-plugin-filters-web)** for Web [@banuba/agora-extension](https://www.npmjs.com/package/@banuba/agora-extension).

Or if you need finer control, you may use the **Banuba WebAR** directly:

1. Import **AgoraRTC** and **Banuba WebAR**

index.html

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

index.html

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

2. Setup client tokens

AgoraAppId.js

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

BanubaClientToken.js

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

3. Initialize `Player`

index.html

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

4. Initialize `AgoraRTC`

index.html

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

5. Connect `Player` to `AgoraRTC`

index.html

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

6. Run the application! 🎉 🚀 💅

tip

See [AgoraWebSDK NG API docs](https://agoraio-community.github.io/AgoraWebSDK-NG/api/en/interfaces/iagorartc.html#createcustomvideotrack) for details.

tip

See [Banuba Video call demo app](https://github.com/Banuba/videocall-web) for more code examples.

### OpenTok (TokBox)[​](#opentok-tokbox "Direct link to OpenTok (TokBox)")

```
import "https://cdn.jsdelivr.net/npm/@opentok/client"
import { MediaStream, Player, Module Effect, MediaStreamCapture } from "https://cdn.jsdelivr.net/npm/@banuba/webar/dist/BanubaSDK.browser.esm.js"

// ...

const camera = await navigator.mediaDevices.getUserMedia({ audio: true, video: true })
const player = await Player.create({ clientToken: "xxx-xxx-xxx" })
await player.addModule(new Module("https://cdn.jsdelivr.net/npm/@banuba/webar/dist/modules/background.zip"))

const webar = new MediaStreamCapture(player)

await player.use(new MediaStream(camera))
player.applyEffect(new Effect("BackgroundBlur.zip"))
player.play()

// original audio
const audio = camera.getAudioTracks()[0]
// webar processed video
const video = webar.getVideoTracks()[0]

const session = OT.initSession("OT API KEY", "OT SESSION ID")
session.connect("OT SESSION TOKEN", async () => {
  const publisher = await OT.initPublisher(
    "publisher",
    {
      insertMode: "append",
      audioSource: audio,
      videoSource: video,
      width: "100%",
      height: "100%",
    },
    () => {},
  )

  session.publish(publisher, () => {})
})

// ...
```

tip

See [TokBox Video API docs](https://tokbox.com/developer/sdks/js/reference/OT.html#initPublisher) for details.

tip

See [Banuba Video call (TokBox) demo app](https://github.com/Banuba/videocall-tokbox-web) for more code examples.

### WebRTC[​](#webrtc "Direct link to WebRTC")

Considering the [Fireship WebRTC demo](https://github.com/fireship-io/webrtc-firebase-demo/blob/main/main.js)

```
import {
  MediaStream as BanubaMediaStream,
  Player,
  Module,
  Effect,
  MediaStreamCapture,
} from "https://cdn.jsdelivr.net/npm/@banuba/webar/dist/BanubaSDK.browser.esm.js"

// ...

webcamButton.onclick = async () => {
  localStream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true })
  remoteStream = new MediaStream()

  const player = await Player.create({ clientToken: "xxx-xxx-xxx" })
  await player.addModule(new Module("https://cdn.jsdelivr.net/npm/@banuba/webar/dist/modules/background.zip"))

  const webar = new MediaStreamCapture(player)

  await player.use(new BanubaMediaStream(localStream))
  player.applyEffect(new Effect("BackgroundBlur.zip"))
  player.play()

  // original audio
  const audio = localStream.getAudioTracks()[0]
  // webar processed video
  const video = webar.getVideoTracks()[0]

  localStream = new MediaStream([audio, video])

  // Push tracks from local stream to peer connection
  localStream.getTracks().forEach((track) => {
    pc.addTrack(track, localStream)
  })

  // ...
}
```

Due to **Flutter** limitations, for every videocall solution you have to create a **native Flutter plugin**. We have developed one for integration with [Agora](https://docs.agora.io). It is expected that you will develop your own [Flutter plugin](https://docs.flutter.dev/packages-and-plugins/developing-packages) if **Agora** isn't suitable for you.

We have created [Agora Extension](https://docs.agora.io/en/video-calling/develop/use-an-extension?platform=flutter) which is accessible from **Flutter**.

[The sample](https://github.com/Banuba/banuba-agora-flutter-sdk) described below is a fork of [Flutter plugin of Agora](https://github.com/AgoraIO-Extensions/Agora-Flutter-SDK). Follow the instructions in `README.md` to run it.

These are the general steps to integrate the sample code into your app:

* Android
* iOS

1. Add the **Client Token** and extension properties keys constants

example/lib/examples/basic/join\_channel\_video/join\_channel\_video.dart

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

2. Add common methods to interact with **Banuba extension**

example/lib/examples/basic/join\_channel\_video/join\_channel\_video.dart

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

3. Initialize **Banuba** and load an **effect**

example/lib/examples/basic/join\_channel\_video/join\_channel\_video.dart

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

4. Copy effects in [`assets/effects`](https://github.com/Banuba/banuba-agora-flutter-sdk/tree/main/example/assets/effects) folder

5. Add a reference to **Banuba Maven repo**

example/android/build.gradle

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

6. Add **Banuba dependencies** and prepare a task to copy effects into app

example/android/app/build.gradle

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

1. Add **Client Token** and extension properties keys constants

example/lib/examples/basic/join\_channel\_video/join\_channel\_video.dart

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

2. Add common methods to interact with **Banuba extension**

example/lib/examples/basic/join\_channel\_video/join\_channel\_video.dart

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

3. Initialize **Banuba** and load an **effect**

example/lib/examples/basic/join\_channel\_video/join\_channel\_video.dart

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

4. Copy effects in [`assets/effects`](https://github.com/Banuba/banuba-agora-flutter-sdk/tree/main/example/assets/effects) folder

5. Add Banuba dependencies to `Podfile`

example/ios/Podfile

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

6. Add the `effects` folder added earlier into your project. Link it with your app: add the folder into `Runner` **Xcode** project (`File` -> `Add Files to 'Runner'...`).

Due to **React Native** limitations, for every videocall solution you have to create a **React native module**. We developed one for integration with [Agora](https://docs.agora.io). It is expected that you will develop your own [React native module](https://reactnative.dev/docs/native-modules-intro) if **Agora** isn't suitable for you.

We have created [Agora Extension](https://docs.agora.io/en/video-calling/develop/use-an-extension?platform=react-native) which is accessible from **React Native**.

[The sample](https://github.com/Banuba/banuba-react-native-agora) described below is a fork of [React Native around Agora](https://github.com/AgoraIO-Extensions/react-native-agora). Follow the instructions in `README.md` to run it.

These are general steps to integrate the sample code into your app:

* Android
* iOS

1. Add the **Client Token** and extension properties keys constants

example/src/examples/basic/JoinChannelVideo/JoinChannelVideo.tsx

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

2. Add the common methods to interact with **Banuba extension**

example/src/examples/basic/JoinChannelVideo/JoinChannelVideo.tsx

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

3. Initialize **Banuba** and load an **effect**

example/src/examples/basic/JoinChannelVideo/JoinChannelVideo.tsx

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

warning

`intiBanuba()` must be called just after `engine.initialize(...)`, calling it later will cause an error during extention loading.

4. Copy effects in [`effects`](https://github.com/Banuba/banuba-react-native-agora/tree/main/example/effects) folder

5. Add a reference to the **Banuba Maven repo**

example/android/build.gradle

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

6. Add **Banuba dependencies** and prepare a task to copy effects into app

example/android/app/build.gradle

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

1. Add **Client Token** and extension properties keys constants

example/src/examples/basic/JoinChannelVideo/JoinChannelVideo.tsx

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

2. Add common methods to interact with the **Banuba extension**

example/src/examples/basic/JoinChannelVideo/JoinChannelVideo.tsx

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

3. Initialize **Banuba** and load an **effect**

example/src/examples/basic/JoinChannelVideo/JoinChannelVideo.tsx

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

warning

`intiBanuba()` must be called immediately after `engine.initialize(...)`, calling it later will cause an error during extention loading.

4. Copy effects in [`effects`](https://github.com/Banuba/banuba-react-native-agora/tree/main/example/effects) folder

5. Add **Banuba dependencies** to `Podfile`

example/ios/Podfile

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

6. Add the `effects` folder added earlier into your project. Link it with your app: add the folder into **Xcode** project (`File` -> `Add Files to '<You project>'...`).
