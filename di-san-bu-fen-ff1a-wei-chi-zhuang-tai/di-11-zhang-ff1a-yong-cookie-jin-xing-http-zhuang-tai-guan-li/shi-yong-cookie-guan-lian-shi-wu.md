#### Using Cookies to Associate Transactions

If each HTTP request includes an identifier that is unique to the Web client sending the request, association between subsequent requests from the same Web client is intuitive. This is exactly what cookies are intended to achieve.

Although cookies are most often described in conversation as if they are entities (for example, "a Web server sends you a cookie"), they are much easier to understand at a functional level if you consider them an extension of the HTTP protocol, which is actually more correct. Cookies can be defined as the addition of two HTTP headers:

* Set-Cookie response header

* Cookie request header

>**Note
**
Cookies are defined in RFC 2109, "HTTP State Management Mechanism." Although a newer specification (RFC 2965) claims to obsolete RFC 2109, this is not the case in practice. In addition, neither of these specifications perfectly matches industry support. This chapter focuses on cookies from a developer's perspective, where compatibility with existing Web agents is most important.


The implementation of these two headers is illustrated in Figure 11.3.

**Figure 11.3. A Web server and Web client implement cookies.
**

When people refer to a Web server setting a cookie, it would be more accurate (although admittedly more cumbersome) to describe this scenario as the Web server requesting that a cookie be sent by the Web client in future requests, because the Web server simply includes a Set-Cookie response header in its response. Whether the value of this cookie is sent in subsequent HTTP requests via the Cookie request header is entirely up to the Web client.

This characteristic is the most common source of confusion with regard to cookies. A common question seen on mailing lists and discussion forums for Web developers is how to test whether the client is accepting cookies, and many people do not understand the answer. As is evident in Figure 11.3, it is impossible to determine whether the client accepted the cookie until the second request is sent (step 3 in the figure). If the cookie is included in the second request, the client accepted it. If not, the client rejected it.

Some developers choose to force the issue of determining whether the client accepts cookies by redirecting the client to a second URL upon entrance. Thus, by the time the user sees the first page, the application has already determined whether cookies can be used. An example of this technique is the following PHP code:

```
<? 
header("Set-Cookie: accept_cookies=yes"); 
header("Location: http://127.0.0.1/step_two.php"); 
?> 
```

The script step_two.php would then be able to determine whether the client accepted the accept_cookies cookie. An example of how the step_two.php might check is the following:

```
<? 
if (isset($_COOKIE["accept_cookies"])) 
{ 
     echo "<p>The cookie was accepted</p>"; 
} 
else 
{ 
     echo "<p>The cookie was rejected</p>"; 
} 
?> 
```

>**Note
**
Because of past problems with protocol-level redirection combined with cookies, many developers opt to redirect with a <meta http-equiv="refresh"> tag instead of using the technique given in the previous example. You can also use the HTTP header Refresh to achieve the same results.


In practice, many Web browsers are configured to always accept cookies by default (where acceptance refers to compliance with the request), so many users are unaware that this behavior is optional. Of course, the reasoning behind such default behavior in browsers is for functionality, because many Web sites depend on cookies to preserve the identity as well as the state of the various users, being otherwise inoperable.

In the next chapter, I show you how to avoid complete dependence on cookies for your state management.

