
#### Content-Length

The Content-Length header simply indicates the length of the content section of the HTTP message in bytes. An example of this header is given in the following HTTP response:

```html
HTTP/1.1 200 OK 
Date: Tue, 21 May 2002 12:34:56 GMT 
Content-Type: text/html 
Content-Length: 102 

<html> 
<head> 
<title>Content-Length Example</title> 
</head> 
<body> 
Content-Length: 102 
</body> 
</html> 
```

In this example, the number of bytes between the first character of the content (the < of <html>) and the final character of the content (the > of </html>) inclusively is 102.

As I briefly mentioned in the discussion of the Connection general header, the Content-Length header is necessary when persistent connections are being used (and content is expected) in order to let the receiving Web agent know when it has received the entire HTTP message. The only exception to this is the use of chunked transfer encoding, which is explained in the previous chapter in the discussion of the Transfer-Encoding general header.

There are several types of HTTP messages in which no content is expected, so the transmission can be considered complete after the first CRLF (carriage return followed by a line feed). Many implementations use a newline alone, as this is a common line-termination sequence in the Unix environment.

Alternatively, anytime the sending Web agent closes the TCP connection (recall that this actually involves communication), it can be assumed that the HTTP message is complete. This is, of course, the technique used when persistent connections are not used.v