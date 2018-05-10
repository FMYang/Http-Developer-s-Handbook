#### Content-Range

The Content-Range header is used in situations when the Web server is only returning a portion of the requested resource.

An example of this header is as follows:

`Content-Range: 600-900/1234 `

This indicates that the content being returned is bytes 600-900 only and that the entire resource is 1234 bytes. It is important to remember that the first byte of a resource is 0, so byte 1233 would be the final byte for a resource with a length of 1234 bytes.

This header is, of course, most often seen in a response to an HTTP request that includes the Range request header, such as the following example:

`Range: bytes 600-900 `

Multiple ranges can be requested in a comma-delimited list. The syntax also allows for open-ended ranges, such as -1000 for bytes 0-1000 and 500- to receive all but the first 500 bytes (0-499). However, a Content-Range header can specify only a single range. If multiple ranges are requested, they are returned in a multipart message with a content type of multipart/byteranges.

A response including the successful partial content response will be a 206 Partial Content. If the requested range cannot be fulfilled, a response status code of 416 Requested Range Not Satisfiable is returned, and the Content-Range header will be specified as follows:

`Content-Range: */1234 `

Note that even though the HTTP definition does not limit the use of the Content-Range header to Web servers only, it is currently not used in practice within an HTTP request.

>**Note
**
Related material can be found in the description of the Range request header in Chapter 5 and the 206 Partial Content and 416 Requested Range Not Satisfiable status codes in Chapter 6.

