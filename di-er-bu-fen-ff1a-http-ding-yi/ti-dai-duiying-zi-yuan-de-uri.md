#### Content-Location

The Content-Location header is used in cases in which the URL of the resource being returned differs from the URL being requested for some reason. This situation may occur when the preferences of the Web client being communicated via the HTTP headers indicates a preference for a different resource than the specific one being requested. Because some resources are maintained in various languages, an Accept-Language request header could prompt the Web server to return an alternative resource in the requested language. Consider the following HTTP request made to httpd.apache.org:

```
HEAD /docs/index.html HTTP/1.1 
Host: httpd.apache.org 
Accept-Language: fr 
```

Because the HTTP request indicates a language preference of fr, the Web server returns an alternative version of the resource that is available in the requested language. Because the actual resource returned differs from the requested resource, this is indicated in the Content-Location header:

```
HTTP/1.1 200 OK 
Date: Tue, 21 May 2002 12:34:56 GMT 
Server: Apache (Unix) 
Content-Location: index.html.fr 
Content-Type: text/html 
Content-Language: fr 
```