:::danger
**This is deprecated feature**. [We recommend  you to use Prefabs](../prefabs/overview)
:::

Banuba provides the Makeup API designed to help you integrate augmented reality beauty try-on features into your app. The AR makeup features fit into e-commerce try-on apps, makeovers, selfie editors and portrait retouching software. It aims to overlay realistic makeup onto the face to showcase the product or let users change their appearance. 

  
    Download example
  

:::tip
[Read more about how to use and combine](makeup_usage.md) Makeup API and Face Beauty API features.
:::

The Beauty module allows to enhance the face with the following built-in features: 

## Face Makeup

Sets texture as composite makeup (i.e. all-in-one: eyelashes, shadows, eyeliner, etc).

- `Makeup.set("makeup_texture.png")` - sets an image-file as a makeup texture. The file should be placed into the effect's folder.
  - Supported formats: `.jpeg`, `.jpg`, `.png`, `.ktx`, `.gif`.

  

  ```js
  /* Feel free to add your custom code below */

  Makeup.set("example_makeup.png")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Makeup.set('example_makeup.png')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Makeup.set('example_makeup.png')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Makeup.set('example_makeup.png')")
  ```

  

**Preview**

### Highlighting  

Set highlighter color.

- `Makeup.highlighter("R G B A")` - set highlighter color in R G B A format (separated with space). Each value should be in a range from 0 to 1 (including decimal).

  

  ```js
  /* Feel free to add your custom code below */

  Makeup.highlighter("0.75 0.74 0.74 0.4")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Makeup.highlighter('0.75 0.74 0.74 0.4')", null);
  ``` 

  
  

  ```swift
  // var currentEffect: BNBEffect = ... 

  currentEffect?.evalJs("Makeup.highlighter('0.75 0.74 0.74 0.4')", resultCallback: nil)
  ``` 

  
  

  ```js
  await effect.evalJs("Makeup.highlighter('0.75 0.74 0.74 0.4')")
  ```

  
 

**Preview**
  

**Set a custom highlighter texture**

- `Makeup.highlighter("highlighter.png")` - sets an image-file as a highlighter texture. The file should be placed into the effect's folder.
  - Supported formats: `.jpeg`, `.jpg`, `.png`, `.ktx`, `.gif`.

  

  ```js
  /* Feel free to add your custom code below */

  Makeup.highlighter("highlighter.png")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Makeup.highlighter('highlighter.png')", null);
  ``` 

  
  

  ```swift
  // var currentEffect: BNBEffect = ... 

  currentEffect?.evalJs("Makeup.highlighter('highlighter.png')", resultCallback: nil)
  ``` 

  
  

  ```js
  await effect.evalJs("Makeup.highlighter('highlighter.png')") 
  ```

  

### Contouring

Sets contour color.

- `Makeup.contour("R G B A")` - set contour color in R G B A format (separated with space). Each value should be in a range from 0 to 1 (including decimal).

  

  ```js
  /* Feel free to add your custom code below */

  Makeup.contour("0.3 0.1 0.1 0.6")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Makeup.contour('0.3 0.1 0.1 0.6')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Makeup.contour('0.3 0.1 0.1 0.6')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Makeup.contour('0.3 0.1 0.1 0.6')")
  ```

  

**Preview**

**Set a custom contour texture**

- `Makeup.contour("contour.png")` - sets an image-file as a contour texture. The file should be placed into the effect's folder.
  - Supported formats: `.jpeg`, `.jpg`, `.png`, `.ktx`, `.gif`.

  

  ```js
  /* Feel free to add your custom code below */

  Makeup.contour("contour.png")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Makeup.contour('contour.png')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Makeup.contour('contour.png')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Makeup.contour('contour.png')")
  ```

  

### Foundation

