#### Transfer-Encoding

Without the use of persistent connections, a Web client can be assured that the Web server is finished sending the HTTP response once it closes the TCP connection. When using persistent connections, however, it becomes necessary for the Web server to indicate the length of the content being sent so that the Web client knows when the transmission is complete.

The only drawback to having to specify the length of the content is when that content is dynamically generated. Instead of being able to send the content as it is generated, the Web server must wait for all of the content to be generated before it can adequately calculate and report its length. Thus, the use of persistent connections requires that the Web server wait until the entire content is prepared before it can begin responding.

A solution to this removal of flexibility is the Transfer-Encoding header. Because persistent connections became the default behavior in HTTP/1.1, this header was introduced. The specification allows for some flexibility in the values that can be assigned to this header, but only one form is used in practice:

Transfer-Encoding: chunked 
This specific value addresses the problem just discussed. A chunked transfer encoding allows a format in which the content implicitly declares the length of pieces of the response. Consider the following example HTTP response:

```
HTTP/1.1 200 OK 
Date: Tue, 21 May 2002 12:34:56 GMT 
Content-Type: text/html 
Transfer-Encoding: chunked 

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
```

In this example, notice the absence of a Content-Length entity header. The length of the content is included within the content section of the HTTP response. The format of the chunked transfer encoding is that each piece of the content begins with a hexadecimal value of the length of the next chunk on a line by itself.

For example, in the previous example, the first line indicates a chunk consisting of 7f (127) bytes. Immediately after the 127 bytes begins the next chunk. Just as before, the length is given on a line by itself. Thus, the next chunk is 2c (44) bytes. This continues until the Web server sends an empty chunk, identifying it with a 0 on a line by itself, signaling the end of the HTTP response.

>**Note
**
Hexadecimal is a numeric format that uses the characters 0-9 and a-f, where the latter set represents decimal values 10-15. The format uses a base of 16 (using the 16 characters 0-f) rather than the more common decimal notation, which uses a base of 10 (using characters 0-9 only).

Thus, hexadecimal 0f represents decimal 15. As with decimal, a leading 0 has no value, thus 0000000f is also equivalent to 15. Also, just as decimal represents numbers in powers of 10 (127 = 1*100 + 2*10 + 7*1), hexadecimal represents numbers in powers of 16 (7f = 7*16 + 6*1 and 127 = 1*256 + 2*16 + 7*1).

An understanding of hexadecimal can also be helpful in interpreting RGB (short for red, green, and blue) values that are common in HTML colors. For example, #ff0000 indicates a value of ff for red, 00 for green, and 00 for blue. Thus, this would indicate the highest possible value for red and the lowest possible value for both green and blue, which yields the purest red possible.


The previous example is also an attempt at illustrating how this capability can be very helpful to Web developers. When a particular transaction might take a bit of time to process, it can be helpful to give feedback to the users prior to completing the transaction. Using the previous example, the users would see:

**Please wait while we complete your transaction ... 
**

Though the browser would still appear busy, nothing additional would appear until the transaction had completed. Then the following message would be displayed just below the previous one:

**Transaction complete! **

Thus, the users would not be redirected to another URL, adding unnecessary HTTP traffic. Rather, the page would be displayed as it became available. This could continue for as many chunks as necessary. Many sites that have a great deal of content, such as http://slashdot.org/, also make use of this method so that the browser can begin to render to Web page prior to the entire content being transmitted.

