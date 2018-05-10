#### Secure HTTP Responses

Upon receiving a Secure HTTP request, the Web server will unwrap the HTTP request, generate its HTTP response, and use a similar technique to protect the response. The following example shows a possible reply to the previous Secure HTTP request:

```
Secure-HTTP/1.4 200 OK 
Content-Type: message/http 
Content-Privacy-Domain: CMS 

(content is encrypted) 
```

Here is the response shown in decrypted format for your convenience:

```
Secure-HTTP/1.4 200 OK 
Content-Type: message/http 
Content-Privacy-Domain: CMS 

HTTP/1.1 200 OK 
Date: Tue, 21 May 2002 12:34:56 GMT 
Security-Scheme: S-HTTP/1.4 
Content-Type: text/html 
Content-Length: 35 

<html> 
Secure HTTP Response 
</html> 
```

The HTTP message in this example is as follows:

```
HTTP/1.1 200 OK 
Date: Tue, 21 May 2002 12:34:56 GMT 
Content-Type: text/html 
Content-Length: 35 

<html> 
Secure HTTP Response 
</html> 
```

The message format used is given in the Content-Privacy Domain header, just as in the Secure HTTP request.

Because the HTTP message is contained within the Secure HTTP message, it is possible that an error exists in the HTTP transaction, while the Secure HTTP transaction is successful. For example, a 200 OK Secure HTTP response can contain a 404 Not Found HTTP response.