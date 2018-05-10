#### Request Methods

One of the most important attributes of the HTTP request is the request method. This method indicates the overall intent of the Web client's request. Although many methods are rarely used in practice, each serves an important purpose. In most cases, I will discuss each of these as defined under HTTP/1.1, and I will mention differences in previous implementations if appropriate. Priority is given to the specification as commonly implemented rather than to maintaining theoretical purity.

There are eight request methods in HTTP/1.1: GET, POST, PUT, DELETE, HEAD, TRACE, OPTIONS, and CONNECT. HTTP/1.0 specifies three methods (GET, HEAD, and POST), although four others are implemented by some servers and clients claiming to be HTTP/1.0. The support for these four other methods (PUT, DELETE, LINK, and UNLINK) is inconsistent and mostly undefined, although they are each briefly referenced in Appendix D of RFC 1945, the HTTP/1.0 specification.

I will cover the eight methods as defined in HTTP/1.1, as these are the most current as well as the methods most Web clients and servers adhere to. For compatibility concerns, it is usually appropriate to consider previous implementations of a request method by the same name to be compatible with the HTTP/1.1 version. Additionally, nearly all modern Web clients and servers have support for at least HTTP/1.0.

#### The GET Method

The most popular method used by Web clients is GET. This is the type of request your browser uses when you click on a link or type a URL into the browser's location bar. A GET request is basically a request to receive the content located at a specific URL. This is the simplest request method in HTTP as well as the oldest, being the single method available in HTTP/0.9.

**Chapter 1**, "What Is HTTP?," defined the various parts of a URL. The query string, the part of the resource after the ? character and before the # character (if these characters exist), is constructed of one or more name/value pairs, with each pair being delimited by the & character. Thus, a query string specifying three variables looks something like the following:

`var1=value1&var2=value2&var3=value3`

In HTML forms, the <form> tag has an attribute called method that allows for values of get and post. When get is chosen, each form field name and value is included in the URL as the query string using the syntax just noted, and the Web client issues an HTTP GET request to request the content specified by the action attribute of the <form> tag.

Obtaining a URL using the GET method allows users to bookmark the URL, create a link to the URL, email the URL to a friend, and the like. For example, you can bookmark Google search results, and each time you visit the bookmark, you will receive the current results of performing the same search. In the same sense, you could include a link to a set of search results on your own Web site.

If these search results required data obtained via the POST method, bookmarks would have to be handled explicitly so that they do not appear to be broken links. Otherwise, the processing of the POST data would yield unexpected results because it would be absent. Even when absent data is handled explicitly, the rendered page would likely not be what the users were expecting, so the situation needs to be explained. For example, a search where no search terms were specified might state exactly that (no search terms were specified), so it would be beneficial to add additional information to notify the users that using a bookmark might have also been the reason for the lack of search terms.

For many forms, the POST method is preferable. For example, you would not want a user to be able to bookmark a URL that makes a purchase. You also would not want sensitive data appearing in the location bar of a Web browser, because this would expose the data to wandering eyes. GET has a few other drawbacks that you need to consider when choosing a form method. There is a limited amount of data that can be sent from the Web client using this method, and this limit is very inconsistently implemented, so the exact limitation depends on both the Web client and Web server being used. Nearly all modern Web clients and Web servers, including the Apache Web server, can handle URLs up to 1024 characters in length. However, the specification warns against relying on URLs greater than 255 characters in length. Of course, it is important to remember that the URL contains more than just the query string, but you can see how a form with many fields could cause a URL to exceed the capacity of the client and/or server involved in the transaction.

A GET request traditionally contains no content and thus no entity headers.

#### The POST Method

The addition of the POST method is arguably one of the largest improvements of the HTTP specifications to date. This method can be credited with transitioning the Web to a truly interactive application development platform.

The POST method is commonly supported by browsers as a method of submitting form data. If an HTML form specifies a method of POST, the browser will send the data from the form fields in a POST request rather than a GET request. Using the sample HTTP request from Chapter 3, the following example illustrates the same request if the POST method were used instead of GET.

```
POST /search HTTP/1.1 
Host: www.google.com 
User-Agent: Mozilla/5.0 Galeon/1.2.5 (X11; Linux i686; U;) Gecko/20020606 
Accept: text/xml,application/xml,application/xhtml+xml,text/html;q=0.9, 
        text/plain;q=0.8,video/x-mng,image/png,image/jpeg,image/gif;q=0.2, 
        text/css,*/*;q=0.1 
Accept-Language: en 
Accept-Encoding: gzip, deflate, compress;q=0.9 
Accept-Charset: ISO-8859-1, utf-8;q=0.66, *;q=0.66 
Keep-Alive: 300 
Connection: keep-alive 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 31 

hl=en&q=HTTP&btnG=Google+Search 
```

