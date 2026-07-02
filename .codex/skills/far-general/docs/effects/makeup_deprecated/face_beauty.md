:::danger
**This feature is deprecated**. [We recommend  you to use Prefabs](../prefabs/overview)
:::

Banuba provides the Beauty API designed to help you integrate face modification and touch-up functionality into your app. The beautification features fit into a variety of use cases, e.g. live streaming, video chats and video conferencing apps, selfie editors, and portrait retouching software. It aims to make the user feel comfortable about the camera experience. 

  
    Download example
  

:::tip
[Read more about how to use and combine](makeup_usage.md) Makeup API and Face Beauty API features.
:::

:::note
Please [contact us](/support) if you wish to use Makeup API features in iOS version 14.x.
:::

The Beauty module allows to enhance the face via the following built-in features: 

## Teeth Whitening

Allows for a beautiful smile.

- `Teeth.whitening(n)` - changes whitening texture intensity, where **n** - any value from 0 to 1 (including decimal).

  

  ```js
  /* Feel free to add your custom code below */
  
  // set teeth whitening strength
  Teeth.whitening(1)
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  // set teeth whitening strength
  mCurrentEffect.evalJs("Teeth.whitening(1)", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  // set teeth whitening strength
  currentEffect?.evalJs("Teeth.whitening(1)", resultCallback: nil)
  
  ```

  
  

  ```js
  // set teeth whitening strength
  await effect.evalJs("Teeth.whitening(1)")
  ```

  

**Preview**

## Face morphing

Slims down the cheeks and nose to make it more delicate.

