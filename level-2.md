# RESTful Image API Level 2 Specification

`under development`

## Introduction

This specification defines a URI syntax that compliant HTTP servers and clients can use to communicate image modification instructions.

### Prequisites

To be compliant with this specification, you must also comply with each of the following:

* [RIAPI Paths & Parsing Rules](https://github.com/riapi/riapi/blob/master/parsing.md)
* [RIAPI Level 1](https://github.com/riapi/riapi/blob/master/level-1.md)


## Scope of Level 2

* Format conversion
* Jpeg quality selection
* Background/matte color overrides
* Rotation and flipping
* Alignment control (padding and cropping)

## Level 2 specification 


### Instruction Vocabulary

* format=jpg|png|(other format)
* quality=0..100 (for jpeg encoding only)

* bgcolor=RGB|RGBA|RRGGBB|RRGGBBAA|named color
* anchor=topleft|topcenter|topright|middleleft|middlecenter|middleright|bottomleft|bottomcenter|bottomright
* srotate=-270|-180|-90|0|90|180|270
* rotate=-270|-180|-90|0|90|180|270
* sflip=none|x|y|xy
* flip=none|x|y|xy

* subsampling=444|422|420 (Optional)


## Rotation

Rotation and flipping are the only operations whose order can be controlled. 

To rotate/flip prior to all other operations, use `srotate` and `sflip`. 

To rotate/flip *after* all other operations, use `rotate` and `flip`. 

## Cropping and padding alignment

Use `anchor` to control the alignment used by fit modes 'crop' and 'pad'.

Valid values are `topleft|topcenter|topright|middleleft|middlecenter|middleright|bottomleft|bottomcenter|bottomright`.

When used with `mode=crop`, the anchor specifies the area of the image to be preserved. 
When used with `mode=crop`, the anchor specifies the area of the image that will not get whitespace. 

## Backround Color

If a background color is specified, it MUST be used as the matte color regardless of the output format.

See the Parsing specification for details. 

## Format selection

The user can override the output encoding method with `format`.

* format=jpg|png|gif

The appropriate mime-type MUST be sent back. 

As the source URI will likely have a conflicting file extension, specifying a Content-disposition filename may be useful in allowing for image downloading sanity.

## Jpeg Encoding Customization

All Jpeg encoders support quality adjustment, but only a few support subsampling control. Progressive Jpegs are not widely supported by browsers or libraries and are out of the scope of this specification.

* quality=0..100
* subsampling=444|422|420
