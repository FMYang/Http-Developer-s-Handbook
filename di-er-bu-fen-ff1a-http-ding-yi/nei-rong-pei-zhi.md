#### Content-Disposition

The Content-Disposition header, which is borrowed from RFC 1806, allows for some additional flexibility with regard to media types. The most common use of this header is to force a filename for a file that should be saved rather than rendered. An example of this use is the following:

Content-Disposition: attachment; filename="example.pdf" 
This indicates that the user should be prompted to download the file and that the pre-filled filename should be example.pdf.

>**Note
**
This technique can also resolve the browser flaw mentioned previously, where the file extension is used to determine media type instead of the Content-Type entity header. Thus, Content-Disposition can allow the true filename of the resource to be overridden.

For cases where the resource is intended to be inline, including resources such as streaming media, a format similar to the following can be used:

`Content-Disposition: inline; filename="playlist.m3u" `

Content-Disposition, combined with a proper Content-Type header, provides the developer absolute control over the interpretation of the resource's media type.

### Summary

This chapter completes the material on the HTTP definition. The remainder of the book focuses on practical uses of the points you have learned thus far.

The applied material begins by covering cookies, an extension of the HTTP protocol intended to provide stateful HTTP transactions.