- `FaceMorph.eyebrows({spacing: value, height: value, bend: value})` - set eyebrows morph params: spacing - Adjusting the space between the eyebrows [-1;1], height - Raising/lowering the eyebrows [-1;1], bend - Adjusting the bend of the eyebrows [-1;1]
- `FaceMorph.eyes({rounding: value, enlargement: value, height: value, spacing: value, squint: value, lower_eyelid_pos: value, lower_eyelid_size: value, down: value, eyelid_upper: value, eyelid_lower: value})` - set eyes morph params: rounding - Adjusting the roundness of the eyes [0;1], enlargement - Enlarging the eyes [0;1], height - Raising/lowering the eyes [-1;1], spacing - Adjusting the space between the eyes [-1;1], squint - Making the person squint by adjusting the eyelids [-1;1], lower_eyelid_pos - Raising/lowering the lower eyelid [-1;1], lower_eyelid_size - Enlarging/shrinking the lower eyelid [-1;1], down - Eyes down [0;1], eyelid_upper - Eyelid upper [0;1], eyelid_lower - Eyelid lower [0;1]
- `FaceMorph.nose({width: value, length: value, tip_width: value, down_up: value, sellion: value})` - set nose morph params: width - Adjusting the nose width [-1;1], length - Adjusting the nose length [-1;1], tip_width - Adjusting the nose tip width [0;1], down_up - Nose down/up [0;1], sellion - Nose sellion [0;1]
- `FaceMorph.lips({size: value, height: value, thickness: value, mouth_size: value, smile: value, shape: value, sharp: value})` - set lips morph params: size - Adjusting the width and vertical size of the lips [-1;1], height - Raising/lowering the lips [-1;1], thickness - Adjusting the thickness of the lips [-1;1], mouth_size - Adjusting the size of the mouth [-1;1], smile - Making a person smile [0;1], shape - Adjusting the shape of the lips [-1;1], sharp - Lips Sharp [0;1]
- `FaceMorph.face({narrowing: value, v_shape: value, cheekbones_narrowing: value, cheeks_narrowing: value, jaw_narrowing: value, chin_shortening: value, chin_narrowing: value, sunken_cheeks: value, cheeks_jaw_narrowing: value, jaw_wide_thin: value, chin: value, forehead: value})` - set face morph params: narrowing - Narrowing the face [0;1], v_shape - Shrinking the chin and narrowing the cheeks [0;1], chekbones_narrowing - Narrowing the cheekbones [-1;1], cheeks_narrowing - Narrowing the cheeks [0;1], jaw_narrowing - Narrowing the jaw [0;1], chin_shortening - Decreasing the length of the chin [0;1], chin_narrowing - Narrowing the chin [0;1], sunken_cheeks - Sinking the cheeks and emphasizing the cheekbones [0;1], cheeks_jaw_narrowing - Narrowing the cheeks and the jaw [0;1], jaw_wide_thin - Jaw wide/thin [0;1], chin - Face Chin [0;1], forehead - Forehead [0;1]
- `FaceMorph.clear()`- resets morph

  

  ```js
  // Set FaceMorph effects
  FaceMorph.eyebrows({spacing: 0.6, height: 0.3, bend: 1.0})
  FaceMorph.eyes({rounding: 0.6, enlargement: 0.3, height: 0.3, spacing: 0.3, squint: 0.3, lower_eyelid_pos: 0.3, lower_eyelid_size: 0.3})
  FaceMorph.face({narrowing: 0.6, v_shape: 0.3, cheekbones_narrowing: 0.3, cheeks_narrowing: 0.3, jaw_narrowing: 0.7, chin_shortening: 0.3, chin_narrowing: 0.3, sunken_cheeks: 0.1, cheeks_jaw_narrowing: 0.2})
  FaceMorph.nose({width: 0.3, length: 0.2, tip_width: 0.1})
  FaceMorph.lips({size: 0.4, height: 1.0, thickness: 0.1, mouth_size: 0.2, smile: 0.8, shape: 0.4})
  
  // Reset all the FaceMorph effects
  FaceMorph.clear()

  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  // Set FaceMorph effects
  mCurrentEffect.evalJs("FaceMorph.eyebrows({spacing: 0.6, height: 0.3, bend: 1.0})", null);
  mCurrentEffect.evalJs("FaceMorph.eyes({rounding: 0.6, enlargement: 0.3, height: 0.3, spacing: 0.3, squint: 0.3, lower_eyelid_pos: 0.3, lower_eyelid_size: 0.3})", null);
  mCurrentEffect.evalJs("FaceMorph.face({narrowing: 0.6, v_shape: 0.3, cheekbones_narrowing: 0.3, cheeks_narrowing: 0.3, jaw_narrowing: 0.7, chin_shortening: 0.3, chin_narrowing: 0.3, sunken_cheeks: 0.1, cheeks_jaw_narrowing: 0.2})", null);
  mCurrentEffect.evalJs("FaceMorph.nose({width: 0.3, length: 0.2, tip_width: 0.1})", null);
  mCurrentEffect.evalJs("FaceMorph.lips({size: 0.4, height: 1.0, thickness: 0.1, mouth_size: 0.2, smile: 0.8, shape: 0.4})", null);

  // Reset FaceMorph effects
  mCurrentEffect.evalJs("FaceMorph.clear()", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  // Set FaceMorph effects
  currentEffect?.evalJs("FaceMorph.eyebrows({spacing: 0.6, height: 0.3, bend: 1.0})", resultCallback: nil)
  currentEffect?.evalJs("FaceMorph.eyes({rounding: 0.6, enlargement: 0.3, height: 0.3, spacing: 0.3, squint: 0.3, lower_eyelid_pos: 0.3, lower_eyelid_size: 0.3})", resultCallback: nil)
  currentEffect?.evalJs("FaceMorph.face({narrowing: 0.6, v_shape: 0.3, cheekbones_narrowing: 0.3, cheeks_narrowing: 0.3, jaw_narrowing: 0.7, chin_shortening: 0.3, chin_narrowing: 0.3, sunken_cheeks: 0.1, cheeks_jaw_narrowing: 0.2})", resultCallback: nil)
  currentEffect?.evalJs("FaceMorph.nose({width: 0.3, length: 0.2, tip_width: 0.1})", resultCallback: nil)
  currentEffect?.evalJs("FaceMorph.lips({size: 0.4, height: 1.0, thickness: 0.1, mouth_size: 0.2, smile: 0.8, shape: 0.4})", resultCallback: nil)
  
  // Reset FaceMorph effects
  currentEffect?.evalJs("FaceMorph.clear()", resultCallback: nil)
  ```

  
  

  ```js
  // Set FaceMorph effects
  await effect.evalJs("FaceMorph.eyebrows({spacing: 0.6, height: 0.3, bend: 1.0})")
  await effect.evalJs("FaceMorph.eyes({rounding: 0.6, enlargement: 0.3, height: 0.3, spacing: 0.3, squint: 0.3, lower_eyelid_pos: 0.3, lower_eyelid_size: 0.3})")
  await effect.evalJs("FaceMorph.face({narrowing: 0.6, v_shape: 0.3, cheekbones_narrowing: 0.3, cheeks_narrowing: 0.3, jaw_narrowing: 0.7, chin_shortening: 0.3, chin_narrowing: 0.3, sunken_cheeks: 0.1, cheeks_jaw_narrowing: 0.2})")
  await effect.evalJs("FaceMorph.nose({width: 0.3, length: 0.2, tip_width: 0.1})")
  await effect.evalJs("FaceMorph.lips({size: 0.4, height: 1.0, thickness: 0.1, mouth_size: 0.2, smile: 0.8, shape: 0.4})")
  
  // Reset FaceMorph effects
  await effect.evalJs("FaceMorph.clear()")
  ```

  

