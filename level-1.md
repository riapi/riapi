# RESTful Image API Level 1 Specification

## Introduction

This specification defines a URI syntax that compliant HTTP servers and clients can use to communicate image modification instructions.

## Scope of Level 1

* Width and height constraints
* Constraint modes
* Upscaling & downscaling conrols
* Default background color

## Level 1 specification 

### URI model

All URIs MUST be [valid URIs per RFC 3986][1].

Compliant servers MUST parse the querystring according to [RFC 3986 section 3.4][2], which defines the query as the text between the first '?' characer in the URL and the first '#' character or the end of the string.

The '&' character MUST be a supported delimiter between pairs, and the '=' character MUST be a supported delimiter between the name and value. After being split into a collection of name and value pairs, both names and values MUST be URL decoded exactly once. Repeated URL decoding is not permitted.

[W3C suggests][3] supporting the ';' character as well as '&' for delimiting pairs. We also suggest supporting this, but do not require it. We do, however, REQUIRE that clients URL-encode semicolons that are not intended as delimiters, regardless of the server they are targeting. 

Certain CDNs and reverse proxies strip querystrings. As this is catasrophic to our purposes, we suggest the following workaround: 

* Servers MAY support parsing the querystring starting with the first ';' character and ending at the first '#' character or the end of the string. 

Servers are NOT required to support duplicate values. Clients are PROHIBITED from specifying the same command name twice.

### Instruction Vocabulary

* width/w=[positive integer]
* height/h=[positive integer]
* mode=max|pad|crop|sretch
* scale=down|both|canvas

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

Based on [surveys][4], users do not expect images to be upscaled above their original size. This makes sense for bandwith and image quality reasons, but can be disconcerting if HTML layout depends upon a certain result. Thus, it is important that the behavior can be controlled.

* `scale`=`down` (default): If the constraints are larger than the source image, the resulting image will use the source dimensions instead, foregoing any cropping, padding, or stretching.
* `scale`=`both`: Enables upscaling. Images will be upscaled to match constrains and may be cropped, padded, or stretched in order to modify the aspect ratio.
* `scale`=`canvas`: Enables upscaling of the canvas, but not the image. Above the original image size, padding will be used to reach the requestd dimensions.


### Mime-type

The correct mime-type MUST be served for the resulting image. 

* Jpeg: "image/jpeg"

### Encoding

The server should attempt to return the image in the most similar encoding available to the source encoding. I.E, if GIF encoding is not available, PNG encoding should be used. If PNG encoding is not available, Jpeg encoding should be used with a white matte color.

## Definitions

* Non-empty: Containing a string of one or more characters.
* RIAPI: RESTful Image API


### Integer parsing

Integers must be parsed in culture-invariant manner. Commas must be discarded. Periods mark the end of the integer portion, and the remaining numerals can be discarded.

[1]: http://tools.ietf.org/html/rfc3986
[2]: http://tools.ietf.org/html/rfc3986#section-3.4
[3]: http://www.w3.org/TR/1999/REC-html401-19991224/appendix/notes.html#h-B.2.2
[4]: http://imageresizing.net/plugins/defaultsettings



