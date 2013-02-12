# RESTful Image API Level 1 Specification

`under development`

## Introduction

This specification defines a URI syntax that compliant HTTP servers and clients can use to communicate image modification instructions.
The Level 1 Specification only covers basic image resizing commands. 

### Prerequisites

To be compliant with this specification, you must also comply with each of the following:

* [RIAPI Paths & Parsing Rules](https://github.com/riapi/riapi/blob/master/parsing.md)

## Scope of Level 1

* Width and height constraints
* Constraint fit modes
* Upscaling & downscaling controls
* Default background color
* API version selection

## Level 1 Specification - Basic Resizing


### Instruction Vocabulary

* width/w=[positive integer]
* height/h=[positive integer]
* mode=max|pad|crop|stretch
* scale=down|both|canvas
* v=0.1...infinity

### Width and Height

Duplicate values: If both `width` and `w` have non-empty values, the value from `w` will be discarded and the value from `width` will be used. `height` and `h` follow the same rules.

### Constraint mode

Constraint modes only matter when both width and height are specified.

* max - the image will be scaled to fit within the width/height constraint box while maintaining aspect ratio
* pad (default) - the image will be evenly padded with whitespace or transparency to become exactly the specified size while maintaining aspect ratio
* crop - the image will be minimally cropped evenly to match the required aspect ratio
* stretch - the image will be stretched to fit the given dimensions.

Servers MAY support other constraint modes, but MUST support these 4 in order to be Level 1 compliant.

If the output format supports transparency, all padding should be transparent. If transparency is not supported, white MUST be used. FFFFFF, not anything else.

### Upscaling

Based on [surveys][4], users do not expect images to be upscaled above their original size. This makes sense for bandwidth and image quality reasons, but can be disconcerting if HTML layout depends upon a certain result. Thus, it is important that the behavior can be controlled.

* `scale`=`down` (default): If the constraints are larger than the source image, the resulting image will use the source dimensions instead, foregoing any cropping, padding, or stretching.
* `scale`=`both`: Enables upscaling. Images will be upscaled to match constrains and may be cropped, padded, or stretched in order to modify the aspect ratio.
* `scale`=`canvas`: Enables upscaling of the canvas, but not the image. Above the original image size, padding will be used to reach the requested dimensions.

### Versioning

Clients wishing to have portable, future-proof links SHOULD specify the RIAPI version they are aiming for with `v=x.x`. Backwards-incompatible changes are not anticipated, but may occur. 

If the server does not support the RIAPI version specified by the client, it should use the nearest larger version number supported.

### Mime-type

The correct mime-type MUST be served for the resulting image. 

* Jpeg: "image/jpeg"
* Png: "image/png"
* Gif: "image/gif"
* Tiff: "image/tiff"

### Encoding

The three widely supported image formats are: JPEG, PNG, and GIF. Other output formats lack widespread browser support, and SHOULD not be used unless explicitly requested.

If the source format is a web-compatible format, and encoding in the format is supported, the original format should be maintained. 

If a different output format must be chosen, the closest format should be selected based on the following criteria, in the specified order:

1. Color Depth - The output format should support similar or greater color depth.
2. Transparency Support - The output format should support transparency to a similar or greater precision than the input format.

Here are some examples:

| Source format | Ideal output format | Available encoders | Correct choice given available encoders
| --- | --- | --- |
CR2/RAW | JPG | JPG, PNG, GIF | JPG
PNG | PNG | JPG, PNG, GIF | PNG
GIF | GIF | JPG, PNG | PNG
24 or 32-bit PNG | PNG | JPG, GIF | JPG
8-bit PNG | PNG |  JPG, GIF | GIF

If the input format supports transparency, but the output format does not, a matte color MUST be applied, and that color MUST be white (FFFFFF) unless another color is specified in the querystring.

## Definitions

* Non-empty: Containing a string of one or more characters.
* RIAPI: RESTful Image API


[4]: http://imageresizing.net/plugins/defaultsettings