Noteworthy in this HTTP request is that it includes a few entity headers:

```
Content-Type: application/x-www-form-urlencoded 
Content-Length: 31 
```

as well as content:

`hl=en&q=HTTP&btnG=Google+Search`

Also notice that the request line not only specifies a request method of POST, of course, but also that the resource lacks a query string because this information was communicated in the content of the request.

As with the query string of a URL, the data in a POST consists of name/value pairs separated by the & character. Special characters are URL encoded, and the Content-Type header references this fact. For more information on URL encoding, see Chapter 9, "Formatting Specifications."

It has become an unfortunate tendency for Web developers to blindly choose to use the POST method for submitting HTML forms without considering the advantages and disadvantages of each option. The approach I recommend is to consider the nature of the data being submitted prior to making such a decision.

In the example of searching Google, the search terms are all that the client is sending. By using GET, the search results can be referenced by a URL. For example, the following link will display the search results of a Google search for the term HTTP:

```
<a href="http://www.google.com/
graphics/ccc.gifsearch?hl=en&ie=ISO-8859-1&q=HTTP&btnG=Google+Search">HTTP</a> 
```

This is very convenient, as it allows these search results to be obtained with a simple link rather than requiring a form submission as POST does. Also, it allows the search terms to be specified in advance; as the developer, you have some control over the data the Web client submits when you include a link in a Web page. For example, including the previous link in a Web page allows a user to search for HTTP without having to enter it into a form and then click a submit button. In general, GET is more convenient than POST and is preferable when there is a reasonably small amount of data being submitted that can risk exposure.

GET is a poor choice when there is a great deal of data being submitted, because it can increase the length of the URL beyond the capacity of some Web agents. In addition, most users pay no attention to a URL, so including sensitive information on a URL can be dangerous. This is not because the data is easier to obtain while in transit, but rather because it is more exposed once it reaches the Web client. It is displayed in the Web browser's location bar, it might be stored in the browser's history, and the user might mistakenly bookmark it or send it to a friend. Thus, POST is preferable when submitting large amounts of data or when the data is sensitive. However, POST does nothing to protect sensitive data in transit. For a common method of protecting HTTP messages in transit, see Chapter 17, "Secure Sockets Layer."

### The PUT Method

The PUT method is not nearly as common as GET or POST. However, it is useful in certain situations because it allows the Web client to send content that will be stored on the Web server.

The semantics of the PUT method are very similar to POST. The resource in the request line, however, is the requested location for the content to be stored rather than the resource intended to receive the content. All associated entity headers carry the same meaning as with the POST method.

If the PUT request results in the content being created, the Web server responds with a 201 Created response. If the content existed previously, and the PUT request modified it, either a 200 OK or a 204 No Content response will be returned.

>**Note
**
As should be expected, any type of security surrounding such a request should be considered separately. There is no inherent security in a PUT request just as there is no inherent security in a POST request. It is up to the configuration of the Web server as to how such requests are handled. Be sure to consult your Web server's documentation.


It should be noted that the PUT method is very rarely implemented in Web clients. A common misconception is that the PUT method is required for uploading files. However, this capability is actually an enhancement to the POST method as identified in RFC 1867, "Form-based File Upload in HTML". If you try to use the PUT method in your HTML form, the browser will most likely use the default GET method instead, as it will not understand your intent. For example:

<form action="/" method="put"> 
This often results in an attempt to submit the form data in the query string of the URL as a GET request, which is likely not the desired behavior. The method attribute of the <form> tag can only accept the values of get or post. For uploading files, the POST method can be used in combination with an input type of file.

The DELETE Method

As a perfect counterpart to PUT, HTTP provides the DELETE request method. A DELETE request will specify content on the Web server to be deleted as the resource in the request line.

The only interesting thing about the DELETE method is that a successful response of 200 OK by the Web server does not necessarily indicate that the resource has been deleted. It merely indicates that the server's intent is to delete the content. This exception allows for human intervention as a safety precaution.

#### The HEAD Method

HEAD is a very useful request method for people who are interested in finding out more information about the way a certain transaction behaves. The HEAD method is supposed to behave exactly like GET, except that the content is not present. Thus, HEAD is like a normal GET request with all of the HTML stripped away.

For example, using the example request from Chapter 3, you can replace GET with HEAD in the request to yield the following HTTP response:

````
HTTP/1.1 200 OK 
Server: GWS/2.0 
Date: Tue, 21 May 2002 12:34:56 GMT 
Transfer-Encoding: chunked 
Content-Encoding: gzip 
Content-Type: text/html 
Cache-control: private 
Set-Cookie: PREF=ID=58c00...cXi3RhSE; 
            domain=.googl...14:07 GMT 
