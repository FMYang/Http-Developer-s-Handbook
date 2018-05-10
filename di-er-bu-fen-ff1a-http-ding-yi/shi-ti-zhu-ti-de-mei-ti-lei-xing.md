#### Content-Type

The Content-Type header relays information about the type of the resource being transmitted. One significant advantage to this header is that it allows for many data types to be transmitted using HTTP as a transport.

The most common example of this header is as follows:

`Content-Type: text/html `

Many developers of CGI programs are likely familiar with this statement, as it is often the first bit of output from a CGI script. Because a CGI script could theoretically return any type of content, the Web server delegates the responsibility of indicating the content type to the script itself.

The various allowable content types are maintained by the IANA. More information about these is available in Chapter 10, "Media Types."

>**Note
**
Related material can be found in the description of the Accept request header in Chapter 5.