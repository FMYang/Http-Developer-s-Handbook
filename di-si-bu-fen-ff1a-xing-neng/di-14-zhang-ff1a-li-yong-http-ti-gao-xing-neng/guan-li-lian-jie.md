#### Managing Connections

The default behavior of an HTTP transaction, prior to HTTP/1.1, was that a TCP connection (see Figure 14.3 for an illustration of a TCP connection) would be established, the HTTP request and response would be exchanged, and the TCP connection would be closed. Consider that most Web pages consist of HTML and a handful of images (all of which must be requested separately), and the inefficiency of this method becomes apparent. One way around this was for the Web server and Web client to agree to use a persistent connection for several consecutive transactions.

**Figure 14.3. A TCP connection requires three messages.
**

#### Connection General Header

As mentioned in Chapter 7, one of the most important distinctions between HTTP/1.0 and HTTP/1.1 is how connections are treated. Although both versions support persistent connections, with HTTP/1.0, persistent connections are not the default behavior, so you must use a Connection: Keep-Alive header to request a persistent connection.

With HTTP/1.1, persistent connections are the default behavior, so the Web server will not close the connection after sending the HTTP response unless the client intends to close the connection after receiving it. In this case, the client will include the following header in the HTTP request:

`Connection: close `

Alternatively, the server can close the connection upon sending the HTTP response, although it should be polite and include the same header as shown previously, so that the Web client expects this action.

In general, it is best to use persistent connections (sometimes called keep-alives) whenever possible. When developing for an HTTP/1.1 Web server, this will most likely require no special configuration on your part.

#### Control Flow

It is important to visualize the necessary transactions required to operate your Web application. When possible, you should fulfill a request with an appropriate response. Although this may seem intuitive, consider that the use of the Location header (which has become quite popular) requires the client to make an additional request to receive the desired response. Although this technique can be useful in certain situations, you should avoid it if there is no clear advantage. A properly constructed application should allow you to easily make decisions as to which actions to take based on a client's request. For some good programming practices, see Chapter 22, "Programming Practices."

There are some beneficial techniques that require very little effort. A common error is a link that points to a directory but omits the trailing slash. For example, consider the following HTML:

`<a href="httphandbook.org/dir">Click Here</a> `

If dir is a directory, a series of transactions similar to the following will transpire when this link is clicked:

```
GET /dir HTTP/1.1 
Host: httphandbook.org 

HTTP/1.1 301 Moved Permanently 
Date: Tue, 21 May 2002 12:34:56 GMT 
Server: Apache/1.3.26 (Unix) 
Location: http://httphandbook.org/dir/ 
Transfer-Encoding: chunked 
Content-Type: text/html; charset=iso-8859-1 

13e 
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN"> 
<html> 
<head><title>301 Moved Permanently</title></head> 
<body> 
<h1>Moved Permanently</h1> 
<p>The document has moved <a href="http://httphandbook.org/dir/">here</a>.</p> 
<hr> 
<address>Apache/1.3.26 Server at httphandbook.org Port 80</address> 
</body> 
</html> 
0 

GET /dir/ HTTP/1.1 
Host: httphandbook.org 

HTTP/1.1 200 OK 
Date: Tue, 21 May 2002 12:34:56 GMT 
Server: Apache/1.3.26 (Unix) 
Transfer-Encoding: chunked 
Content-Type: text/html; charset=iso-8859-1 

a9 
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN"> 
<html> 
<head><title>http://httphandbook.org/dir/</title></head> 
<body> 
<pThis is a directory</p> 
</body> 
</html> 
0 
```

If the HTML link includes the proper URL, a single transaction will transpire rather than two. This attention to detail can make a significant difference when there are many such links and many users.

#### Pipelining

With HTTP/1.1, a Web client can issue multiple requests prior to receiving a response. Consider an example Web page consisting of an HTML document and three images. The transactions can occur as illustrated in Figure 14.4.

**Figure 14.4. A Web client pipelines its requests.
**

In this example, the Web client must wait for the response to its initial request because this is the HTML document. Once it receives this document, it can determine that it needs three more resources in order to properly render the page. At this point, it can send these three requests without having to wait for each response. Thus, not only do these transactions occur over a single persistent connection, but the wait involved between each request and subsequent response is also lessened.