#### Controlling Caching with HTTP

One of the most important areas of performance a Web developer should focus on is controlling the caching behavior with HTTP. It is very rare that the same caching rules will be appropriate for all pages within a Web application. Unfortunately, this approach is prevalent. A more proper use of HTTP headers is all that you need to benefit from the opportunity you are given by the HTTP protocol to control the caching behavior in both proxies as well as Web clients.

#### Cache-Control General Header

Of the HTTP headers defined in HTTP/1.1, one of the ones deserving attention is Cache-Control. Although Cache-Control is defined in Chapter 7, "General Headers," I will explain it again here.

>**Note
**
Because most Web development focuses on constructing HTTP responses, valid directives for the Cache-Control header to be included in HTTP requests are not covered here. See Chapter 7 if you need this information.


The Cache-Control general header indicates the behavior expected for any caching system, usually a Web proxy or the Web client itself. An example use of the header is as follows:

`Cache-Control: max-age=600 `

Multiple directives can be specified with a single Cache-Control header as a comma-delimited list. This is illustrated by the following example:

`Cache-Control: max-age=600, no-cache="Set-Cookie"`
 
In this example, the value given to the no-cache directive indicates an HTTP header that should not be cached. Thus, although caching is allowed, the Set-Cookie header should not be included in the cached responses returned to future Web clients. Of course, the original requesting client will still receive the entire response, including this header, so this technique cannot be used to filter HTTP headers. It is only helpful for excluding potentially sensitive information from future responses that use the cached copy.

An important distinction to be made with regard to the Cache-Control header is the distinction between no-cache and no-store. The no-cache directive does not prevent a caching system from caching the resource. Rather, it requires the caching system to always verify that the resource is not stale. In general, the caching system will use an HTTP metric such as the last modified date (Last-Modified) or the entity tag (ETag) to determine whether the resource has changed. If it has not, returning the cached copy is appropriate.

The no-store directive requires that a caching system not store the resource. This requirement extends to all caching systems, including the Web client. The excessive use of this particular directive is an extremely common problem. This directive effectively disables caching. Thus, it should only be used on pages where caching is unwanted.

Because the no-cache directive can include specific HTTP headers to not cache, the only personalized information that should be considered is the content of an HTTP response. If the content contains personal or sensitive information, disabling caching for that particular response is appropriate. However, if the information contained in some of the HTTP headers is personal or sensitive, the no-cache directive can be used to disable caching for those headers only. In many cases, a dynamic application will contain many pages that are identical for many users.

For example, although a user might be required to log in prior to viewing a specific page, it does not necessarily warrant disabling caching for that page. If all users who log in receive the same welcome screen, it might be appropriate to allow this screen to be cached so long as the information displayed to authenticated users is not considered sensitive.

>**Note
**
Most versions of Microsoft Internet Explorer ignore the no-store directive of the Cache-Control header, treating it the same as no-cache instead. Thus, using no-cache is often more appropriate simply so that Web clients will behave more consistently with one another.

For example, Netscape Navigator and the Mozilla Web browser often require a request to be re-sent, sometimes requiring the user to agree to repost form data (See Figure 14.2), for actions such as using the Back button, printing the page, or viewing the source. This is because these Web clients interpret no-store literally and do not have a copy of the resource that can be used, even for these simple tasks.

**Figure 14.2. A browser prompts the user prior to re-sending a POST request.
**

However, RFC 2616 suggests that the history mechanism is not a cache. Section 13.13 has the following to say:

"History mechanisms and caches are different. In particular history mechanisms SHOULD NOT try to show a semantically transparent view of the current state of a resource. Rather, a history mechanism is meant to show exactly what the user saw at the time when the resource was retrieved."

Thus, the no-store directive should not apply to navigation. This apparent conflict in the HTTP definition is likely to blame for the inconsistent implementations in this case.


The following additional directives are allowed for the Cache-Control header in an HTTP response:

* public— This is the most open directive for the Cache-Control header. It allows any caching by any caching system.

* private— This directive allows caching, but not on public caches.

* no-transform— This directive explicitly requires that the caching system not modify the content part of the HTTP response.

* must-revalidate— This directive requires the cache to always revalidate its copy of a cached resource in cases where the resource has expired. This behavior is usually expected even with the absence of the must-revalidate directive, but like many things in the HTTP definition, it allows for more clarity and an unambiguous requirement.

* proxy-revalidate— This directive behaves exactly like the must-revalidate directive except that it does not require revalidation for private caches.

* max-age— This directive includes a value of the form max-age=600. The value is in seconds, so this example indicates a max-age directive of 10 minutes. This indicates to a caching system that it can send a cached copy of this resource to the Web client, but only if the age of its cached copy is less than or equal to 10 minutes (for this example).

* s-maxage— This directive behaves exactly like the max-age directive except that it is ignored by private caches.

* cache-extension— The HTTP definition allows directives not explicitly defined to be used. When used, any proxy that does not understand the directive must ignore it.

The Pragma header is also sometimes used to ensure that caching intentions can be relayed to Web agents that are not HTTP/1.1 compliant. It is usually combined with the Cache-Control header as a last resort. Because the Pragma header is less flexible, it is much simpler. In general, anything that would be specified as no-cache or no-store should use the following value:

`Pragma: no-cache`
 
Unfortunately, the Pragma header is inconsistently implemented, and some HTTP/1.1 compliant Web agents will interpret it identically to the no-store directive of Cache-Control, although it is intended to be interpreted the same as the no-cache directive. Thus, using Pragma can potentially eliminate any gains made by allowing a caching system to use a cached copy of a resource after revalidation. As a majority of proxies are now HTTP/1.1-compliant, and because the number of HTTP/1.1-compliant proxies should continue to increase, this concern is minimal. Most systems use the Cache-Control header.

#### Conditional GETs

When a browser must request a previously retrieved resource, it will typically issue a conditional GET request. The condition is indicated by an HTTP header such as If-Modified-Since.

An example of a conditional GET request in action is the following series of HTTP transactions that my browser initiates when I visit http://www.google.com/ (HTML and some headers edited for readability):

```
GET / HTTP/1.1 
Host: www.google.com 

HTTP/1.1 200 OK 
Content-Length: 9390 
Server: GWS/2.0 
Date: Tue, 21 May 2002 12:34:56 GMT 
Content-Type: text/html 
Cache-control: private 

<html>... 

GET /images/logo.gif HTTP/1.1 
Host: www.google.com 
Referer: http://www.google.com/ 
If-Modified-Since: Wed, 01 May 2002 12:34:56 GMT 

HTTP/1.1 304 Not Modified 
Content-Length: 0 
Server: GWS/2.0 
Content-Type: text/html 
Date: Tue, 21 May 2002 12:34:57 GMT 
```

Notice that the second request (the request for the image) includes an If-Modified-Since header. Because the image has not been modified since the date indicated in the request, the Web server responds with a 304 Not Modified response. The size of the image (8558 bytes) is a close approximation of the savings in terms of bandwidth.

In order to take advantage of this feature of HTTP, all you must do is ensure that valid Last-Modified headers are included in your responses. For static resources, the Web server will handle this for you, so it is only important that your server's time be set correctly.

>**Note
**
A campaign entitled "Cache Now!" has a few published resources that are good references for Web developers who want to make their applications more cacheable. The home for this campaign is http://vancouver-webpages.com/CacheNow/.

