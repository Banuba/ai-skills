# flutter

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
