# RESTful Image API Specification (RIAPI)

This is a work in progress and it has a long ways to go. Please fork and contribute!

Click a filename above, then hit "Edit". When you're done, send a pull request. In the commit, please state that you are releasing your contributions into the public domain.

Home page: http://riapi.org/
Mailing list: https://groups.google.com/d/forum/riapi

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

It was invented in the 1990s, but was mostly exclusive to large corporations until 2003-2007, when affordable (but often slower) implementations started appearing, and even the cheapest hosting plans became capable of performing the task.

2011 saw explosive, exponential growth in the space, and the number of incompatible implementations has been doubling every 6 months. With the proliferation of mobile devices and high-dpi displays, it is becoming impractical to manually export all the required image variants.

### What else is it called?

* Single-source imaging
* Dynamic image resizing
* Server-side resizing


### Why do we need to standardize this?

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

It's not a spec yet, it's still being drafted. In fact, it's not even a draft yet. I'm probably still typing as you read this...

### [RIAPI Paths & Parsing Rules](https://github.com/riapi/riapi/blob/master/parsing.md)

This document serves as the bases for Level 1 and Level 2, and covers the following topics.

* How paths must identify a source file, even if the querystring is removed.
* Commonly accepted syntaxes for remote files
* Methods for encoding data in the path
* Querystring parsing rules
* Case-sensitivity
* Numeric parsing rules

### [RIAPI Level 1](https://github.com/riapi/riapi/blob/master/level-1.md)

Level 1 defines basic resizing commands for images:

* Width and height constraints
* Constraint modes
* Upscaling & downscaling controls
* Default background color
* API version selection

### [RIAPI Level 2](https://github.com/riapi/riapi/blob/master/level-2.md)

Level 2 expands the command set to provide more control

* Format conversion
* JPEG quality selection
* Background/matte color overrides
* Rotation and flipping
* Alignment control (padding and cropping)


## Philosophy

Some image servers accept lists of commands and apply them in order. This places the mathematical burden on client, and often requires the client to have data it does not possess, like the original size of the image. We do not take this approach, although compliant implementations *are* permitted to accept command lists as well.

RIAPI attempts to be descriptive, informing the server of the desired result without prescribing the steps. This frees the server to compose operations for optimal performance. The order of commands in the querystring has *no* bearing on the order of operations. 

We believe this is a more human and javascript-friendly approach, as it assumes no prior knowledge of dimensions nor mathematical prowess. It also covers a overwhelming majority of use cases. 

### Versioning

Specification version identifiers have not yet been assigned, but if you want to link to a specific version of any file, you can click the `History` button while visiting the file, and click `Browse Source` afterwards to get a permalink to that version. We take a 'release early, fix early' approach here, so spelling errors are not uncommon. 


