#### Range Requests

Range requests, sometimes referred to as byteserving, involve the Web client requesting specific pieces of a resource (specified as a range of bytes) rather than the entire resource. The Range header allows the HTTP client to request partial content, rather than the usual full content, by specifying a range of bytes it seeks to receive. The client should always be prepared to receive the entire content, because this is how a Web server handles an invalid range, and it is also how servers that do not understand the Range header will respond.

To request the first 500 bytes of content, a server can include the following Range header in the request:

`Range: 0-499 `

Ranges are represented using the hyphen character, and multiple ranges can be included, separated by commas. For example, to request the first 500 bytes and the third 500 bytes, you could use this Range header:

`Range: bytes 0-499, 1000-1499`
 
The syntax also allows for open-ended ranges, such as -1000 for bytes 0-1000 and 500- to receive all but the first 500 bytes.

The Content-Range entity header is used to respond to a range request. An example of this header is as follows:

`Content-Range: 600-900/1234 `

This indicates that the content being returned is bytes 600-900 only and that the entire resource is 1234 bytes. It is important to remember that the first byte of a resource is 0, so byte 1233 would be the final byte for a resource with a length of 1234 bytes.

A Content-Range header may only specify a single range. If multiple ranges are requested, they are returned in a multipart message with a content type of multi-part/byteranges.

A response including the successful partial content response will be a 206 Partial Content. If the requested range cannot be fulfilled, a response status code of 416 Requested Range Not Satisfiable and the Content-Range header will be specified as follows:

`Content-Range: */1234 `

When partial content is being successfully fulfilled, a response with a 206 Partial Content status code will include the partial content that is being requested.

An example of software that takes advantage of this technique is the Adobe Acrobat Reader plug-in that reads PDF files from the Web. In order to allow the users to immediately begin viewing a document, the PDF is requested in ranges, so that the first few pages are obtained and rendered prior to the entire document being received. The following example illustrates this scenario.

```
GET /example.pdf HTTP/1.1 
Host: httphandbook.org 
Range: bytes=0-1023 

HTTP/1.1 206 Partial Content 
Last-Modified: Tue, 21 May 2002 12:34:56 GMT 
Accept-Ranges: bytes 
Content-Length: 1024 
Content-Range: bytes 0-1023/2048 
Content-Type: application/pdf 

(binary content) 
```

While the user is viewing the first part of the document, the Adobe Acrobat Reader plug-in continues to request the document in ranges until the entire document has been received.