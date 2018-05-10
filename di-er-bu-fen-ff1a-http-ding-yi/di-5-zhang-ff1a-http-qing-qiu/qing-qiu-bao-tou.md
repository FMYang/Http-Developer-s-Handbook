#### Request Headers

Associated with each HTTP request is a collection of HTTP headers. These headers provide supporting information that helps the Web server fulfill the request appropriately. These headers vary widely in purpose, although they belong to one of three groups: request headers, general headers, and entity headers.

Request headers pertain specifically to the request itself. Thus, these headers cannot be used in a response and do not pertain to the content being sent. HTTP/1.1 defines 19 request headers. I will explain each of these as well as an additional request header, Cookie, that has become commonly supported and is defined separately in RFC 2109, "HTTP State Management Mechanism."

#### The Accept Header

The basic purpose of the Accept header is to inform the Web server about the types of content that can be accepted as well as the order of preference for the acceptable content types.

Using the example HTTP request from Chapter 3, where we searched Google for the term "HTTP," you can examine the Accept header sent in the request.

```
Accept: text/xml,application/xml,application/xhtml+xml,text/html;q=0.9, 
        text/plain;q=0.8, video/x-mng,image/png,image/jpeg,image/gif;q=0.2, 
        text/css,*/*;q=0.1 
```

Broken down, you should notice several parts having the same format:

```
text/xml,application/xml,application/xhtml+xml,text/html;q=0.9 
text/plain;q=0.8 
video/x-mng,image/png,image/jpeg,image/gif;q=0.2 
text/css,*/*;q=0.1 
```

Each of these entries consists of one or more content types (also referred to as media types) delimited by commas, followed by a quality factor delimited by a semicolon. The higher the value of q, the more preferred the type. This quality factor can carry any value between 0 and 1 inclusively, with a default value of 1 assumed if it is not specified. A quality factor of 0 is used to denote that a specific type is unacceptable. Because browsers try to be as lenient as possible to guard against obsolete content types, the option to specify a content type as being unacceptable is rarely implemented.

Each content type is divided into type and subtype. The asterisk character is used for either the type, such as */html, or the subtype, such as text/*. It is common to see the use of */* as an acceptable type. This basically allows any type of content, although it will almost always be given the lowest possible preference, so it is a browser's last resort. This helps to keep absent content types in the Accept header from making a browser useless for accessing a specific type of content. If the browser did not have this allowance, any content type not explicitly mentioned would have a quality factor of 0. This is similar in nature to the behavior of browsers to ignore markup tags that they do not recognize.

Allowing a browser to natively support a specific type of content is an entirely different matter, so do not confuse the two. The Accept header is not meant to relay such information. Rather, the browser must determine whether it can render a certain type of content natively. Often the use of plug-ins can enhance a browser's capabilities, such as enabling the viewing of Adobe Acrobat documents, Flash animations, and so on. When a browser cannot determine what to do with the content, it will generally prompt the user to decide whether to open the "file" (although the user will also have to choose which application to use) or to save it to disk. See Figure 5.2.

**Figure 5.2. A browser asks how to handle an unknown content type.
**

If a Web server cannot fulfill a request due to restrictions in any of the Accept request headers (Accept, Accept-Charset, Accept-Encoding, and Accept-Language), the server should respond with a 406 Not Acceptable response code.

#### The Accept-Charset Header

All of the headers in the Accept "family" share similar syntax, so the previous discussion of the Accept header will apply to each of these others in terms of semantics.

The Accept-Charset header informs the Web server which types of character encodings are acceptable, with allowances for specifying the preference with the same quality factor as the Accept header. Using the example HTTP request from Chapter 3, you can examine a sample Accept-Charset header.

Accept-Charset: ISO-8859-1, utf-8;q=0.66, *;q=0.66 
Here you see the asterisk character again matching all character sets. However, ISO-8859-1 and utf-8 have preference because they are explicitly identified. The "catch-all" asterisk character, as with the Accept header, is a last resort.

