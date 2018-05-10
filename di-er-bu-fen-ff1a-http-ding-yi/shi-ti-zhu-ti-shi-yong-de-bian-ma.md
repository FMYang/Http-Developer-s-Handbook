#### Content-Encoding

The Content-Encoding header indicates any special encodings that have been performed on the resource. If the resource has been encoded (if this header is present), the encoding only affects the content section of the HTTP message. It is important to note, although this may seem obvious, that the encoding does not affect the content type in any way. Once the content has been properly decoded, it will have the type indicated in the Content-Type entity header. The primary use of encoding is to compress the resource in order to ease and accelerate network transfer.

A Web client will indicate the types of encoding that it supports via the Accept-Encoding request header. This triggers the encoded response that is sent in the HTTP request.

There are three content encodings used in practice by most modern Web agents, discussed as follows.

#### compress

The compress encoding uses the compression algorithm made popular by the Unix program of the same name.

#### deflate

This indicates that a zlib format has been applied to the resource, which was then compressed with deflate. These techniques are defined in RFC 1950 and RFC 1951, respectively.

#### gzip

This indicates that the resource has been compressed using gzip (GNU zip), arguably the most popular compression algorithm in computing today. When this header is included in an HTTP request, and the server cannot handle the decoding necessary, it responds with a status code of 415 Unsupported Media Type. Although it seems that this status code would rarely be used, because the Web client indicates support for gzip encoding with the Accept-Encoding request header, Web clients may erroneously indicate their support.

>**Note
**
Related material can be found in the discussion of the Accept-Encoding request header in Chapter 5, "HTTP Requests."