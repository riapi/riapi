# The URL rewriting specification
`proposal status`

## Why using rewriting ?

Resizing pictures costs cpu and memory so caching the result becomes important for scalling the application. 

With a reverse proxy like akamai, varnish, or nginx you can send headers from scripts and force serving cache, but
for many little applications this configuration can be hard to deploy.

A common trick is to use the 404 fallback over a picture to generate the missing ressource for the first time,
and serve it from the file system after.

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

## Parameters ordering

In a query string the order of parameters doesn't matters, but with the caching the parameters are building the 
cache key, so the same result can be stored with same parameters but at many places :

/static/w.120/h.60/sid/pictures/my_logo.png

is the same as :

/static/h.60/w.120/sid/pictures/my_logo.png






