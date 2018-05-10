#### Uniform Resource Identifiers

Accordingto the W3C's information on addressing \([http://www.w3.org/Addressing/](http://www.w3.org/Addressing/default.htm)\), a Uniform Resource Identifier is defined as "The generic set of all names/addresses that are short strings that refer to resources." A URL, Uniform Resource Locator, is defined as "An informal term \(no longer used in technical specifications\) associated with popular URI schemes: http, ftp, mailto, etc."

Thus, when speaking of URIs in this book, I will refer exclusively to URLs. Let us examine all of the pieces of a URL using a hypotheticalone. Refer to RFC 1808for the official specification.

http://myname:mypass@httphandbook.org:80/mydir/myfile.html?myvar=myvalue\#myfrag

http|scheme (protocol)
------|------
myname|username (optional)
mypass|password (optional)
httphandbook.org|network location (host)
80|port (optional)
/mydir/myfile.html|path (resource)
myvar=myvalue|query string (optional)
myfrag|fragment (optional)


Sometimesit is also helpful to dissect a more common example.

http://httphandbook.org/

http|scheme (protocol)
------|------
httphandbook.org|network location (host)
/|path (resource)

Although the scheme and path can be omitted in most modern Web browsers, they are still required for a correct URL. Browsers will simply assume the HTTP protocol and the root directory \(/\) when these are notspecified.

