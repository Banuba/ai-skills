# flutter

[Banuba SDK](https://pub.dev/packages/banuba_sdk) for [Flutter](https://docs.flutter.dev/) is available for iOS and Android and provides the following functionality:

* Load and interact with any Effect (including the `Makeup` effect).
* Still image processing.
* Interaction with Camera (open/close, take photo, flashlight control, facing).
* Screen recording.
* [Videocall](/tutorials/development/videocall.md) powered by [Agora](https://docs.agora.io/).

If this is not enough, you should go with native integration.

## Integration guide[​](#integration-guide "Direct link to Integration guide")

* Android
* iOS

1. Add `banuba_sdk` plugin:

```
flutter pub add banuba_sdk
```

2. Add code from [the basic sample](https://github.com/Banuba/banuba-sdk-flutter/blob/master/example/lib/main.dart) into your app. Don't forget to `initialize` `BanubaSdkManager` with the Client Token:

example/lib/main.dart

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

3. Add `effects` folder into your project. Link it with your app: add the following code into app `build.gradle`.

example/android/app/build.gradle

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

1. Add `banuba_sdk` plugin:

```
flutter pub add banuba_sdk
```

2. Link to **Banuba SDK** podspecs in `ios/Podfile`:

```
source 'https://github.com/sdk-banuba/banuba-sdk-podspecs.git'
```

3. Add code from [the basic sample](https://github.com/Banuba/banuba-sdk-flutter/blob/master/example/lib/main.dart) into your app. Don't forget to `initialize` `BanubaSdkManager` with the Client Token:

example/lib/main.dart

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

4. Add `effects` folder into your project. Link it with your app: add the
   <br />
   <!-- -->
   folder into `Runner` **Xcode** project (`File` -> `Add Files to 'Runner'...`).
