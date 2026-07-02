Banuba provides the Virtual Background API designed to help you integrate augmented reality background separation into your app. It aims to change your background to hide everything behind you. 

  
    Download example
  

## How to add a background to an effect

Assume you have an effect, say Afro,
and want to add a background image to it, say beach.png .

To accomplish this, connect the built-in Background module to the effect via `evalJs`.
Now, you can set the image as background texture:

  

  ```java
  // Effect mCurrentEffect = ...

  // Connect the built-in background module (once per effect)
  mCurrentEffect.evalJs("Background = require('bnb_js/background')", null);
  // Then, set the background texture
  mCurrentEffect.evalJs("Background.texture('/absolute/path/to/beach.png')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  // Connect the built-in background module (once per effect)
  currentEffect?.evalJs("Background = require('bnb_js/background')", resultCallback: nil);
  // Then, set the background texture
  currentEffect?.evalJs("Background.texture('/absolute/path/to/beach.png')", resultCallback: nil);
  ```

  

### Combine VBG with a WebAR effect (AR 3D Mask)

On the Web platform, the file system is represented by the effect itself.
It means you should put the desired image inside the effect folder before the `evalJs` call.

One way to accomplish this is to put the image directly into the effect archive:
1. Unpack the effect archive
2. Put the image into the effect folder, say as `images/beach.png`
3. Compress the effect folder and use the new archive instead of the original one

This way you should be able to set the image using the relative path:

```js
// const player = await Player.create(...)
// const effect = new Effect(...)
// await player.applyEffect(effect)

// Connect the built-in background module (once per effect)
await effect.evalJs("Background = require('bnb_js/background')")
// Then, set the background texture
await effect.evalJs("Background.texture('images/beach.png')")
```

Another way is to upload an image to the effect's file system on demand.
You can leverage the `Effect.writeFile()` API to accomplish this:

```js
// const player = await Player.create(...)
// const effect = new Effect(...)
// await player.applyEffect(effect)

// Load the image file however you like, e.g from a remote server
const image = await fetch("/path/to/beach.png").then(r => r.arrayBuffer())
await effect.writeFile("images/beach.png", image)

// Connect the built-in background module (once per effect)
await effect.evalJs("Background = require('bnb_js/background')")
// Then, set the background texture
await effect.evalJs("Background.texture('images/beach.png')")
```

**Preview**

The Virtual Background API allows to change background with the following built-in features: 

## Background texture

Sets the background behind the user to a texture.

- `Background.texture('image.png')` - sets an image or a video file as a background texture. The file should be placed into the effect's folder.
- [Supported formats.](/tutorials/capabilities/technical_specification.md#video-formats-support)

  

  ```js
  /* Feel free to add your custom code below */

  Background.texture('image.png')
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Background.texture('image.png')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Background.texture('image.png')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Background.texture('image.png')")
  ```

  

**Preview**

### Background texture content mode

Scales the background content mode.

- `Background.contentMode(mode)` - sets a content mode of background texture. 
- Available mods: `scale_to_fill`, `fill`, `fit`.

  

  ```js
  /* Feel free to add your custom code below */
  
  Background.texture('image.png')
  Background.contentMode('fit')
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Background.texture('image.png')", null);
  mCurrentEffect.evalJs("Background.contentMode('fit')", null);
  ``` 

  
  

  ```swift
  // var currentEffect: BNBEffect = ... 

  currentEffect?.evalJs("Background.texture('image.png')", resultCallback: nil)
  currentEffect?.evalJs("Background.contentMode('fit')", resultCallback: nil)
  ``` 

  
  

  ```js
  await effect.evalJs("Background.texture('image.png')")
  await effect.evalJs("Background.contentMode('fit')")
  ```

  
 

 

### Background texture rotation

Rotates the background texture clockwise in degrees.

- `Background.rotation(angle)` - sets background image rotation angle. Angle should be provided in degrees.

  

  ```js
  /* Feel free to add your custom code below */
  
  Background.texture('image.png')
  Background.rotation(90)
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Background.texture('image.png')", null);
  mCurrentEffect.evalJs("Background.rotation(90)", null);
  ``` 

  
  

  ```swift
  // var currentEffect: BNBEffect = ... 

  currentEffect?.evalJs("Background.texture('image.png')", resultCallback: nil)
  currentEffect?.evalJs("Background.rotation(90)", resultCallback: nil)
  ``` 

  
  

  ```js
  await effect.evalJs("Background.texture('image.png')")
  await effect.evalJs("Background.rotation(90)")
  ```

  
 

**Preview**
  

### Background texture scale 

Scales the background texture.

- `Background.scale(factor)` - sets the scale factor of background texture.

  

  ```js
  /* Feel free to add your custom code below */
  
  Background.texture('image.png')
  Background.scale(2)
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Background.texture('image.png')", null);
  mCurrentEffect.evalJs("Background.scale(2)", null);
  ``` 

  
  

  ```swift
  // var currentEffect: BNBEffect = ... 

  currentEffect?.evalJs("Background.texture('image.png')", resultCallback: nil)
  currentEffect?.evalJs("Background.scale(2)", resultCallback: nil)
  ``` 

  
  

  ```js
  await effect.evalJs("Background.texture('image.png')")
  await effect.evalJs("Background.scale(2)")
  ```

  
 

**Preview**
  

## Background blur

Sets the background blur radius.

- `Background.blur(radius)` - set the background blur radius in range from 0 to 1.

  

  ```js
  /* Feel free to add your custom code below */

  Background.blur(0.6)
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Background.blur(0.6)", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Background.blur(0.6)", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Background.blur(0.6)")
  ```

  

**Preview**
  

## Background transparency

Sets background transparency value.

- `Background.transparency(value)` - set background transparency value in range from 0 to 1. 0 - transparent background disabled , 1 - fully transparent background enabled 

  

  ```js
  /* Feel free to add your custom code below */

  Background.transparency(1)
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Background.transparency(1)", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Background.transparency(1)", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Background.transparency(1)")
  ```

  

**Preview**
