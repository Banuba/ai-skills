# react\_native

[Banuba SDK](https://www.npmjs.com/package/@banuba/react-native) for [React Native](https://reactnative.dev/) available for iOS and Android and provides the following functionality:

* Load and interact with any Effect (including `Makeup` effect).
* Interaction with camera (open/close).
* Screen recording (screenshots and video).
* [Videocall](/tutorials/development/videocall.md) powered by [Agora](https://docs.agora.io/).

If this is not enough, you should go with native integration.

## Integration guide[​](#integration-guide "Direct link to Integration guide")

* Android
* iOS

1. Add `@banuba/react-native` dependency

```
yarn add @banuba/react-native
```

2. Add our **Maven repository**

example/android/build.gradle

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

3. Add [`effects` folder](https://github.com/Banuba/banuba-sdk-react-native/tree/master/example/effects) and add a task to copy them into app

example/android/app/build.gradle

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

4. Copy code from [this file](https://github.com/Banuba/banuba-sdk-react-native/blob/master/example/src/App.tsx) into your app. Don't forget to intialize the SDK with the Client Token

example/src/App.tsx

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

1. Add `@banuba/react-native` dependency

```
yarn add @banuba/react-native
```

2. Add our podspecs repo to your `Podfile`

example/ios/Podfile

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

3. Add [`effects` folder](https://github.com/Banuba/banuba-sdk-react-native/tree/master/example/effects) and link it to Xcode project: `(File -> Add Files to ...)`

4. Copy code from [this file](https://github.com/Banuba/banuba-sdk-react-native/blob/master/example/src/App.tsx) into your app. Don't forget to intialize the SDK with the Client Token

example/src/App.tsx

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")
