# The URL rewriting specification
`proposal status`

## Why using rewriting ?

Resizing pictures costs cpu and memory so caching the result becomes important for scalling the application. 

With a reverse proxy like akamai, varnish, or nginx you can send headers from scripts and force serving cache, but
for many little applications this kind of configuration can be hard to deploy and maintain.

A common trick is to use the 404 fallback over a picture to generate the missing ressource for the first time,
and after serve it directly with the server from the file system.

## The scope of this specification

The scope of this specification is to define an alternative syntax of the RIAPI based on path arguments. This rules
does *not change* any part of the GET parameters version.

## Path and source object identity

Taking for example the bellow original file :

/files/pictures/my_logo.png

The caching system is for example hosted in the folder `/static/`, so the same picture could be found at :

/static/sid/files/pictures/my_logo.png

*NOTE* : The sid path means `source identity` and after you will find the path of the 

Another base64 variant could also do the same thing (based on the original proposal) :

/static/b64/ZmlsZXMvcGljdHVyZXMvbXlfbG9nby5wbmc.png
(ZmlsZXMvcGljdHVyZXMvbXlfbG9nby5wbmc for `files/pictures/my_logo.png`)

*NOTE* : The file extension **MUST** be keept to enable the server to send the correct content-type header.

## Injecting parameters

The query string (parameters and values) must be part of the path, but without having colisions with the source path :

The parameter name is followed by a point and after the point you have the value of the parameter.

The `sid` or `b64` directories will indicate that the rest of the following path will define the source path. 

/static/param.value/...etc.../[sid|b64]/path to the ressource

Sample resizing :

/static/w.120/h.60/sid/pictures/my_logo.png
Will resize the picture /pictures/my_logo.png with with=120 and height=60

## Parameters : ordering, aliases and default values

In a query string the order of parameters doesn't matters, but with the caching the parameters are building the 
cache key, so the same result can be stored with same parameters but at many places :

/static/w.120/h.60/sid/pictures/my_logo.png

is the same as :

/static/h.60/w.120/sid/pictures/my_logo.png

To avoid duplicating cache entries and waste disk space you should follow the vocabulary instructions from 
level 1 to N, and if available, the short form of parameters (should use w but not width).

If a parameter contains a default value `../mode.pad/...` the parameter and it's value should not be 
used for storing the key.

List of ordering parameters :

**LEVEL 1 :** 

  * w (width)
  * h (height)
  * mode (default=pad)
  * scale (default=down)
  * v (version=1.0)

**LEVEL 2 :**

  * format
  * quality
  * bgcolor (default=ffffff)
  * anchor
  * srotate
  * rotate
  * sflip
  * flip
  * subsampling

### What to do when the path contains bad ordering ?

You must not serve the content and send a 301 header(http://en.wikipedia.org/wiki/HTTP_301).

Example :
/static/h.60/width.120/mode.pad/sid/pictures/my_logo.png

Response : 
301 -> Location : /static/w.120/h.60/sid/pictures/my_logo.png






