#### Line Termination

An HTTP message, as discussed earlier, begins with several lines that are terminated by CRLF (carriage return plus a line feed, which is \r\n in many programming languages). The dominance of Unix-based operating systems on the Internet has made the single line feed character (\n) quite common as a termination character as well, although those who strictly adhere to the standard use the proper two-character termination sequence.

The content, if it exists, is always separated from the rest of the HTTP message by an empty line with nothing but the termination character(s). For example, consider this HTTP response:

```
HTTP/1.1 200 OK 
Date: Tue, 21 May 2002 12:34:56 GMT 
Content-Length: 20 
Content-Type: text/html 
Connection: close 

<html> 
HTTP Developer's Handbook 
</html> 
```

The blank line between the Connection header and the <html> tag indicates the separation between the HTTP headers and the message content.

>**Note
**
The content adheres to the format indicated by the Content-Type entity header and is not bound by these rules that govern the HTTP message format.