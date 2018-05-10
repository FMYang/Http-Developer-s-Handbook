#### Chunked Transfers

In order to enhance the user experience, chunked transfer encoding can be used to allow the Web browser to begin rendering content prior to the entire content being generated and sent. A Web server indicates a chunked response with the following header:

`Transfer-Encoding: chunked `

A chunked transfer encoding allows a format in which the content implicitly declares the length of pieces of the response within the content. Consider the following example PHP script:

```
<p>This is an example of a time-delayed list:</p> 
<ul> 
<? 
flush(); 
sleep(3); 
echo "<li>List item one</li>"; 
flush(); 
sleep(1); 
echo "<li>List item two</li>"; 
flush(); 
sleep(1); 
echo "<li>List item three</li>"; 
flush(); 
sleep(1); 
echo "<li>List item four</li>"; 
flush(); 
sleep(1); 
echo "<li>List item five</li>"; 
flush(); 
sleep(1); 
?> 
</ul> 
```

When this resource is requested, the following HTTP response is generated:

```
HTTP/1.1 200 OK 
Date: Tue, 21 May 2002 12:34:56 GMT 
Transfer-Encoding: chunked 
Content-Type: text/html 

36 
<p>This is an example of a time-delayed list:</p> 
<ul> 
16 
<li>List item one</li> 
16 
<li>List item two</li> 
18 
<li>List item three</li> 
17 
<li>List item four</li> 
17 
<li>List item five</li> 
5 
</ul> 
0 
```

Notice the absence of a Content-Length entity header. The length of the content is included within the content section of the HTTP response preceding each chunk. As the Web client reads the data, it can begin rendering these chunks.

Just as with all HTTP responses, the content begins after a blank line is sent, separating the headers. When chunked transfer encoding is used, the first line of the content specifies the length of the first chunk in hexadecimal. In the previous example, this is 36 (54 in decimal). Thus, the Web client can read for 54 bytes and be finished reading the first chunk. In some cases, this may be all that the Web server has been able to generate at this point. The 54 bytes read are as follows:

`<p>This is an example of a time-delayed list:</p> <ul> `

The Web client will then read the next chunk's length on the following line. This same process continues until a 0 is read as the value of the next chunk. This indicates that there is no more content (the next chunk is of 0 length) .

#### Summary

There are many characteristics of the HTTP protocol that help to improve the performance of your Web applications. Although some of these characteristics do not require specific action on your part, it is still beneficial to study these characteristics in order to give yourself a more accurate perspective of your environment as you make decisions based on performance.

As with most of the content in this book, the goal is to provide you with knowledge that helps you make better decisions regarding your own applications. Because there are so many factors affecting the performance of your application, it can be a daunting task to resolve bottlenecks and other performance problems once the application is constructed and deployed. Unfortunately, this situation arises quite often, as performance is not something taken into consideration during the initial design. By incorporating many of the considerations presented here into your initial design, you can avoid frustrating performance problems that might arise later in the development process or after deployment.

In the following chapter, I briefly discuss some of the protocols that caching proxies use to make decisions regarding caching. Although this information lies outside of the scope of HTTP, and likely outside of the scope of your professional development, it will help further explain and elaborate upon some of the topics mentioned in this chapter regarding proxy behavior.