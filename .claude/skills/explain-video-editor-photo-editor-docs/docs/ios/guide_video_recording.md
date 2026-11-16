# Video recording integration guide on iOS

> Video recording integration guide on iOS

# Video recording integration guide on iOS

Guide to modifying video recording feature in Video Editor SDK.

## Quality details
The subsequent table describes video quality details used for video recording in various resolutions with h264 codec.
If h265 (HEVC) codec is used for recording a video, the values listed below will be reduced by approximately 34%:

| Recording speed            | 360p(360 x 640) | 480p(480 x 854) | 540p(540 x 960) |  HD(720 x 1280) | FHD(1080 x 1920) | QHD(2560 x 1440) | 4K(3860 x 2160) |
| -------------------------- | --------------- | --------------- | --------------- | --------------- | ---------------- | ---------------- | --------------- |
| 0.5x, 1x (Default), 2x, 3x | 1.2 Mbits       | 2 Mbits         | 2.4 Mbits       | 3.6 Mbits       | 5.8 Mbits        | 16 Mbits         | 36 Mbits        |

## Implement configurations
`VideoEditorConfig` is the core class used for customizing all features in the Video Editor SDK.
The class includes many internal config classes that are very useful if you want to create your custom experience.  

`RecorderConfiguration` is the main configuration class in `VideoEditorConfig` and is used for the video recording functionality:

| Property |               Values                | Description |
| ------------- |:-----------------------------------:| :------------- |
| videoResolution |    VideoResolutionConfiguration     | defines the resolution configuration for a video recording
| loopAudioWhileRecording |      Bool; Default  `true`      | defines if the audio used in the video recording should be looped
| isDynamicMusicTitle |      Bool; Default `false`      | defines if the music title on the screen changes when a new track is applied
| isDefaultFrontCamera |      Bool; Default `false`      | set `true` if you want to open camera in the front mode
| useHEVCCodecIfPossible |      Bool; Default `true`       | enables H265 codec for recording if it is available on the device
| isPhotoSequenceAnimationEnabled |      Bool; Default `false`      | should use animation for photo sequences
| isAudioRateEqualsVideoSpeed |      Bool; Default `false`      | the video speed selected by the user is applied to audio
| isGalleryButtonHidden |      Bool; Default `false`      | defines if the gallery button located in the bottom-right should be hidden. `true` will hide the button 
| supportMultiRecords |      Bool; Default `true`       | defines if the user can record multiple video files
| takeAudioDurationAsMaximum | Bool; Default `false` | limits the maximum length of a video recording to match the duration of the audio used in the recording 

The next very handy config class is `VideoEditorDurationConfig` that is responsible for customizing video recording durations:

:::warning

All values are in seconds.

:::

| Property |                           Values                            | Description |
| ------------- |:-----------------------------------------------------------:| :------------- |
| maximumVideoDuration |            TimeInterval > 0; Default `120.0`            | the maximum allowed video duration
| videoDurations | [TimeInterval] not empty; Default  `[60.0, 30.0, 15.0]` | the array of durations allowed for the video recording. The user sees a certain button and can change the duration by tapping. For example,  `60.0` means that the user can record multiple video sources with the total duration no more than `60.0` seconds.
| minimumDurationFromCamera |             TimeInterval > 0; Default `3.0`             | the minimum allowed video duration required to proceed and open the next screen
| minimumDurationFromGallery |             TimeInterval > 0; Default `0.3`             | the minimum allowed video duration displayed on the gallery screen
| minimumVideoDuration |             TimeInterval > 0; Default `1.0`             | the minimum allowed video source duration that can be recorded on the camera screen
| minimumTrimmedPartDuration |             TimeInterval > 0; Default `0.3`             | the minimum allowed video source duration to trim
| slideshowDuration |             TimeInterval > 0; Default `0.3`             | the slideshow video duration produced by an image

In this example, the maximum video recording duration is set to 30 seconds:
```swift
var config = VideoEditorConfig()
config.videoDurationConfiguration.maximumVideoDuration = 30.0
```

And `FeatureConfiguration` that helps to customize the use of some specific features on the camera screen where the 
video recording happens:

| Property |          Values           | Description |
| ------------- |:-------------------------:| :------------- |
| isDoubleTapForToggleCameraEnabled | Bool; Default `false` | enables switching between the front and the back camera facing by double tapping on the camera screen
| isMuteCameraAudioEnabled | Bool; Default `false` | indicates if the "mute microphone" button is visible on the camera screen   
| isSpeedBarEnabled | Bool; Default `true`  | enables the speed selection bar. If the bar is disabled, the speed recording will be interactively switched by tapping
| openAutomaticallyPIPSettingsDropdown | Bool; Default `false` | if this property is enabled, the PiP settings drop down view will be presented after opening the camera screen

## Configure the microphone state
The `RecorderConfiguration` class includes the `muteMicrophoneForPIP` property you can use to mute the sound in the PIP mode. The default value is `true`:

```swift
var config = VideoEditorConfig()
config.recorderConfiguration.muteMicrophoneForPIP = false
```

## Configure recording modes
The recording includes 3 modes for recording content implemented in `captureButtonModes` in the `RecorderConfiguration` class:
- `Photo`,
- `Video`,
- `Photo` and `Video` - **default**.

In this example, the recording mode is set to `Video` only:
```swift
var config = VideoEditorConfig()
config.recorderConfiguration.captureButtonModes = [.video]
```

## Configure the record button appearance

The record button is the main UI control on the camera screen which you can fully customize along with the animation that is played by tapping.

Implement the `RecordButtonProvider`, `RecordButton`, `RecordButtonDelegate` protocols to create your custom recording button experience:

```swift
public protocol RecordButtonProvider {
  func getButton() -> RecordButton
}

public protocol RecordButton: UIView {
  var delegate: RecordButtonDelegate? { get set }
  var configuration: RecordButtonConfiguration? { get set }
  func changeViewToIdleState()
  func changeViewToRecordingState()
}

public protocol RecordButtonDelegate: AnyObject {
  var captureButtonMode: CaptureButtonViewMode { get }
  func recordButtonDidTakePhoto(_ recordButton: RecordButton)
  func recordButtonDidCancelTakePhoto(_ recordButton: RecordButton)
  func recordButtonDidStartVideoRecording(_ recordButton: RecordButton)
  func recordButtonDidStopVideoRecording(_ recordButton: RecordButton)
  func recordButtonDidZoomingVideoRecording(_ recordButton: RecordButton, recognizer: UILongPressGestureRecognizer)
}
``` 
Set the new implementation of `RecordButtonProvider` to `RecorderConfiguration.recordButtonProvider`:
```swift
var config = VideoEditorConfig()
config.recorderConfiguration.recordButtonProvider = ...
```
## Camera Layout

Camera Layout is a set of camera recording layouts that combine the camera feed with additional media in a single video: background replacement, `weatherman` overlays and the classic `Picture in Picture` split screens.
It replaces the standalone `PIP` screen used before `1.54.2`.

See [Camera Layouts](guide_camera_layouts.md) for the full guide, the list of supported layouts and the migration steps from `PIP`.
