# react\_native

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
