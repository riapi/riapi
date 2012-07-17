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
