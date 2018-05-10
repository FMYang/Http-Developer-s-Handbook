#### Pragma

ThePragmaheader is theprimary method for controlling caching behavior in HTTP/1.0. Because HTTP/1.1 introduces the more flexibleCache-Controlgeneral header, use of thePragmaheader is dwindling. Most developers who make use of thePragmaheader do so as a safety measure only in order to support Web clients that might not be compliant with HTTP/1.1.

Additionally, because thePragmaheader was replaced as more control over caching became necessary, it was only implemented with one possible form:

`Pragma: no-cache `

Although other forms are allowed by the specification \(in order to remain flexible to future implementations\), the use of theCache-Controlgeneral header has curtailed the use of any other forms of thePragmaheader.

When used in the way just illustrated, all intermediate proxies are to forward the HTTP request to the origin server rather than replying to the Web client with a cached copy.

According to the HTTP specification, this header is intended to be used in HTTP requests as a way for the Web client to indicate that it wants the request forwarded all the way to the origin server. In practice, however, it is also used by Web servers to indicate that the HTTP response should not be saved in a caching system, and support for this behavior is fairly consistent.

>**Note
**
Related material can be found in the description of theCache-Controlgeneral header earlier in this chapter, as well as in[Chapter 14](itss://chm/0672324547_ch14.html#ch14), "Leveraging HTTP to Enhance Performance."

