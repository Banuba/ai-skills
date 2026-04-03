# ios

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
