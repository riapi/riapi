# RESTful Image API Level 1 Specification

## Introduction

This specification defines a URI syntax that compliant HTTP servers and clients can use to communicate image modification instructions.

## Scope of Level 1

* Width and height constraints
* Constraint modes
* Upscaling & downscaling conrols
* Default background color

See Level 2 for

* Format conversion
* Jpeg quality selection
* Background/matte color overrides
* Rotation or flipping
* Alignment control (padding and cropping)

## Level 1 specification 

### URI model

All URIs MUST be [valid URIs per RFC 3986](http://tools.ietf.org/html/rfc3986).

Compliant servers MUST parse the querystring according to [RFC 3986 section 3.4](http://tools.ietf.org/html/rfc3986#section-3.4), which defines the query as the text between the first '?' characer in the URL and the first '#' character or the end of the string.

The '&' character MUST be a supported delimiter between pairs, and the '=' character MUST be a supported delimiter between the name and value. After being split into a collection of name and value pairs, both names and values MUST be URL decoded exactly once. Repeated URL decoding is not permitted.

[W3C suggests](http://www.w3.org/TR/1999/REC-html401-19991224/appendix/notes.html#h-B.2.2) supporting the ';' character as well as '&' for delimiting pairs. We also suggest supporting this, but do not require it. We do, however, REQUIRE that clients URL-encode semicolons that are not intended as delimiters, regardless of the server they are targeting. 

Certain CDNs and reverse proxies strip querystrings. As this is catasrophic to our purposes, we suggest the following workaround: 

* Servers MAY support parsing the querystring starting with the first ';' character and ending at the first '#' character or the end of the string. 

### Instruction Vocabulary

