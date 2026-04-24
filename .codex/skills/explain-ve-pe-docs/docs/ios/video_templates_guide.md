# Video Templates on iOS

> Video Templates on iOS

# Video Templates on iOS

Templates let users create stunning videos quickly and easily using predefined sets of effects, transitions, and music. All it takes to make a shareable piece is changing the placeholders. With templates, even people who are new to video editing or just lack time can make impressive content in minutes.

&nbsp;
&nbsp;
&nbsp;

:::important
The ```Video Templates``` is not enabled by default. Contact Banuba representatives to know more.
:::

## Launch Video Templates

"se the ```.videoTemplates``` entry point in ```VideoEditorLaunchConfig``` to launch the Video Editor SDK from Video templates.

```swift
let launchConfiguration = VideoEditorLaunchConfig(
     entryPoint: .videoTemplates,
     hostController: self,
     animated: true
)

videoEditorSDK.presentVideoEditor(
     withLaunchConfiguration: launchConfiguration,
     completion: nil
)
```
