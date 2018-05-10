#### Response Headers

There are a few HTTP headers that are specific to responses. They provide information related only to the nature of the response and not the content.

#### Accept-Ranges

The Accept-Ranges header is the only HTTP header in the Accept family that is not a request header. This header indicates to the Web client that the server has the capability to handle range requests. A Web client can make a range request using the Range request header.

There are only two valid formats for the Accept-Ranges header that are allowed according to the definition:

`Accept-Ranges: bytes `
A`ccept-Ranges: none `

These basically indicate that the Web server does and does not accept range requests, respectively.

It is not necessary for a Web server to indicate that it can accept range requests in this manner. In fact, a Web client is free to issue a range request regardless of whether it is certain that the Web server can issue them. A Web server that cannot properly respond to a range request will respond with the entire content.



#### Age

The Age header indicates an estimation, in number of seconds, of the age of the resource being requested since it was last requested from the Web server of origin. In cases where the calculation of the age results in an overflow condition, the value 2^31 (2147483648) is used.

The original Age header will be generated from a caching proxy that is using its own copy of the resource in the response. However, intermediate proxies recalculate the age based on a number of factors. In all calculations made by proxies, the result will err on the side of caution so that a stale response (as determined by the Cache-Control general header) will not be returned.

Because network delays can result in improperly low ages being calculated, the age is recalculated at each step in the return path based on the estimated time between hops. Rather than depending on the calculations made by other intermediate proxies, the age will usually be calculated as a difference between the proxy's current date and the Date general header included in the HTTP response. If unsynchronized clocks make this calculation resulting in a negative difference, an age of 0 is used.

To further eliminate discrepancies in calculation, the proxy calculating the age will take the following steps:


  **1**. Use the maximum age between its own calculation (described in the previous paragraph) and the value in the Age response header.

  **2**. Add the time between its request and the corresponding response.

  **3**. Add the time used in the calculation.

This process is illustrated in Figure 6.1.

**Figure 6.1. Transaction illustrating the Age Response Header
**
#### Authentication-Info

The Authentication-Info header represents the final step in HTTP digest authentication, a topic that is covered in great detail in Chapter 17. This header is an important part of the HTTP response that includes the protected resource. Among other things, this header allows the client to verify the identity of the Web server.

An Authentication-Info header contains a comma-delimited list of parameters, and each parameter has an associated value. An example of an HTTP response that includes the Authentication-Info header is as follows:

```
HTTP/1.1 200 OK 
Authentication-Info: qop="auth-int", rspauth="5913ebca817739aebd2655bcfb952d52", 
                     cnonce="f5e2d7c0b6a7f2e3d2c4b5a4f7e4d8c8b7a", nc="00000001" 
```

The parameters allowed in the Authentication-Info header are as follows:

* cnonce— This is the value of the cnonce parameter included in the client's previous request within the Authorization request header.

* nc— This is the value of the nc parameter included in the client's previous request within the Authorization request header.

* nextnonce— This is the nonce that the Web server requests and that the client uses for authentication in future HTTP requests.

* qop— This is the value of the qop parameter included in the client's previous request within the Authorization request header.

* rspauth— This is the Web server's calculated digest that can be used to authenticate the server's identity.

Related material can be found in Chapter 17.

#### Content-Disposition

Although not an official part of the HTTP definition, the Content-Disposition header is sometimes used to indicate a suggested filename when the Web server wants to prompt the user to save the file. Its use in this manner is borrowed from the definition given in RFC 1806, which defines its use in email message attachments.

An example of this header is as follows:

Content-Disposition: attachment; filename="example.pdf" 
This has been known to resolve problems with certain Web browsers that incorrectly handle the Content-Type entity header.

#### ETag

The ETag header provides a unique value for a specific version of a resource so that it can be used to identify a resource in caching decisions. The same resource with an identical ETag as a previous request can be safely considered to be identical.

An entity tag normally takes the following form:

ETag: "abcd1234" 
When used in this way, which is the most common, the ETag is guaranteed to be unique for every version of the resource that has ever existed. To allude to the strength of identity inherent in this type of entity tag, it is called a strong entity tag.

A weak entity tag is less common and takes the following form:

ETag: W/"abcd1234" 
This type of entity tag is not guaranteed to be unique for a specific version of a resource, but it is intended to be unique when there exists significant differences in versions.

For example, if a resource were modified in such a way that only an extra newline is appended (added to the end of the file), a strong entity tag would indicate a difference in versions, whereas a weak entity tag would not.

>**Note
**
Related material can be found in the description of the If-Match request header in Chapter 5.


#### Location

The Location header provides a URL that the Web client is intended to use in order to receive the resource being requested. This technique is often referred to as redirection or protocol-level redirection (to distinguish it from using HTML to achieve redirection).

An example use of the Location header is as follows:

`Location: http://httphandbook.org/ `

>**Note
**
It is important to remember that the value must be an absolute URL. Although most Web browsers will tolerate relative URLs as well, this violates the HTTP specification. It is a significant risk to develop applications that deliberately violate specifications such as this, because Web client support cannot be guaranteed.


There are various reasons why a Location header might be included in a response. Most of the 300-range status codes require the use of a Location header, so reviewing those status codes is recommended.

In many server-side scripting languages, the developer can specify the use of the Location header. These decisions typically change the status code to a 302 Found and, in many cases, eliminate many other headers that were included, such as the Set-Cookie response header. This is a common pitfall for Web developers.

