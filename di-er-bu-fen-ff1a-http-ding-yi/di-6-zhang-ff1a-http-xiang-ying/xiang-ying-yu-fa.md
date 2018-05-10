#### Response Syntax

After a Web serverreceives an HTTP request, it will take whatever actions are necessary to provide the requested resource. This might include executing a CGI script or performing server-side logic such as PHP or ColdFusion to generate the resource. The Web server will then respond to the client with an HTTP response. This response is organized similarly to an HTTP request.

An HTTP response is broken into the following three logical pieces:

* Status line

* HTTP headers

* Content

An example status line is as follows:

```
HTTP/1.1 200 OK 
```

The status line contains three elements:

* The version of HTTP being used, in the formatHTTP/x.x

* The status code

* A short description of the status code

Because the status code and its short description are always paired together, even in the definition, this book takes the same approach. You may notice slightly altered short descriptions in practice, but this does not affect the interpretation of the status code.

There are three types of HTTP headers allowed in a response:

* General headers

* Response headers

* Entity headers

In order to focus on HTTP responses, this chapter covers the different status codes that can be included in the status line as well as the headers that are unique to HTTP responses, the response headers. Although they can also be present in an HTTP response, general headers and entity headers are covered separately and referenced here where appropriate.

[  
](itss://chm/0672324547_)

