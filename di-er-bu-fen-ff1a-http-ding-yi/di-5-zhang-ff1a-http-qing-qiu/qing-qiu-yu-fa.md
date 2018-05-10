#### Request Syntax

An HTTP request, which is the message sent from a Web client to a Web server, is comprised of three basic elements:

* Request line

* HTTP headers

* Content

The first line of an HTTP request is always the request line. The request line specifies the request method, the location of the resource, and the version of HTTP being used. These three elements are delimited by spaces. For example:

`GET / HTTP/1.1`

This example specifies the GET request method, the resource located at / (document root), and HTTP/1.1 as the version of protocol used.

The second section of an HTTP request is the headers. HTTP headers include supporting information that can help to explain the Web client's request more clearly. There are three types of HTTP headers that can appear in a request:

* General headers

* Request headers

* Entity headers

There is no requirement pertaining to the order of the headers. Also, because entity headers specify information about the content, they are rarely present in HTTP requests.

>**Note
**
Most HTTP requests do not contain any content, because their intent is usually to request content. However, as you will see, the flexibility to allow content to be sent in a request is very helpful. This is especially true for interactive Web sites, where the users must be able to send data, as well as receive it, in order to interact.


Using the example HTTP request from Chapter 3 (where you searched Google for the term HTTP), you can now apply these terms to something more tangible. The entire HTTP request again is as follows:

```
GET /search?hl=en&q=HTTP&btnG=Google+Search HTTP/1.1 
Host: www.google.com 
User-Agent: Mozilla/5.0 Galeon/1.2.0 (X11; Linux i686; U;) Gecko/20020326 
Accept: text/xml,application/xml,application/xhtml+xml,text/html;q=0.9, 
        text/plain;q=0.8, video/x-mng,image/png,image/jpeg,image/gif;q=0.2, 
        text/css,*/*;q=0.1 
Accept-Language: en 
Accept-Encoding: gzip, deflate, compress;q=0.9 
Accept-Charset: ISO-8859-1, utf-8;q=0.66, *;q=0.66 
Keep-Alive: 300 
Connection: keep-alive 
```

Broken down, the request line is as follows:

`GET /search?hl=en&q=HTTP&btnG=Google+Search HTTP/1.1`

The request method GET, resource /search?hl=en&q=HTTP&btnG=Google+ Search (a relative URL), and HTTP version HTTP/1.1 are delimited by spaces. In this case, the URL is more extensive than the typical / character (denoting document root), because it contains some additional information about the resource we are requesting. The search terms are included in the URL itself. This is due to Google's <form> tag using the method of GET. This technique is an alternative to using the POST method for sending data along with the HTTP request in the content section of the message. The distinction between these two methods (GET and POST) is a common source of confusion for potential Web developers, and this distinction is made clearer in the next section, "Request Methods".

The HTTP headers comprise the remainder of this request, as there is no content. Broken down by type, the general headers are as follows:

```
Keep-Alive: 300 
Connection: keep-alive 
The request headers are:

Host: www.google.com 
User-Agent: Mozilla/5.0 Galeon/1.2.0 (X11; Linux i686; U;) Gecko/20020326 
Accept: text/xml,application/xml,application/xhtml+xml,text/html;q=0.9, 
        text/plain;q=0.8, video/x-mng,image/png,image/jpeg,image/gif;q=0.2, 
        text/css,*/*;q=0.1 
Accept-Language: en 
Accept-Encoding: gzip, deflate, compress;q=0.9 
Accept-Charset: ISO-8859-1, utf-8;q=0.66, *;q=0.66
```

In general, it is fairly easy to discern which category a header belongs to. Request headers specifically relate to something unique to an HTTP request, such as the User-Agent header which identifies the client software being used. General headers are common headers that can (at least theoretically) be used in either an HTTP request or an HTTP response. Entity headers relay information about the content itself (the entity). As this request has no content, it also lacks entity headers.

Keep this HTTP request in mind, as it is used in most of the examples in this chapter.