It should be noted that a Location header can also be included in an HTTP response with a status code of 201 Created in order to identify the URL of the created resource.

>**Note
**
Related material can be found in the description of the 201 Created status code as well as the 300-range status codes earlier in this chapter.


#### Proxy-Authenticate

The Proxy-Authenticate header is analogous to the WWW-Authenticate response header, except that it is used by proxies to authenticate themselves with the Web server after it lets them know that a resource requires authentication via the 407 Proxy Authentication Required status code in the HTTP response.

>**Note
**
Related material can be found in the description of the Proxy-Authorization request header in Chapters 5 and 17.


#### Refresh

Web browsers that support the Refresh header interpret it exactly as its HTML equivalent:

`<meta http-equiv="refresh" content="3; url=http://httphandbook.org/"> `

The value of the Refresh header uses the same syntax as the content attribute in the previous example:

`Refresh: 3; url=http://httphandbook.org/ `

In this example, a Web browser that supports the Refresh header will refresh the current page after three seconds to display the page located at http://httphandbook.org/.

#### Retry-After

The Retry-After header is intended to be included in an HTTP response with a status code of 503 Service Unavailable or any of the 300-range (redirection) status codes. It indicates to the Web client the minimum time to wait prior to issuing its next request. In the case of the service being unavailable, its use suggests that it will be unavailable until after the time specified.

There are two formats the Web server can use in a Retry-After header. It can include a properly formatted date, or it can give the value as number of seconds:

`Retry-After: Tue, 21 May 2002 12:34:56 GMT `
`Retry-After: 600 `

####Server

The Server header relays information to the Web client about the software being used by the Web server. A typical Server header looks something like this:

`Server: Apache/1.3.22 (Unix)  (Red-Hat/Linux) mod_python/2.7.8 Python/1.5.2 mod_ssl/` 
`2.8.5 OpenSSL/0.9.6b DAV/1.0.2 PHP/4.0.6 mod_perl/1.26 mod_throttle/3.1.2 `

>**Note
**
Although the formatting in this book makes this example appear as if it takes up more than one line, it is in fact a single line of text.


Because an honest Server header can reveal a great deal of information about the Web server software, thus allowing known vulnerabilities in that software to pose a serious security risk, many people will rebuild their Web server software so that it purposely provides a misleading Server header. Although the definition does not specifically support such practices, it does suggest making the inclusion of this header optional.

#### Set-Cookie

The Set-Cookie header is not included in the HTTP/1.1 specification. It is described in a separate specification, RFC 2109. This header provides a method for the Web server to ask the client to store data locally that it will include in a Cookie request header in future requests, based on the access information.

The access information is included in the Set-Cookie header. Consider the following example:

```
Set-Cookie: fname=chris; domain=.httphandbook.org; path=/; expires=Tue, 21 May 2002 12:34:
graphics/ccc.gif56 GMT; secure 
```

In this example, there are four name/value pairs and one name with no value, all separated by semicolons:

* fname=chris

* domain=.httphandbook.org

* path=/

* expires=Tue, 21 May 2002 12:34:56 GMT

* secure

The first name/value pair is the actual name and value of the cookie. All others indicate various types of access restriction implemented by the Web client.

The domain attribute specifies the domains that this cookie is restricted to. The syntax is provided in such a way as to allow for all domains that end with the characters identified in this attribute. For example, www.httphandbook.org matches the domain given in this example.

The path attribute indicates the required path. Many cookies set this attribute to / (document root) so that any resource within the Web site has access to the cookie. If you picture the hierarchy of a Web site's directory structure, this attribute specifies the top-most directory of the directory tree that is allowed access to this cookie. Perhaps a more helpful way to imagine the use of this attribute is that the more information you provide, the more restricted the access becomes.

The expires attribute informs the Web client when it should consider the cookie to be expired and thus no longer include it in HTTP requests. The time is calculated based on the client's time, which can pose a challenge to Web developers who want to set the cookie with server-side logic. Although the time is given in GMT, discrepancies in the synchronization of the clocks can result in session logic failures. The lack of this attribute indicates that it should only persist while the current Web client is running. Cookies that expire at the end of the current browsing session are referred to as session cookies, whereas those that persist from session to session are referred to as persistent cookies.

It is important to note that having multiple windows of a browser open generally uses the same parent process so that closing a single browser will not eliminate session cookies.

The secure attribute, when present, indicates that the cookie should only be sent with HTTP requests sent over a secure connection such as SSL.

>**Note
**
Related material can be found in the description of the Cookie request header in Chapter 5 and Chapter 11, "HTTP State Management with Cookies."


#### Vary

The Vary header indicates to an intermediate proxy the request headers for which the proxy is not allowed to tolerate fluctuation. Multiple values are delimited by commas as shown in this example:

`Vary: Accept-Language, User-Agent `

In this example, the resource might be different for different user agents (perhaps some dynamically generated client-side script that is browser-dependent) and for different language capabilities, such as when the Web server will use a different language if the Web client can accept it. Thus, the proxy may consider its cache of the resource valid so long as these headers do not fluctuate; otherwise, it should obtain a fresh copy.

A value in the Vary header of asterisk indicates that other factors affect whether the cache is still valid, so comparing headers indicates nothing useful in terms of determining whether to use the cached resource.

#### WWW-Authenticate

The WWW-Authenticate header is required for HTTP responses with a status code of 401 Unauthorized. This header gives the Web client the information it needs to properly identify itself on the next request so that it can access the resource being requested.

>**Note
**
Related material can be found in Chapter 17.


