# desktop

The steps below apply to desktop integration (**Windows** and/or **macOS**) with **C++**.

1. Download **Banuba SDK** binaries [from GitHub](https://github.com/Banuba/FaceAR-SDK-desktop-releases).

2. Integrate libraries downloaded on the previous step into your build system. If you use **CMake**, consider our [quickstart-desktop-cpp](https://github.com/Banuba/quickstart-desktop-cpp) sample.

for Windows

Besides the **Banuba SDK** itself, you will require third party libraries from the `bin` folder.

3. Create rendering context or copy and paste into your project [ready-to-use helpers](https://github.com/Banuba/quickstart-desktop-cpp/tree/master/helpers/src) sources based on [GLFW](https://www.glfw.org/)

4. Setup `BNB_CLIENT_TOKEN`

helpers/src/BanubaClientToken.hpp

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

5. Initialize **Banuba SDK** with the **Client Token** and path to resources from the archive with the binaries. Create `Player`, `Camera`, `Input` and `Output` and load the **effect**.

realtime-camera-preview/main.cpp

```
loading...
```

![Icon](/img/icons/github.svg "GitHub")

6. Run the application! 🎉 🚀 💅

info

The **effects** are also resources, you may initialize **Banuba SDK** with several resource paths (one for effects and one for SDK assets).

for macOS

Resources for **MacOS** are inside `BanubaEffectPlayer.xcframework`.
