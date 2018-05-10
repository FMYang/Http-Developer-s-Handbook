#### Trailer

The Trailer header allows a method that the Web server or Web client can use in HTTP messages to include some HTTP headers after the content. Recall that an HTTP message is constructed as follows:

* Request line (HTTP requests)/Status line (HTTP responses)

* HTTP headers

* Content

The use of the Trailer header allows for the following format to be used:

* Request line (HTTP requests)/Status line (HTTP responses)

* HTTP headers

* Content

* HTTP headers

This allocation is only allowed when a chunked transfer encoding is used, as is specified with the use of the following general header:

`Transfer-Encoding: chunked `

The headers that are included after the content must be specified by name in the Trailer header. The following example shows an HTTP response using this method:

```
HTTP/1.1 200 OK 
Content-Type: text/html 
Transfer-Encoding: chunked 
Trailer: Date 

7f 
<html> 
<head> 
<title>Transfer-Encoding Example</title> 
</head> 
<body> 
<p>Please wait while we complete your transaction ...</p> 
2c 
<p>Transaction complete!</p> 
</body> 
</html> 
0 
Date: Tue, 21 May 2002 12:34:56 GMT 
```

This allocation is helpful for information that cannot be accurately determined until the response has been completely generated. For example, the previous response may have a significant wait between the first and second chunks.