````
            
This is exactly the same response that the GET request sent, only the content is absent. This is very helpful for debugging problems or just researching server behavior. Because the bulk of a response is usually content that is of little interest in these cases, the HEAD method allows you to suppress the content for the sake of convenience and clarity.

>**Note
**
HEAD is not perfectly dependable for returning the HTTP headers that would be included in a normal response to a GET request. The reason is that it is possible for the resource generating the response to take the request method into consideration. For cases where you can tolerate the extra data, a GET request is much more dependable.


#### The TRACE Method

TRACE is another diagnostic request method. This method allows the client to gain more perspective into any intermediary proxies that lie between the client and the server. As each proxy forwards the TRACE request on route to the destination Web server, it will add itself to the Via header, with the first proxy being responsible for adding the Via header. When the response is given, the content is actually the final request including the Via header.

See Figure 5.1 for an example TRACE transaction. It is initiated with a TRACE request and involves two proxies between the HTTP client and the HTTP server.

**Figure 5.1. HTTP transaction using the TRACE request method.
**

#### The OPTIONS Method

Sometimes it is helpful to simply identify the capabilities of the Web server you want to interact with prior to actually making a request. For this purpose, HTTP provides the OPTIONS request method.

Consider the following request made to my local Apache Web server:

```
OPTIONS * HTTP/1.1 
Host: 127.0.0.1 
```

The asterisk character is not a valid resource, but it allows an OPTIONS request to inquire as to the server's capabilities without the context of any specific resource. The response will detail the capabilities that can be provided for any resource served by the Web server.

```
HTTP/1.1 200 OK 
Date: Tue, 21 May 2002 12:34:56 GMT 
Server: Apache/1.3.22 (Unix)  (Red-Hat/Linux) mod_python/2.7.8 Python/1.5.2 
        mod_ssl/2.8.5 OpenSSL/0.9.6b DAV/1.0.2 PHP/4.0.6 mod_perl/1.26 
        mod_throttle/3.1.2 
Content-Length: 0 
Allow: GET, HEAD, OPTIONS, TRACE 
Connection: close 
```

The Allow header shows the request methods that are always supported. You will probably notice that some methods, such as POST, are missing. If you specify a specific resource to see the server's capabilities in the context of that resource, you will see a drastically different response.

```
OPTIONS / HTTP/1.1 
Host: 127.0.0.1 
```

Now Apache can commit to supporting several more methods, as it knows the resource involved.

```
HTTP/1.1 200 OK 
Date: Tue, 21 May 2002 12:34:56 GMT 
Server: Apache/1.3.22 (Unix)  (Red-Hat/Linux) mod_python/2.7.8 Python/1.5.2 
        mod_ssl/2.8.5 OpenSSL/0.9.6b DAV/1.0.2 PHP/4.0.6 mod_perl/1.26 
        mod_throttle/3.1.2 
Content-Length: 0 
Allow: GET, HEAD, POST, PUT, DELETE, CONNECT, OPTIONS, PATCH, PROPFIND, 
       PROPPATCH, MKCOL, COPY, MOVE, LOCK, UNLOCK, TRACE 

Connection: close 
```

The OPTIONS header can also be used to test the capabilities of an intermediary proxy server. If a proxy receives a request whereby the Max-Forwards request header is 0, the proxy must respond to the request itself. If the request is an OPTIONS request, the proxy must respond accordingly.

#### The CONNECT Method

The CONNECT request method is reserved explicitly for use by intermediary servers to create a tunnel to the destination server. The intermediary, not the HTTP client, issues the CONNECT request to the destination server.

A tunnel is unlike a proxy in that it does not interpret the HTTP requests (for example, to examine the Max-Forwards header or modify the Via header) nor does it cache any of the traffic. From the perspective of both the client and server, a tunnel is transparent. The tunnel remains established as long as the TCP connection remains open. The connection is closed once the destination server closes the connection with the client.

The most common use of the CONNECT method is by a Web client that must use a proxy to request a secure resource using SSL (Secure Sockets Layer) or TLS (Transport Layer Security). The client will tunnel the request through the proxy so that the proxy will simply route the HTTP messages to and from the Web server without trying to examine or interpret them.

>**Note
**
Another use of the CONNECT request method is for an SSL accelerator to establish a tunnel to the Web server. The SSL accelerator will perform the SSL negotiation with the Web client as well as all of the cryptographic processing that is required, thus relieving the Web server from this responsibility and freeing up its resources. The accelerator then forwards decrypted HTTP requests transparently to the Web server over the established tunnel so that the Web server believes itself to simply be communicating to a Web client using standard HTTP (no SSL). In most implementations, the Web client is unaware of this tunneling, much like it would be unaware of a load balancer. For more information, see Chapter 17.