**Preview**

## Photo filters (LUTs)

Applies color filters to the entire image.

- `Filter.set("lut_texture.png")` - set lut filter texture,
- `Filter.strength(n)` - set lut filter strength, where **n** - any value from 0 to 1 (including decimal), but also larger values may be passed, like 2, 3, etc;
- `Filter.clear()`- clears filter.

  

  ```js
  // Set lut filter characteristics
  Filter.set("lut_texture.png")
  Filter.strength(1)
  
  // Clear filter
  Filter.clear()
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  // Set lut filter characteristics
  mCurrentEffect.evalJs("Filter.set('lut_texture.png')", null);
  mCurrentEffect.evalJs("Filter.strength(1)", null);

  // Clear filter
  mCurrentEffect.evalJs("Filter.clear()", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  // Set lut filter characteristics
  currentEffect?.evalJs("Filter.set('lut_texture.png')", resultCallback: nil)
  currentEffect?.evalJs("Filter.strength(1)", resultCallback: nil)

  // Clear filter
  currentEffect?.evalJs("Filter.clear()", resultCallback: nil)
  ```

  
  

  ```js
  // Set lut filter characteristics
  await effect.evalJs("Filter.set('lut_texture.png')")
  await effect.evalJs("Filter.strength(1)")

  // Clear filter
  await effect.evalJs("Filter.clear()")
  ```

  

**Preview**

## Skin

### Skin smoothing (Skin softening)

Makes the skin look younger by smoothing wrinkles.

- `Skin.softening(n)` - set softening intensity, where **n** - any value from 0 to 1 (including decimal).

  

  ```js
  /* Feel free to add your custom code below */
  
  Skin.softening(1)
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Skin.softening(1)", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Skin.softening(1)", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Skin.softening(1)")
  ```

  

**Preview**

### Skin color 

Changes the face and neck skin color.

- `Skin.color("R G B A")` - set skin color in R G B A format (separated with space). Each value should be in a range from 0 to 1 (including decimal),
- `Skin.clear()` - clears skin color and softening.

