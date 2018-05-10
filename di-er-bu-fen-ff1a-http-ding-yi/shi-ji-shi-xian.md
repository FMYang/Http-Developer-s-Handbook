#### Practical Implementations

Media types are used in HTTP headers such as the Accept request header and the Content-Type entity header. They are also used to relay information about the content of Internet mail messages (email).

The Content-Type entity header specifies the media type of the HTTP message content. For example, consider the following HTTP response:

```
HTTP/1.1 200 OK 
Content-Type: text/html 
Content-Length: 19 

<p>This is HTML</p> 
```

The content (<p>This is HTML</p>) is declared to be of media type text/html. This allows the Web client to properly identify and handle the resource. If the resource being requested is named index.html, it may seem that this declaration is unnecessary, because the file extension should indicate the file type. However, HTTP does not depend on something so unreliable, as some file extensions (such as .PHP) can output many different media types. For example, a PHP script can create images, PDF documents, Flash animations, and so forth.

>**Note
**
Some Web browsers, such as recent versions of Internet Explorer, ignore the Content-Type header and instead rely on the file extension of the requested resource to identify the media type.

This characteristic has proven to be particularly frustrating to Web developers who create many different types of media with server-side scripts. One common solution to this flaw is to use a URL variable to trick the browser into mistaking the file extension of the resource to be one that matches the media type. For example, a PHP script that creates a PDF document might be referenced by a URL similar to the following:

http://httphandbook.org/pdf.php?foo=bar.pdf

Thus, even though the real extension is .php, the flawed browsers mistake the extension to be .pdf instead and correctly interpret the resource as being a PDF document.


In cases where the content has been encoded (designated by the Content-Encoding entity header), the Content-Type header refers to the original media type of the entity prior to the encoding.