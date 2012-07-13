# RESTful Image API Specification (RIAPI)

This is a work in progress and it has a long ways to go. Please fork and contribute!

### What is this about?

On-demand, server-side, image scaling and modification, controlled via the query string.

Ex. `image.jpg?width=100&height=100`


### Where is this used extensively?

* E-commerce. Product images are always needed at variety of resolutions.
* Mobile websites
* Responsive web design
* Photography sites
* Social networks
* Agile web development - eliminating the download-Photoshop-upload cycle can save hours per day.

### What else is it called?

* Single-source imaging
* Dynamic image resizing
* Server-side resizing

It was invented in the 1990s, but was mostly exclusive to large corporations until 2003-2007, when affordable (but often slower) implementations started appearing, and even the cheapest hosting plans became capable of performing the task.

### Why do we need to standardize this?

2011 saw explosive, exponential growth in the space, and the number of incompatible implementations has been doubling every 6 months. With the proliferation of mobile devices and high-dpi displays, it is becoming impractical to manually export all the required image variants.

A standardized URL syntax will:

* Allow us to make resuable javascript libraries for
  * Responsive web design
  * In-browser image editing
  * WYSIWG editor integration
* Encourage CMS integration and support
* Free us from platform and vendor lock-in
* Improve content portablility
* Reduce developer and designer frustration through simplifcation
* Facilitate buy-in from your forward-thinking superior.

## Where's the spec? 

It's not a spec yet, it's still being drafted. 

* [Level 1 Specification](level1.md)
* [Level 2 Specification](level2.md)

## Scope of Level 1

* Width and height constraints
* Constraint modes
* Upscaling & downscaling conrols
* Default background color

## Scope of Level 2

* Format conversion
* Jpeg quality selection
* Background/matte color overrides
* Rotation or flipping
* Alignment control (padding and cropping)