:::info
Requires [Skin segmentation](/tutorials/capabilities/sdk_features.md#face-ar-sdk-neural-network-features) Neural Network.
:::

  

  ```js
  // Set skin color
  Skin.color("0.8 0.6 0.1 0.4")

  // Reset skin color
  Skin.clear()
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  // Set skin color
  mCurrentEffect.evalJs("Skin.color('0.8 0.6 0.1 0.4')", null);

  // Reset skin color
  mCurrentEffect.evalJs("Skin.clear()", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  // Set skin color
  currentEffect?.evalJs("Skin.color('0.8 0.6 0.1 0.4')", resultCallback: nil)

  // Reset skin color
  currentEffect?.evalJs("Skin.clear()", resultCallback: nil)
  ```

  
  

  ```js
  // Set skin color
  await effect.evalJs("Skin.color('0.8 0.6 0.1 0.4')")

  // Reset skin color
  await effect.evalJs("Skin.clear()")
  ```

  

**Preview**

## Background separation

:::info
Requires [Background separation](/tutorials/capabilities/sdk_features.md#face-ar-sdk-neural-network-features) Neural Network.
:::

:::tip
If you need only the background separation effect, see [Virtual Background API](../virtual_background).
:::

### Background texture

Sets the file as the background texture.

- `Background.texture("bg_image.png")` - sets an image file as a background texture. 
  - Supported formats: `.jpeg`, `.jpg`, `.png`, `.ktx`, `.gif`.
- `Background.texture("bg_video.mp4")` - sets the video file as a background texture. Visit [technical specification](/tutorials/capabilities/technical_specification#video-formats-support) for supported video formats.

  

  ```js
  /* Feel free to add your custom code below */

  Background.texture("bg_colors_tile.png")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Background.texture('bg_colors_tile.png')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Background.texture('bg_colors_tile.png')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Background.texture('bg_colors_tile.png')")
  ```

  

**Preview**

### Background transparency

Sets the background transparency.

- `Background.transparency(n)` - set transperany value from 0 to 1 (including decimal).

  

  ```js
  /* Feel free to add your custom code below */

  Background.transparency(0.5)
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Background.transparency(0.5)", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Background.transparency(0.5)", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Background.transparency(0.5)")
  ```

  

**Preview**

### Background rotation

Rotates the background texture clockwise in degrees.

- `Background.rotation(deg)` - set rotation value from 0 360 degrees. The value should be divisible by 90 degrees.

  

  ```js
  /* Feel free to add your custom code below */

  Background.rotation(90)
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Background.rotation(90)", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Background.rotation(90)", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Background.rotation(90)")
  ```

  

### Background scale

Scales the background texture.

- `Background.scale(n)` - multiplies the background texture size on given value.

  

  ```js
  /* Feel free to add your custom code below */

  Background.scale(2)
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Background.scale(2)", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Background.scale(2)", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Background.scale(2)")
  ```

  

### Background contentMode

Sets the background texture content mode.

- `Background.contentMode("mode")` - set mode type, possible values: `fill`, `fit`, `scale_to_fill`.

  

  ```js
  /* Feel free to add your custom code below */

  Background.contentMode("fill")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Background.contentMode('fill')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Background.contentMode('fill')", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Background.contentMode('fill')")
  ```

  

### Background blur

Blurs the background behind the user.

- `Background.blur(n)` - sets the background blur radius in [0, 1] range.

  

  ```js
  /* Feel free to add your custom code below */

  Background.blur(0.2)
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Background.blur(0.2)", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Background.blur(0.2)", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Background.blur(0.2)")
  ```

  

**Preview**

### Background clear

Removes the background color and texture, resets any settings applied.

  

  ```js
  /* Feel free to add your custom code below */

  Background.clear()
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Background.clear()", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Background.clear()", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Background.clear()")
  ```

  

## Hair coloring

:::info
Requires [Hair segmentation](/tutorials/capabilities/sdk_features.md#face-ar-sdk-neural-network-features) Neural Network.
:::

### Single color

Dyes hair with one color.

- `Hair.color("R G B A")` - set hair color in R G B A format (separated with space). Each value should be in a rage from 0 to 1 (including decimal),
- `Hair.clear()` - clears hair color (including gradient and hair strands).

  

  ```js
  // Set hair color
  Hair.color("0.39 0.14 0.14 0.8")
  
  // Reset hair color
  Hair.clear()
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  // Set hair color
  mCurrentEffect.evalJs("Hair.color('0.39 0.14 0.14 0.8')", null);

  // Reset hair color
  mCurrentEffect.evalJs("Hair.clear()", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  // Set hair color
  currentEffect?.evalJs("Hair.color('0.39 0.14 0.14 0.8')", resultCallback: nil

  // Reset hair color
  currentEffect?.evalJs("Hair.clear()", resultCallback: nil)
  ```

  
  

  ```js
  // Set hair color
  await effect.evalJs("Hair.color('0.39 0.14 0.14 0.8')")

  // Reset hair color
  await effect.evalJs("Hair.clear()")
  ```

  

**Preview**

### Hair gradient

Dyes hair with 1 to 5 colors.

- `Hair.color("start_color_rgba", "end_color_rgba")` - set hair gradient in R G B A format (separated with space). Each rgba value should be in a rage from 0 to 1 (including decimal),

  

  ```js
  /* Feel free to add your custom code below */

  Hair.color("0.19 0.06 0.25", "0.09 0.25 0.38")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Hair.color('0.19 0.06 0.25', '0.09 0.25 0.38')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Hair.color('0.19 0.06 0.25', '0.09 0.25 0.38')", resultCallback: nil)

  ```

  
  

  ```js
  await effect.evalJs("Hair.color('0.19 0.06 0.25', '0.09 0.25 0.38')")
  ```

  

**Preview**

### Hair strands painting

Dyes hair strands with 1 to 5 colors.

- `Hair.strands("R G B A", "R G B A", "R G B A", ...)` - set hair strands in R G B A format (separated with space) with 5 color maximum.

:::info
Requires [Hair strands painting](/tutorials/capabilities/sdk_features.md#face-ar-sdk-neural-network-features) Add-On.
:::

  

  ```js
  /* Feel free to add your custom code below */

  Hair.strands("0.80 0.40 0.40 1.0", "0.83 0.40 0.40 1.0", "0.85 0.75 0.75 1.0", "0.87 0.60 0.60 1.0", "0.99 0.65 0.65 1.0")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Hair.strands('0.80 0.40 0.40 1.0', '0.83 0.40 0.40 1.0', '0.85 0.75 0.75 1.0', '0.87 0.60 0.60 1.0', '0.99 0.65 0.65 1.0')", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Hair.strands('0.80 0.40 0.40 1.0', '0.83 0.40 0.40 1.0', '0.85 0.75 0.75 1.0', '0.87 0.60 0.60 1.0', '0.99 0.65 0.65 1.0')", resultCallback: nil);
  ```

  
  

  ```js
  await effect.evalJs("Hair.strands('0.80 0.40 0.40 1.0', '0.83 0.40 0.40 1.0', '0.85 0.75 0.75 1.0', '0.87 0.60 0.60 1.0', '0.99 0.65 0.65 1.0')")
  ```

  

**Preview**

## Eyes beautification

:::info
Requires [Eye segmentation](/tutorials/capabilities/sdk_features.md#face-ar-sdk-neural-network-features) Neural Network.
:::

### Eyes coloring

Changes the color of the iris as in virtual lens try on.

- `Eyes.color("R G B A")` - set eyes color in R G B A format (separated with space). Each value should be in a rage from 0 to 1 (including decimal),
- `Eyes.clear()` - clears eyes color including flare and whitening.

  

  ```js
  // Set eyes color 
  Eyes.color("0 0.2 0.8 0.64")

  // Reset eyes color
  Eyes.clear()
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  // Set eyes color
  mCurrentEffect.evalJs("Eyes.color('0 0.2 0.8 0.64')", null);

  // Reset eyes color
  mCurrentEffect.evalJs("Eyes.clear()", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  // Set eyes color
  currentEffect?.evalJs("Eyes.color('0 0.2 0.8 0.64')", resultCallback: nil)
  
  // Reset eyes color
  currentEffect?.evalJs("Eyes.clear()", resultCallback: nil)
  ```

  
  

  ```js
  // Set eyes color
  await effect.evalJs("Eyes.color('0 0.2 0.8 0.64')")

  // Reset eyes color
  await effect.evalJs("Eyes.clear()")
  ```

  

**Preview**

### Eye flare

Makes eyes more expressive adding flare.

- `Eyes.flare(n)` - sets the eyes flare strength from 0 to 1.

  

  ```js
  /* Feel free to add your custom code below */

  Eyes.flare("1")
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Eyes.flare(1)", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Eyes.flare(1)", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Eyes.flare(1)")
  ```

  

**Preview**

### Eyes whitening

Makes the look more expressive by whitening eyes.

- `Eyes.whitening(n)` - sets the eyes sclera whitening strength from 0 to 1.

  

  ```js
  /* Feel free to add your custom code below */
  
  Eyes.whitening(1)
  ```

  
  

  ```java
  // Effect mCurrentEffect = ...

  mCurrentEffect.evalJs("Eyes.whitening(1)", null);
  ```

  
  

  ```swift
  // var currentEffect: BNBEffect = ...

  currentEffect?.evalJs("Eyes.whitening(1)", resultCallback: nil)
  ```

  
  

  ```js
  await effect.evalJs("Eyes.whitening(1)")
  ```

  

**Preview**