Foundation is a combination of two [Beauty API](./face_beauty) features: [`Skin.color()`](./face_beauty#skin-color) and [`Skin.softening()`](./face_beauty#skin-smoothing-skin-softening).

:::info
Requires [Skin segmentation](/tutorials/capabilities/sdk_features.md#face-ar-sdk-neural-network-features) Neural Network.
:::

  

  ```js
  /* Feel free to add your custom code below */

  // set skin color
  Skin.color("0.73 0.39 0.08 0.3")
  // set softening strength (skin smoothing)
  Skin.softening(1)
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  // set skin color
  mCurrentEffect.evalJs("Skin.color('0.73 0.39 0.08 0.3')", null);
  // set softening strength (skin smoothing)
  mCurrentEffect.evalJs("Skin.softening(1)", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  // set skin color
  currentEffect?.evalJs("Skin.color('0.73 0.39 0.08 0.3')", resultCallback: nil)
  // set softening strength (skin smoothing)
  currentEffect?.evalJs("Skin.softening(1)", resultCallback: nil)
  ```

  
  

  ```js
  // set skin color
  await effect.evalJs("Skin.color('0.73 0.39 0.08 0.3')")
  // set softening strength (skin smoothing)
  await effect.evalJs("Skin.softening(1)")
  ```

  

**Preview**

### Blush

Set blush color.

- `Makeup.blushes("R G B A")` - set blushes color in R G B A format (separated with space). Each value should be in a range from 0 to 1 (including decimal).

  

  ```js
  /* Feel free to add your custom code below */

  Makeup.blushes("0.7 0.1 0.2 0.5")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Makeup.blushes('0.7 0.1 0.2 0.5')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Makeup.blushes('0.7 0.1 0.2 0.5')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Makeup.blushes('0.7 0.1 0.2 0.5')")
  ```

  

**Preview**

**Set a custom blush texture**

- `Makeup.blushes("blushes.png")` - sets an image-file as a blushes texture. The file should be placed into the effect's folder.
  - Supported formats: `.jpeg`, `.jpg`, `.png`, `.ktx`, `.gif`.

  

  ```js
  /* Feel free to add your custom code below */

  Makeup.blushes("blush.png")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Makeup.blushes('blush.png')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Makeup.blushes('blush.png')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Makeup.blushes('blush.png')")
  ```

  

### Softlight

Highlights a face like a directional flashlight.

- `Softlight.strength(n)` - changes softlight intensity, where **n** - any value from 0 to 1 (including decimal). Values > 1 may be also provided.
- `Softlight.clear()` - reset softlight, equals to `Softlight.strength(0)`.

  

  ```js
  // Set softlight strength
  Softlight.strength(1)

  // Reset softlight
  Softlight.clear()
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  // Set softlight strength
  mCurrentEffect.evalJs("Softlight.strength(1)", null);

  // Reset softlight
  mCurrentEffect.evalJs("Softlight.clear()", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...
  
  // Set softlight strength
  currentEffect?.evalJs("Softlight.strength(1)", resultCallback: nil)
  
  // Reset softlight
  currentEffect?.evalJs("Softlight.clear()", resultCallback: nil)
  ```

  
  

  ```js
  // Set softlight strength
  await effect.evalJs("Softlight.strength(1)")
  
  // Reset softlight
  await effect.evalJs("Softlight.clear()")
  ```

  

**Preview**

## Brows makeup

Set brows color.

- `Brows.color("R G B A")` - set brows color in R G B A format (separated with space). Each value should be in a range from 0 to 1 (including decimal).
- `Brows.clear()`- clears brows color.

  

  ```js
  // Set brows color
  Brows.color("0.172 0.125 0.105 0.732")

  // Reset brows color
  Brows.clear()
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  // Set brows color
  mCurrentEffect.evalJs("Brows.color('0.172 0.125 0.105 0.732')", null);

  // Reset brows color
  mCurrentEffect.evalJs("Brows.clear()", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  // Set brows color
  currentEffect?.evalJs("Brows.color('0.172 0.125 0.105 0.732')", resultCallback: nil)

  // Reset brows color
  currentEffect?.evalJs("Brows.clear()", resultCallback: nil)
  ```

  
  

  ```js
  // Set brows color
  await effect.evalJs("Brows.color('0.172 0.125 0.105 0.732')")
  
  // Reset brows color
  await effect.evalJs("Brows.clear()")
  ```

  

  **Preview**
  

## Eye makeup

### Eyeliner

Set eyeliner color.

- `Makeup.eyeliner("R G B A")` - set eyeliner color in R G B A format (separated with space). Each value should be in a range from 0 to 1 (including decimal).

  

  ```js
  /* Feel free to add your custom code below */

  Makeup.eyeliner("0 0 0")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Makeup.eyeliner('0 0 0')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Makeup.eyeliner('0 0 0')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Makeup.eyeliner('0 0 0')")
  ```

  

**Preview**

**Set a custom eyeliner texture**

- `Makeup.eyeliner("eyeliner.png")` - sets an image-file as an eyeliner texture. The file should be placed into the effect's folder.
  - Supported formats: `.jpeg`, `.jpg`, `.png`, `.ktx`, `.gif`.

  

  ```js
  /* Feel free to add your custom code below */

  Makeup.eyeliner("eyeliner.png")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Makeup.eyeliner('eyeliner.png')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Makeup.eyeliner('eyeliner.png')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Makeup.eyeliner('eyeliner.png')")
  ```

  

### Eyeshadow

Set eyeshadow color.

- `Makeup.eyeshadow("R G B A")` - set eyeshadow color in R G B A format (separated with space). Each value should be in a range from 0 to 1 (including decimal).

  

  ```js
  /* Feel free to add your custom code below */

  Makeup.eyeshadow("0.6 0.5 1 0.6")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Makeup.eyeshadow('0.6 0.5 1 0.6')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Makeup.eyeshadow('0.6 0.5 1 0.6')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Makeup.eyeshadow('0.6 0.5 1 0.6')")
  ```

  

**Preview**

**Set a custom eyeshadow texture**

- `Makeup.eyeshadow("eyeshadow.png")` - sets an image-file as an eyeshadow texture. The file should be placed into the effect's folder.
  - Supported formats: `.jpeg`, `.jpg`, `.png`, `.ktx`, `.gif`.

  

  ```js
  /* Feel free to add your custom code below */

  Makeup.eyeshadow("eyeshadow.png")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Makeup.eyeshadow('eyeshadow.png')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Makeup.eyeshadow('eyeshadow.png')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Makeup.eyeshadow('eyeshadow.png')")
  ```

  

### Eyelashes

Set eyelashes color.

- `Makeup.lashes("R G B A")` - set eyelashes color in R G B A format (separated with space). Each value should be in a rage from 0 to 1 (including decimal).

  

  ```js
  /* Feel free to add your custom code below */

  Makeup.lashes("0 0 0")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Makeup.lashes('0 0 0')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Makeup.lashes('0 0 0')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Makeup.lashes('0 0 0')")
  ```

  

  **Preview**
  

**Set a custom eyelashes texture**

- `Makeup.lashes("eyelashes.png")` - sets an image-file as an eyelashes texture. The file should be placed into the effect's folder.
  - Supported formats: `.jpeg`, `.jpg`, `.png`, `.ktx`, `.gif`.

  

  ```js
  /* Feel free to add your custom code below */

  Makeup.lashes("eyelashes.png")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Makeup.lashes('eyelashes.png')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Makeup.lashes('eyelashes.png')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Makeup.lashes('eyelashes.png')")
  ```

  

### Makeup.clear

Global method for Makeup. Clears all Makeup features that have been set.

  

  ```js
  Makeup.clear()
  ```

  
  

  ```java
  mCurrentEffect.evalJs("Makeup.clear()", null);
  ```

  
  

  ```swift
  currentEffect?.evalJs("Makeup.clear()", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Makeup.clear()")
  ```

  

## Lipstick

### Matt

Set lips matte color.

- `Lips.matt("R G B A")` - set lips matte color in R G B A format (separated with space). Each value should be in a range from 0 to 1 (including decimal).
- `Lips.clear()`- clears lips color.

  

  ```js
  // Set lips color 
  Lips.matt("0.85 0.43 0.5 0.8")

  // Reset lips color 
  Lips.clear()
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  // Set lips color
  mCurrentEffect.evalJs("Lips.matt('0.85 0.43 0.5 0.8')", null);

  // Reset lips color
  mCurrentEffect.evalJs("Lips.clear()", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  // Set lips color
  currentEffect?.evalJs("Lips.matt('0.85 0.43 0.5 0.8')", resultCallback: nil)
  
  // Reset lips color 
  currentEffect?.evalJs("Lips.clear()", resultCallback: nil)
  ```

  
  

  ```js
  // Set lips color 
  await effect.evalJs("Lips.matt('0.85 0.43 0.5 0.8')")
  
  // Reset lips color 
  await effect.evalJs("Lips.clear()")
  ```

  

**Preview**

### Shiny

Set lips shiny color.

- `Lips.shiny("R G B A")` - set shiny lips color in R G B A format (separated with space). Each value should be in a range from 0 to 1 (including decimal).
- `Lips.clear()`- clears lips color.

  

  ```js
  // Set lips color 
  Lips.shiny("1 0 0.49 1")
  
  // Reset lips color 
  Lips.clear()
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  // Set lips color
  mCurrentEffect.evalJs("Lips.shiny('1 0 0.49 1')", null);

  // Reset lips color
  mCurrentEffect.evalJs("Lips.clear()", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  // Set lips color 
  currentEffect?.evalJs("Lips.shiny('1 0 0.49 1')", resultCallback: nil)
  
  // Reset lips color 
  currentEffect?.evalJs("Lips.clear()", resultCallback: nil)
  ```

  
  

  ```js
  // Set lips color 
  await effect.evalJs("Lips.shiny('1 0 0.49 1')")
  
  // Reset lips color 
  await effect.evalJs("Lips.clear()")
  ```

  

**Preview**

### Glitter

Set lips glitter color.

- `Lips.glitter("R G B A")` - set lips glitter color in R G B A format (separated with space). Each value should be in a range from 0 to 1 (including decimal).
- `Lips.clear()`- clears lips color.

  

  ```js
  // Set lips color
  Lips.glitter("0.552 0 0 1")
  
  // Reset lips color 
  Lips.clear()
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  // Set lips color
  mCurrentEffect.evalJs("Lips.glitter('0.552 0 0 1')", null);

  // Reset lips color
  mCurrentEffect.evalJs("Lips.clear()", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  // Set lips color 
  currentEffect?.evalJs("Lips.glitter('0.552 0 0 1')", resultCallback: nil)
  
  // Reset lips color 
  currentEffect?.evalJs("Lips.clear()", resultCallback: nil)
  ```

  
  

  ```js
  // Set lips color
  await effect.evalJs("Lips.glitter('0.552 0 0 1')")
  
  // Reset lips color 
  await effect.evalJs("Lips.clear()")
  ```

  

**Preview**

### Extended lips options

It is possible to set up extended lips parameters which are usually pre-defined in Matte, Shiny or Glitter lips.

**Common options**
- `Lips.color("R G B A")` - set lips color in R G B A format (separated with space). Each value should be in a range from 0 to 1 (including decimal).
- `Lips.brightness(n)` - changes lips brightness intensity, where n - any value from 0 to 2 (including decimal). 0 stands for the minimal brightness (black color), 1 stands for standard brightness. Values > 2 may be also provided.

**Shine options**
- `Lips.saturation(n)` - changes shine saturation intensity, where n - any value from 0 to 1 (including decimal).
- `Lips.shineIntensity(n)` - changes shine intensity, where n - any value from 0 to 2 (including decimal). Values > 2 may be also provided.
- `Lips.shineBleeding(n)` - changes shine blending strength, where n - any value from 0 to 1 (including decimal). Values > 1 may be also provided.
- `Lips.shineScale(n)` - changes shine scale, where n - any value from 0 to 1 (including decimal). 0 stands for the minimal scale (shine disabled), 1 stands for standard scale. Values > 1 may be also provided.

**Glitter options**
- `Lips.glitterGrain(n)` - changes glitter grain strength, where n - any value from 0 to 2 (including decimal). Values > 2 may be also provided.
- `Lips.glitterIntensity(n)` - changes glitter intensity, where n - any value from 0 to 2 (including decimal). Values > 2 may be also provided.
- `Lips.glitterBleeding(n)` - changes glitter blending strength, where n - any value from 0 to 2 (including decimal). Values > 2 may be also provided.

  

  ```js
  /* Feel free to add your custom code below */

  Lips.color("1 0 0 1")
  Lips.brightness(1)
  Lips.saturation(1)
  Lips.shineIntensity(2)
  Lips.shineBleeding(1)
  Lips.shineScale(1)
  Lips.glitterGrain(1)
  Lips.glitterIntensity(1)
  Lips.glitterBleeding(1)
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Lips.color('1 0 0 1')", null);
  mCurrentEffect.evalJs("Lips.brightness(1)", null);
  mCurrentEffect.evalJs("Lips.saturation(1)", null);
  mCurrentEffect.evalJs("Lips.shineIntensity(2)", null);
  mCurrentEffect.evalJs("Lips.shineBleeding(1)", null);
  mCurrentEffect.evalJs("Lips.shineScale(1)", null);
  mCurrentEffect.evalJs("Lips.glitterGrain(1)", null);
  mCurrentEffect.evalJs("Lips.glitterIntensity(1)", null);
  mCurrentEffect.evalJs("Lips.glitterBleeding(1)", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Lips.color('1 0 0 1')", resultCallback: nil)
  currentEffect?.evalJs("Lips.brightness(1)", resultCallback: nil)
  currentEffect?.evalJs("Lips.saturation(1)", resultCallback: nil)
  currentEffect?.evalJs("Lips.shineIntensity(2)", resultCallback: nil)
  currentEffect?.evalJs("Lips.shineBleeding(1)", resultCallback: nil)
  currentEffect?.evalJs("Lips.shineScale(1)", resultCallback: nil)
  currentEffect?.evalJs("Lips.glitterGrain(1)", resultCallback: nil)
  currentEffect?.evalJs("Lips.glitterIntensity(1)", resultCallback: nil)
  currentEffect?.evalJs("Lips.glitterBleeding(1)", resultCallback: nil)
  ```

  
  

  ```js  
  await effect.evalJs("Lips.color('1 0 0 1')")
  await effect.evalJs("Lips.brightness(1)")
  await effect.evalJs("Lips.saturation(1)")
  await effect.evalJs("Lips.shineIntensity(2)")
  await effect.evalJs("Lips.shineBleeding(1)")
  await effect.evalJs("Lips.shineScale(1)")
  await effect.evalJs("Lips.glitterGrain(1)")
  await effect.evalJs("Lips.glitterIntensity(1)")
  await effect.evalJs("Lips.glitterBleeding(1)")
  ```

  

## Lips liner

Set lips liner color.

- `LipsLiner.color("R G B A")` - set lips liner color in R G B A format (separated with space). Each value should be in a range from 0 to 1 (including decimal).
- `LipsLiner.clear()`- clears lips liner color.

  

  ```js
  // Set lips color 
  LipsLiner.color("1.0 0.7 0.8 0.8")

  // Reset lips color 
  LipsLiner.clear()
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  // Set lips color
  mCurrentEffect.evalJs("LipsLiner.color('1.0 0.7 0.8 0.8')", null);

  // Reset lips color
  mCurrentEffect.evalJs("LipsLiner.clear()", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  // Set lips color
  currentEffect?.evalJs("LipsLiner.color('1.0 0.7 0.8 0.8')", resultCallback: nil)
  
  // Reset lips color 
  currentEffect?.evalJs("LipsLiner.clear()", resultCallback: nil)
  ```

  
  

  ```js
  // Set lips color 
  await effect.evalJs("LipsLiner.color('1.0 0.7 0.8 0.8')")
  
  // Reset lips color 
  await effect.evalJs("LipsLiner.clear()")
  ```

  

**Preview**
