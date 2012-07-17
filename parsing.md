# RIAPI Paths & Parsing Rules

RESTful Image API Paths & Parsing Rules, `ver: under development`

This document defines

* How paths must identify a source file, even if the querystring is removed.
* Commonly accepted syntaxes for remote files
* Methods for encoding data in the path
* Querystring parsing rules
* Case-sensitivity
* Numeric parsing rules

### URI model

All URIs MUST be [valid URIs per RFC 3986][1].

### Paths and source object identity

The RIAPI specification offers implementations full control over how their physical & virtual file hierarcies are structured. 

We **REQUIRE** only that paths include all neccessary information to locate the original source file. I.e, for request `/folder/image.jpg?width=100`, `/folder/image.jpg` should point to the original file, even if a 403 is returned due to authorization rules.

This means that paths **CANNOT** be specificed as a querystring parameter, although related security credentials (such as a signed hmac of the path) may.

The benefits of this requirement include

1. Uniformity of request structure
2. Readability & maintainability
3. REST compliance 
4. Easier integration with web server authorization modules.
5. Use of relative URLs within HTML. 

This does not mean that you cannot implement an RIAPI-compliant system as a 'handler file' which recieves all arguments in the querystring, but it does mean you will need to use URL rewriting to achieve it, and you will face harder challenges implementing security.

It is suggested that, regardless of platform or language, a 'server module' implementation approach typically allows deeper integration, better performance, and easier security when comparted to a 'handler file' approach.

### Remote Source Files (Optional Guidance)

Files not present on the web server filesystem will require virtualized access.

Common external datastores are S3, Azure, SQL, MongoDB GridFS, and remote HTTP. 

Here are a few sample paths:

	/s3/bucket-name/blob-name.jpg
	/s3/bucket-name/folder/blob-name.jpg
	/s3/bucket-name/image-name
	/azure/container/filename.png
	/azure/other-container/name.jpg
	/sql/f73910f056eb33.jpg
	/sql/f73910f056eb33
	/gridfs/filename.jpg
	/gridfs/folder/filename.jpg
	/gridfs/id/4f44195642f73910f056eb33.jpg
	/remote/othersite.com/otherfolder/image.jpg?width=100&height=200
	/remote/www_othersite_com/otherfolder/image.jpg?width=100&height=200

There is often a mismatch of permitted character sets between the identifier of the remote object and the web server path. This typically cannot be fixed via URL encoding, as servers URL-decode paths prior to parsing and validation. 

If support for arbitrary HTTP files is to be supported, a solution is required.

We suggest using base64u (see end of page) to encode this data, and using the prefix '/b64/' to indicate that the remainder of the path is encoded in URI-safe Base64 format.

	/s3/b64/aHR0cDovL2ltYWdlcmVzaXppbmcubmV0L2ltYWdlLmpwZw

After decoding the data, it MUST only be passed to the assigned virtual provider to prevent exploitation of other, unprepared systems.

Also, be aware that some frameworks normalize incoming paths to lower case, which could destroy the base64 encoding. The original unmodified URL is typically available, however, so this may just require extra effort.

It is EXTREMELY important that you validate this data and apply authorization rules, as it will be bypassing all of your server's built-in url & security filtering.


### Remote data security

We **STRONGLY** suggest either a whitelist or cryptographic signtaure approach to remote file access to prevent exploitation. 

We also suggest re-encoding all image data pulled from an untrusted source, to prevent dual-mode XSS attacks. 

### Query parsing

Compliant servers MUST parse the querystring according to [RFC 3986 section 3.4][2], which defines the query as the text between the first '?' characer in the URL and the first '#' character or the end of the string.

The '&' character MUST be a supported delimiter between pairs, and the '=' character MUST be a supported delimiter between the name and value. After being split into a collection of name and value pairs, both names and values MUST be URL decoded exactly once. Repeated URL decoding is not permitted.

[W3C suggests][3] supporting the ';' character as well as '&' for delimiting pairs. We also suggest supporting this, but do not require it. We do, however, REQUIRE that clients URL-encode semicolons that are not intended as delimiters, regardless of the server they are targeting. 

Certain CDNs and reverse proxies strip querystrings. As this is catasrophic to our purposes, we suggest the following workaround: 

* Servers MAY support parsing the querystring starting with the first ';' character and ending at the first '#' character or the end of the string. If this is supported, a hybrid URL like `image.jpg;width=100?height=200` MUST be supported. 

Servers are NOT required to support duplicate values, but MAY do so, taking the value of the last duplicate instead of returning HTTP 500. Clients are PROHIBITED from specifying the same command name twice.

## Case sensitivity

All command name and enumeration value comparisions must be performed in an ordinal, culture-invariant, case-insensitive manner. 

The following URL is valid: `image.jpg?wIDth=30&moDe=crOp` and must be parsed like `image.jpg?width=30&mode=crop`.

## Numeric parsing

Integers must be parsed in culture-invariant manner. Commas must be discarded. Periods mark the end of the integer portion, and the remaining numerals can be discarded.

For all integer types, commas are the thousands separator, and periods separate the integral and decimal portions. 

We suggest storing floating-point values in variables with at least 15 digits of precision.

## URI-safe Base64 encoding (Base64U)

URI-safe Base64  uses '-' instead of '+' and '_' instead of '/', and '=' padding is optional. 

See [RFC 4648 Section 5 for official details](http://tools.ietf.org/html/rfc4648#section-5)

### C# Example of Base64U Encode/decode

   public static string ToBase64U(byte[] data) {
        return Convert.ToBase64String(data).Replace("=", String.Empty).Replace('+', '-').Replace('/', '_');
    }
    
    public static byte[] FromBase64UToBytes(string data) {
        data = data.PadRight(data.Length + ((4 - data.Length % 4) % 4), '='); //if there is 1 leftover octet, add ==, if 2, add =. 3 octects = 4 chars. 
        return Convert.FromBase64String(data.Replace('-', '+').Replace('_', '/'));
    }
       


