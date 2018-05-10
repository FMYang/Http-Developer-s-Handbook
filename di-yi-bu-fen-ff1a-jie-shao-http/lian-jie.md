#### Connections

When I speak of a connection in HTTP, I refer to a TCP connection. As illustrated in Figure 3.1, a TCP connection requires three separate messages.

**Figure 3.1. A TCP connection requires three messages.
**

**SYN** and **ACK** are two flags within the TCP segment of a packet. Because TCP is such a common transport layer protocol to be used in conjunction with IP, the combined packet of an IP packet containing a TCP segment is sometimes called a TCP/IP packet, even though it would best be described as a packet within a packet. By this example, you can see that a connection is unlike what you might otherwise expect. After this exchange, both computers simply consider themselves connected. In terms of HTTP, this simply means the server is ready to receive requests from this specific client. There is no real active connection in the traditional sense. It is better described as an understanding between the two computers that they are connected.

An example of this type of connection is a two-way radio. If you and a friend both have two-way radios, you can establish a similar method for ensuring that you are both able to send and receive messages properly. To do this, you can send a message (by talking into the radio) asking to establish a connection. Your friend sends back a confirmation message acknowledging your request and agreeing to the connection. At this point, you feel confident that each of you can both send and receive messages, but your friend cannot be assured of this without knowing whether you received the confirmation. You send back a final message acknowledging the receipt of your friend's confirmation. At this point, you both have confidence in your ability to communicate with these radios. This series of events is very similar to a TCP connection.

> **Note**
>
A single connection can support multiple HTTP transactions. In many cases, multiple HTTP transactions are required to properly render a URL in a Web browser due to images and other associated content.


#### Example HTTP Request

It is probably easiest to get an idea about what HTTP is by looking at a few examples.

Using my Galeon 1.2.0 browser, I type in the URL http://127.0.0.1/ and press Enter. This is actually a request to the Web server running on my local computer (127.0.0.1 is a special IP address called the loopback address). The request that my browser sends is as follows:

```html
GET / HTTP/1.1 
Host: 127.0.0.1 
User-Agent: Mozilla/5.0 Galeon/1.2.0 (X11; Linux i686; U;) Gecko/20020326 
Accept: text/xml,application/xml,application/xhtml+xml,text/html;q=0.9, 
        text/plain;q=0.8,video/x-mng,image/png,image/jpeg,image/gif;q=0.2, 
        text/css,*/*;q=0.1 
        Accept-Language: en 
Accept-Encoding: gzip, deflate, compress;q=0.9 
Accept-Charset: ISO-8859-1, utf-8;q=0.66, *;q=0.66 
Keep-Alive: 300 
Connection: keep-alive
```

#### Example HTTP Response

In this example, my Web server gives the following response:

```html
HTTP/1.1 200 OK 
Date: Tue, 21 May 2002 12:34:56 GMT 
Server: Apache/1.3.22 (Unix)  (Red-Hat/Linux) mod_python/2.7.8 Python/1.5.2 
        mod_ssl/2.8.5 OpenSSL/0.9.6b DAV/1.0.2 PHP/4.0.6 mod_perl/1.26 
        mod_throttle/3.1.2 
Last-Modified: Thu, 01 Nov 2001 20:51:45 GMT 
ETag: "df6b0-b4a-3be1b5e1" 
Accept-Ranges: bytes 
Content-Length: 2890 
Connection: close 
Content-Type: text/html 

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
<head>
<title>Test Page for the Apache Web Server on Red Hat Linux</title>
</head>
<body bgcolor="#ffffff">
(...)
</body>
</html>
```

The majority of the response is HTML (omitted for brevity). Only the first few lines are HTTP. Thus, as intended, HTTP does not have much overhead. Lower-level protocols such as TCP and IP have even less overhead than HTTP, however, due mostly to the fact that HTTP is intentionally readable. This makes it easy to study and comprehend.

#### Example Transaction

A good example transaction to review is a search on Google. Being one of the most popular sites on the Web, most people have interacted with this site at one time or another. When performing a search on HTTP (see Figure 3.2), you enter HTTP into the form field and click the button labeled Google Search.

**Figure 3.2. Searching Google for the term "HTTP".
**
When using my Web browser to perform this search, the following HTTP request is sent when I click the button:

```
GET /search?hl=en&q=HTTP&btnG=Google+Search HTTP/1.1 
Host: www.google.com 
User-Agent: Mozilla/5.0 Galeon/1.2.0 (X11; Linux i686; U;) Gecko/20020326 
Accept: text/xml,application/xml,application/xhtml+xml,text/html;q=0.9, 
        text/plain;q=0.8,video/x-mng,image/png,image/jpeg,image/gif;q=0.2, 
        text/css,*/*;q=0.1 
Accept-Language: en 
Accept-Encoding: gzip, deflate, compress;q=0.9 
Accept-Charset: ISO-8859-1, utf-8;q=0.66, *;q=0.66 
Keep-Alive: 300 
Connection: keep-alive 
```

Google's Web site responds:

```
HTTP/1.1 200 OK 
Server: GWS/2.0 
Date: Tue, 21 May 2002 12:34:56 GMT 
Transfer-Encoding: chunked 
Content-Encoding: gzip 
Content-Type: text/html 
Cache-control: private 
Set-Cookie: PREF=ID=58c005a7065c0996:TM=1021283456:LM=1021283456:S=OLJcXi3RhSE; 
            domain=.google.com; path=/; expires=Sun, 17-Jan-2038 19:14:07 GMT 
```

(Web content compressed with gzip) 

Of interest in this response is that the Web content is of a format that cannot be printed, binary. Because my browser specified in its request that it accepts gzip (GNU zip) encoding, Google chose to encode the response with gzip. This is a popular compression algorithm that allows for a quicker transfer due to the smaller size of the HTTP response. My browser decompresses the content in order to reveal the HTML it needs to render the Web page (the results of my search).



