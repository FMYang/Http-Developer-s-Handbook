#### Basic Authentication

A Web client cannot predict whether a particular resource is protected with basic authentication prior to requesting it. For this reason, the initial request for a protected resource is no different than any other request. The response returned by the server is the first indication to the client that the resource is protected. Thus, the series of events in basic authentication consists of two complete HTTP transactions. The steps involved are as follows:

1.The Web client requests a resource that is protected by basic authentication from the Web server.

2.The Web server returns an HTTP response with a 401 Unauthorized status code.

3.The Web client prompts the user for a username and password, and then makes a second request that includes the username and password in the Authorization header.

4.The Web server returns the requested resource.

These steps are illustrated in Figure 17.1.

**Figure 17.1. Basic authentication involves two HTTP transactions.
**

Because the first request (step 1 in Figure 17.1) does not provide the proper authorization credentials (via the Authorization header), the Web server's HTTP response (step 2 in Figure 17.1) will have a status code of 401 Unauthorized. An example of this transaction is as follows:

```
GET / HTTP/1.1 
Host: httphandbook.org 

HTTP/1.1 401 Unauthorized 
WWW-Authenticate: Basic realm="HTTP Developer's Handbook" 
A 401 Unauthorized response can be generated from a PHP script manually as follows:

if (!isset($_SERVER['PHP_AUTH_USER'])) 
{ 
   header('HTTP/1.0 401 Unauthorized'); 
   header('WWW-Authenticate: Basic realm="HTTP Developer's Handbook"'); 
} 
```

Web browsers will then prompt the user for a username and password, referring to the realm given in the WWW-Authenticate header of the HTTP response. My browser displays the prompt shown in Figure 17.2 when it receives the HTTP response given in the previous example.

**Figure 17.2. HTTP authentication prompt.
**
Once the username and password are obtained from the user, the Web client will then send a second request (step 3 in Figure 17.1) similar to the following:

```
GET / HTTP/1.1 
Host: httphandbook.org 
Authorization: Basic bXluYW1lOm15cGFzcw== 
```

The value of the Authorization header is a Base 64-encoded value of the username and password separated by a colon. For example, if you take the Base 64-decoded value of bXluYW1lOm15cGFzcw==, you will get the following result:

`myname:mypass `

This characteristic lies at the heart of the insecurity of basic authentication. Although a casual glance at the value of the Authorization header may seem to indicate that the username and password are encrypted, this is not the case. By encoding the username and password with Base-64 encoding, the only advantage is that more characters are allowed in the username and password than printable characters alone. The only exception is that the username may not contain a colon, as this character is used to separate the two elements (the password, of course, does not have this restriction).

Given the public nature of the Internet, exposing the username and password is a risk that may not be worth taking. However, if you must use basic authentication for some reason (for example, because of its more consistent support in Web browsers), you can also use SSL to mitigate the risk of exposure.

Even when unauthorized access is an acceptable risk, employing basic authentication without another form of protection can expose your users to other risks. Many people use similar passwords for multiple types of accounts, so the compromise of a password can be very damaging. If unauthorized access is not a concern, no authentication should be used.

>**Note
**
Many Web browsers will store the username and password as provided by the user and continue to submit the Authorization header to protected URLs, saving the expense of the 401 Unauthorized response and second request. Because of this characteristic, it may appear as if a user remains logged in. However, this is not the case and is only a convenient characteristic of the Web browser.

Unfortunately, this characteristic can provide more challenges to Web developers, such as how to log a user out or how to debug an unauthorized response when the user is not prompted for a username and password. In the latter case, the Web browser can be restarted in order to dispose of the cached username and password, and there is generally no other solution other than to change the realm indicated by the Web server (which is not always a good option). Because most browsers store the authorization information according to URL and realm, most Web developers implement techniques that take advantage of this in order to simulate a logout, such as fooling the Web browser into thinking the realm has changed.


If you are using the Apache Web server, you can find more information about implementing HTTP authentication at http://httpd.apache.org/docs-2.0/howto/auth.html. You can find specific information regarding basic authentication at http://httpd.apache.org/docs-2.0/mod/mod_auth.html.

In order to address the security concerns of basic authentication, digest authentication includes many improvements and is a more secure alternative. Unfortunately, consistent and proper support for digest authentication is not as common in Web browsers as it should be. As such, many Web developers develop user authentication into the Web application itself rather than relying on either type of HTTP authentication.