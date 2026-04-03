# android

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
