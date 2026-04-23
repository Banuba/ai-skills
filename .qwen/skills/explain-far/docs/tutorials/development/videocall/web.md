# web

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