The character encoding ISO-8859-1 is assumed to have a quality factor of 1 by default. Of additional note is the fact that the HTTP messages themselves are to be encoded with ISO-8859-1 regardless of the content. The possible character encodings themselves are registered with the IANA, Internet Assigned Numbers Authority (see http://www.iana.org/ for more information).

>**Note
**
Character set and character encoding are terms that can be used interchangeably. However, it is a common error to assume the Accept-Encoding header to be related to character encoding, so pay special attention to this distinction.


#### The Accept-Encoding Header

The Accept-Encoding header specifies which types of content encoding the browser has the capability to decode. This is quite different from the notion of character encoding, which is specified in the Accept-Charset header.

As with the character encodings, possible values for this header are registered with the IANA (Internet Assigned Numbers Authority) to ensure interoperability.

The most common uses of content coding are with some sort of compression, where a common compression algorithm such as gzip (GNU zip) is used. This allows for quicker transfers of the content, because the content will be smaller in size.

The same syntax is used for the Accept-Encoding header as the Accept header, with the browser's preference being specified using a quality factor.

#### The Accept-Language Header

The Accept-Language header allows the browser to specify the user's language preferences. The general format is a primary language tag followed by an optional subtag. Examples are en and en-US, respectively. The acceptable language tags are maintained by the IANA (Internet Assigned Numbers Authority).

As with the example HTTP request from Chapter 3, this header is usually quite simple because most browsers support only one language by default. Support for additional languages must be added by the user.

`Accept-Language: en`
 
In this example, the only acceptable language is English. Because no subtype is listed, this will match with any subtype of English, such as en-US.

In order to support multiple languages, you can either use the value of the Accept-Language header in your application logic (usually necessary when generating dynamic data) or you can make your static content available in multiple languages and let your Web server choose the most appropriate one for you based on headers such as Accept-Language and Accept.

For example, Apache users can employ mod_negotiation to select the most appropriate resource. The most common use of this module is to provide static HTML content in various languages by appending an extra language extension to each file corresponding to the language tags allowed in the Accept-Language header. For example, the English version of index.html is index.html.en, whereas the French version is index.html.fr. When a Web client requests index.html, Apache will first search for the resource in the client's preferred language, returning index.html as a last resort. This module can also be used to select the most appropriate format based on a similar naming convention. For more information, see http://httpd.apache.org/docs/mod/mod_negotiation.html.

#### The Authorization Header

The Authorization header allows a browser to authenticate itself with a server. HTTP authorization itself is explained in more detail in Chapter 18.

The most common series of events involves the browser making an HTTP request for content that has been deemed sensitive and subject to a username and password challenge. The HTTP response will be a 401 Unauthorized for the first request, indicating to the browser that it must provide a correction Authorization header to gain access to the content. In order to provide this information, the browser will prompt the user for a username and password, as shown in Figure 5.3.

**Figure 5.3. HTTP authentication prompt.
**

Once the browser has successfully authenticated with a Web server in this way, it will appear to a user as if all further requests do not require reauthentication. However, due to the stateless nature of the Web, every request must include the Authorization header, otherwise the server will respond with a 401 Unauthorized response. The convenient behavior of most modern Web browsers involves the browser storing the access credentials and sending the Authorization header with all HTTP requests for a URL within a domain previously discovered to be protected. Because this utilizes the browser's memory, this convenience lasts as long as the browser (at least one instance of the browser) remains active, and the user will be unaware that this authentication takes place in subsequent requests. This can be a very important factor when debugging HTTP authentication, because if you receive a 401 Unauthorized response without being prompted for a username and password, this suggests that the browser is using incorrect credentials in the Authorization header. Restarting the browser will resolve this situation.

>**Note
**
Related material can be found in the description of the WWW-Authenticate response header in Chapter 6, "HTTP Responses."


#### The Cookie Header

The Cookie header is a crucial part of HTTP state management, which is a topic explained in more detail in Chapters 11-13.

If there are any cookies that correspond to the content being requested, these are sent to the Web server within the Cookie header. Even when multiple cookies are being sent, only a single Cookie header is used. The cookies are separated in name/value pairs delimited by a semicolon. For example:

Cookie: fname=chris; lname=shiflett 
There is a common tendency to consider a cookie an object. People use phrases like "setting a cookie" and "reading a cookie" to denote actions performed by the Web server. This can confuse the issue by making it seem as if the browser is at the mercy of the Web server. The exchange of data with cookies requires more cooperation than these types of phrases might lead you to believe.

When a server wants to set a cookie, it makes a request for the browser to store some data (name/value pairs) to be sent back in subsequent requests that meet the given criteria. See the Set-Cookie response header description in Chapter 6 for more information about this request to set a cookie and the corresponding criteria.

It is up to the browser (and user privacy settings) as to whether any action is taken when a Web server requests a cookie to be set. Because the data in cookies might be personal, users have the option of disabling support for cookies.

If the cookie is accepted, it will be stored in memory or on disk, depending on the criteria in the Set-Cookie response header, and it will be included in the HTTP headers of future requests. It is a common mistake to believe that it is possible to tell whether a browser accepts your cookie(s) after receiving the initial HTTP request. Without receiving a subsequent request, this is impossible to determine. For this reason, some developers use methods that force an additional request after the initial one, often without the knowledge of the user, so that they can better decide which method of state management to implement. See Chapter 11, "HTTP State Management with Cookies," for more information on using cookies to maintain state.

>**Note
**
Related material can be found in the description of the Set-Cookie response header in Chapter 6.


#### The Expect Header

The Expect header allows the browser to relay certain expectations about the Web server within the HTTP request. When the Web server cannot meet an expectation, it must respond with a 417 Expectation Failed response. Intermediate proxies must also meet the expectations or respond with a 417 Expectation Failed. Otherwise, the header should remain unchanged so that the Web server (final recipient) can respond as to whether it meets the expectations.

Quoted values in the Expect header denote a case-sensitive requirement. Otherwise, the expectations are case-insensitive. This can be an important distinction when determining whether a Web server is compliant with an HTTP extension.

Support for the Expect header is sporadic, likely because it is rarely implemented; some HTTP/1.1 agents do not even recognize it. A quick test of your Web server is recommended if support for the Expect header is required.

#### The From Header

The From header relays the email address of the user to the Web server. Due to the privacy concerns of this information, this capability can be disabled in nearly all modern Web browsers.

From: chris@http.org 
The HTTP definition explicitly warns against the tendency to use this information as a rudimentary form of access control or authentication of any sort. For the same reason, this header should not be used as a simple method of client identification.

#### The Host Header

The Host request header is an addition made to HTTP/1.1 that allows for multihoming, which is multiple hosts served by a single IP address. The header itself is quite simple:

Host: www.google.com 
If a port other than the default is used (80 for HTTP, 443 for HTTPS), it is included after the domain, delimited by a colon, just as in the URL syntax.

Recall the HTTP request from Chapter 3, which began as follows:

GET /search?hl=en&q=HTTP&btnG=Google+Search HTTP/1.1 
The resource being requested is represented as a relative URL, relative to the Web server's document root:

/search?hl=en&q=HTTP&btnG=Google+Search 
Because the Web server is the entity receiving the request, any additional information would seem to be superfluous. However, because many Web sites can be adequately supported by a single server, especially with the advances of modern computer hardware, using an entire server for each domain is terribly inefficient and expensive in most cases. This is especially true for Web sites that do not attract a great deal of traffic. Additionally, although servers can support many interfaces, this is still a far more limiting (and expensive) factor than the number of Web sites that can be supported on a single server in terms of resources.

As of HTTP/1.1, the Host header is required for all requests. In fact, it is the only header that is required. This requirement forces the browser to identify the host that it believes itself to be contacting. With a Web server such as Apache, many virtual hosts can be configured.

>**Note
**
This allocation has led to the existence of Web hosting companies. These companies can disperse the high overhead costs of bandwidth, hardware, human resources, and housing across many customers. Most attempt to offer different packages according to the individual needs of the customer as an attempt to isolate potential high-traffic sites for more even dispersion of resources.

#The If-Match Header

The If-Match request header allows the browser to make a conditional request based on the ETag of the resource being requested. The ETag, or entity tag, is a unique identifier of the resource, so the condition the browser makes is based on whether the resource identified in the request line is the same as identified in the If-Match header. The Web server uses the ETag response header to relay this information to the browser initially.

This is helpful in two situations. For caches, this allows the cache to be updated with a minimal amount of overhead. Using the If-Match header in this manner is similar to using the If-Unmodified-Since header. The difference is, of course, the use of a unique identifier rather than a date.

The other situation where the If-Match header is helpful is when the request is an update of some sort to the resource. For example, the PUT method allows the client to alter content on the server. Using the If-Match header can alleviate synchronization problems when the resource has already been changed by someone else since it was last received by the client making the request. See Figure 5.4 to help clarify this example.

**Figure 5.4. The If-Match header aids in synchronization.
**

The server uses the strong comparison function to compare the entity tag. If the condition in the If-Match header is not met, the server responds with a 412, Precondition Failed response. The browser can use the asterisk wildcard to denote any value.

>**Note
**
Related material can be found in the description of the ETag response header in Chapter 6 as well as in the description of the If-Unmodified-Since request header later in this chapter.

#### The If-Modified-Since Header

The If-Modified-Since header is quite intuitively named. It contains a date (see Chapter 9 for more information on dates in HTTP) that is used in a comparison performed by the Web server. If the resource being requested has been modified since the date specified in the If-Modified-Since header, the server will respond with the content as normal. If the resource has not been modified, the server will send a 304 Not Modified response.

This allows a primitive form of caching, whereby the expense of a duplicate response is avoided.

Although dates should always be given in GMT (Greenwich Mean Time), it is important to remember that this comparison takes place on the server. For this reason, it is important to use the value of the Last-Modified response header to determine when the document was last modified according to the server. Subsequent requests can use this date in the If-Modified-Since header to be sure that any changes to the content will trigger a fresh response (See Figure 5.5).

**Figure 5.5. Using the If-Modified-Since request header.
**

Another reason to use this technique is that some servers handle this specification incorrectly and compare the If-Modified-Since header with the resource's Last-Modified value, returning the full content when they do not match, even if the If-Modified-Since date is greater than the Last-Modified one (see Figure 5.6).

**Figure 5.6. Server incorrectly handling the If-Modified-Since request header.
**

>**Note
**
Related material can be found in the description of the Last-Modified response header in Chapter 6 as well as in the description of the If-None-Match request header later in this chapter.

#### The If-None-Match Header

The If-None-Match header is the counterpart of the If-Match request header. This header requests that the server respond with the resource only if its entity tag differs from the value in the header. For this comparison, weak entity tags can be used in the comparison only for the GET and HEAD request methods.

>**Note
**
A distinction is made between validators (HTTP headers used to compare resources) that are guaranteed to be different when the entity changes and those that are usually different when the entity changes, but only when the changes are more than subtle. These are considered to be strong and weak validators, respectively. An entity tag is considered a strong validator unless specifically declared otherwise. More information about the ETag response header is in the next chapter.

For the GET and HEAD methods, the server sends a 304 Not Modified response if the condition is not met. For all other request methods, a response of 412 Precondition Failed denotes that the condition is not met.

As with the If-Match request header, the asterisk character denotes a wildcard and is used to represent any value. This is convenient to safely guard against methods that might potentially overwrite an existing resource.

If a response other than a 200-level response or a 304 Not Modified is returned without the existence of the If-None-Match header, the header is ignored. This allows for error conditions to be communicated back to the client properly.

The most common use of this header is similar to that of the If-Modified-Since request header. The client should simply use the appropriate method based on the information previously received from the server, whether it is an entity tag or a last modified date.

>**Note
**
Related material can be found in the description of the ETag response header in Chapter 6 as well as in the descriptions of the If-Match and If-Modified-Since request headers earlier in this chapter.


#### The If-Range Header

The If-Range header exists to aid clients who have a partial copy of a resource and want to receive the latest copy of the resource in its entirety. Rather than having to request the entire resource again or waste an entire request just to determine whether the partial copy is still up to date, the client can use the If-Range header.

By using the If-Range header in combination with the Range header, the client can receive the remaining resource that it is missing if its own partial copy is up to date. Otherwise, the entire content is received, and an additional request is unnecessary.

Because the client might have received a modified date from the server in previous responses but no entity tag, it is allowed to use the date instead. So, for example, both of the following are valid:

If-Range: "df6b0-b4a-3be1b5e1" 
If-Range: Tue, 21 May 2002 12:34:56 GMT 
The first example behaves like an If-Match header for the condition check, whereas the second behaves like an If-Unmodified-Since header. Logically, these two approaches are similar because the entity tag and last modified date are two ways to identify a specific version of a resource, although there are some obvious differences in the way that specific version is determined.

Because the If-Range header is useless without the accompanying Range header, it is ignored in the absence of a Range header. It is also ignored if the server cannot handle range requests.

>**Note
**
Related material can be found in the descriptions of the ETag and Last-Modified-Date response headers in Chapter 6 as well as in the description of the Range request header later in this chapter.

#### The If-Unmodified-Since Header

The counterpart of the If-Modified-Since request header is the If-Unmodified-Since header. A server response of 412 Precondition Failed indicates that the resource has in fact been modified since the date specified in this header.

In general, a client wants to ensure that the content has not changed before it requests to modify the content, such as with a PUT request, and the client wants to ensure that the content has changed before it requests to receive the content, such as with a GET request.

Thus, the If-Unmodified-Since header is most commonly used as a way to protect against overwriting a resource that has been changed since the current copy being edited was originally received. This is similar in intent to the common use of If-Match to guard against the same scenario.

>**Note
**
Related material can be found in the description of the Last-Modified response header in Chapter 6 as well as in the descriptions of the If-Match and If-Modified-Since request headers earlier in this chapter.

#### The Max-Forwards Header

The Max-Forwards header allows the client to specify the maximum number of times its request may be forwarded by intermediary proxies. This is rarely implemented in browsers but is very helpful in resolving problems with proxies that prevent a response from being returned.

Because each intermediary proxy will attempt to send the request to the destination server, no feedback is returned to the client in the case of an endless loop or a failure by one of the proxies. With the Max-Forwards header, the value is decremented by each proxy. When a proxy receives a request with a Max-Forwards header of 0, it will send a response rather than trying to forward the request.

Figure 5.7 shows a proxy failure and the identification of the problem using the Max-Forwards header. Figure 5.8 shows an endless loop and the identification of the loop using the Max-Forwards header.

**Figure 5.7. Identifying a proxy failure.
**

**Figure 5.8. Identifying an endless loop.
**

For requests other than TRACE and OPTIONS, the Max-Forwards header can be ignored, so these request methods should be used in diagnosing problems.

>**Note
**
Related material can be found in the description of the Via general header in Chapter 7, "General Headers."

#### The Proxy-Authorization Header

If a client must authenticate itself with a proxy, it uses the Proxy-Authorization header in the HTTP request. The use and syntax of this header is exactly the same as the Authorization request header, except that it is the response to a proxy's request for authentication.

>**Note
**
Related material can be found in the descriptions of the Authenticate and Proxy-Authenticate response headers in Chapter 6 as well as in the description of the Authorization request header earlier in this chapter. See also Chapter 17.

#### The Range Header

The Range header allows the HTTP client to request partial content, rather than the usual full content, by specifying a range of bytes it seeks to receive. The client should always be prepared to receive the entire content, because this is how a Web server handles an invalid range and is also how servers that do not understand the Range header respond.

To request the first 500 bytes of content, a server can include the following Range header in the request:

Range: 0-499 
Ranges are represented using the hyphen character, and multiple ranges can be included separated by commas. For example, to request the first 500 bytes and the third 500 bytes, this Range header could be used:

Range: bytes 0-499, 1000-1499 
The syntax also allows for open-ended ranges, such as -1000 for bytes 0-1000 and 500- to receive all but the first 500 bytes. A successful partial content response will be a 206 Partial Content.

>**Note
**
Related material can be found in the description of the Content-Range entity header in Chapter 8.

#### The Referer Header

The Referer header allows the client to identify its current location when making an HTTP request. For example, if I have a Web page on my local server (http://127.0.0.1/) containing a link to a remote URL, my browser will include a Referer header when it sends the GET request to the remote server (when the link is clicked).

Referer: http://127.0.0.1/ 
The most common use of this header is to track how users are finding your site. Most traffic analysis tools will list the top referrers, although this information should only be used to satisfy your curiosity. As with any data originating from the client, very little trust should be given to this information, and it should never be relied upon for any sort of security.

#### The TE Header

The TE header is the same as the Accept-Encoding header in syntax and semantics, but it references the types of transfer encodings it can accept rather than content encodings.

The transfer encodings the browser can support along with an optional quality factor are given. Although the possible values of encodings are identical to the Accept-Encoding header (and collectively maintained by the Internet Assigned Numbers Authority), the TE header allows for an additional type of trailers to indicate that the client can support chunked transfer encoding.

#### The User-Agent Header

The User-Agent header allows the client to identify itself, so that the server can better serve the request. In the example GET request we used to search Google for the term HTTP, the browser identified itself as:

User-Agent: Mozilla/5.0 Galeon/1.2.0 (X11; Linux i686; U;) Gecko/20020326 
Different browsers provide different amounts of information, although it has become fairly common to include the operating system and browser type in the User-Agent string. This information is used for tasks such as user tracking, serving Web pages specific to browser type, and the like. Although it might seem that identical browsers would identify themselves the same, this is not necessarily true. If an application needs to rely on the information in the User-Agent header, key substrings should be matched. Most Web server logs keep statistics on the User-Agent strings being sent in HTTP requests, so these can be referenced to decide which substrings are most appropriate.

>**Note
**
Related material can be found in the description of the Server response header in Chapter 6.

