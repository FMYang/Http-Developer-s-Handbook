#### Compression

A straightforward method to improve performance is by decreasing the size of the HTTP response. Although a minimalist approach to the markup used to display the content can be very helpful in this regard, the use of compression is also very beneficial. Although only the content is compressed, it is often the largest part of an HTTP response.

When a client declares support for specific types of encoding in the HTTP request with the Accept-Encoding header, the Web server can send the content of the HTTP response in any of the supported formats, and the Web client will decode the content once it is received. The Accept-Encoding header specifies which type of content encoding the browser has the capability to decode.

The Content-Encoding header in the HTTP response indicates any special encodings that have been performed on the resource. If the resource has been encoded, the encoding only affects the content section of the HTTP message; the encoding does not affect the content type in any way. The content type is indicated in the Content-Type entity header as always.

There are three main content encodings used in practice by most modern Web agents:

* gzip— This indicates that the resource has been compressed using gzip (GNU zip), arguably the most popular compression algorithm in computing today.

* deflate— This indicates that a zlib format has been applied to the resource, which was then compressed with deflate. These techniques are defined in RFC 1950 and RFC 1951, respectively.

* compress— The compress encoding uses the compression algorithm made popular by the Unix program of the same name.

An example of using PHP to automatically use gzip encoding for Web clients that support it is the following:

```
<? 
ob_start("ob_gzhandler"); 
?> 
<html> 
<body> 
<p> 
If your browser supports gzip encoding, 
this page will be compressed in transit. 
</p> 
</body> 
</html> 
```

In order to demonstrate the benefit of this technique, consider the following PHP script:

```
<? 
ob_start("ob_gzhandler"); 
for ($i = 0; $i < 1024; $i++) 
{ 
   echo "X"; 
} 
?> 
```

This script will create a resource that is exactly 1024 bytes in size. When this resource is requested with an HTTP request that includes an Accept-Encoding: gzip header, the size is decreased to 35 bytes, only 3.4% of the size of the original. Although this technique may not always yield results as staggering as this, allowing for content encoding will typically help to lessen the load on your network as well as improve the user experience.v