#### Expires

The Expires header indicates a date in HTTP format that specifies when the receiving Web agent believes the resource to be invalid for some reason. For example, this date may specify an expected modification date. This header also implicitly declares that the resource is unlikely to change prior to the given date, thus this is used by many caching systems to approximate when a cached copy of a resource is still valid.

An example of this header is as follows:

`Expires: Tue, 21 May 2002 12:34:56 GMT `

This example indicates that the resource should be considered stale after Tue, 21 May 2002 12:34:56 GMT.

>**Note
**
Although it is in violation of the HTTP standard, you may see Expires: -1 in practice. This is interpreted by Microsoft Internet Explorer to mean that the resource should be considered expired immediately. This improper use should be avoided.


For more information on HTTP date formats, see Chapter 9, "Formatting Specifications."