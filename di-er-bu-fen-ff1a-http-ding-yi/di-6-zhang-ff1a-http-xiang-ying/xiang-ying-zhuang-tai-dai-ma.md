#### Response Status Codes

The most important partof an HTTP response is the response status code. This code is analogous to a summary of the response. It lets the Web client know the basic outcome of the server's attempt to fulfill the request.

Status codes are grouped into the following ranges:

* Informational \(100-199\)

* Successful \(200-299\)

* Redirection \(300-399\)

* Client error \(400-499\)

* Server error \(500-599\)

#### Informational \(100-199\)

The status codes in the 100range are not very common, nor do they provide a crucial function in a typical HTTP transaction. Because these responses are for informational purposes only and thus never contain any content, the response is considered to be terminated by an empty line. Also, because status codes in the 100 range were not defined under HTTP/1.0 or HTTP/0.9, a Web server should not respond with a 100 range response to a Web client identifying itself as using either of these.


##### 100 Continue

The100 Continuestatus codeis intended to be used in cases where the Web client desires feedback from the Web server prior to receiving the requested resource. The definition attempts to leave the implementation flexible, simply providing this allocation for cases where it might prove useful.

This status code is similar to a conversation between two people, where the person currently listening gives a nod or says something to indicate that he/she is listening, and it is fine to continue speaking.

The way a proxy handles this response is a bit unique. Although it is required that a proxy forward all 100 range responses, it must not forward this response to a Web client that identifies itself as an HTTP/1.0 client, unless the Web client specifically included anExpectrequestheader indicating that it was expecting a100 Continueresponse:

`Expect: 100-Continue`

This is, in fact, the most common implementation of this feature. TheExpectrequest header, when used in this way, is analogous to saying, "Guess what?". If the Web server responds with a100 Continueresponse, it is basically saying, "What?". Just as in conversation between two people, this indicates to the person talking that it is appropriate to continue.


>**Note
**
Related material can be found in the description of theExpectrequest header in[Chapter 5](itss://chm/0672324547_ch05.html#ch05), "HTTP Requests."


##### 101 Switching Protocols

The101 Switching Protocolsresponse is reserved for the case where the Web client includes anUpgradegeneral header in the HTTP request. This response indicates to the Web client that the Web server is both accepting the request to upgrade and intending to use the new protocol in all communication that follows this response. The response including the101 Switching Protocolsstatus code will be sent using whatever protocol was already being used.

>**Note
**
Related material can be found in[Chapter 19](itss://chm/0672324547_ch19.html#ch19), "Transport Layer Security."


#### Successful \(200-299\)

The status code most oftenseen in HTTP responses on the Web is200 OK, although some may argue that a404 Not Foundis a popular response as well, due to the number of bad links on the Web. The HTTP specification deems all 200-level responses to be a success of some sort. Partially because there are several types of requests, there are also several success messages. According to the definition, each of these specifically indicates that the Web client's request was received, understood, and accepted by the server.


##### 200 OK

The200 OKresponseindicates to the Web client that its request has succeeded. In general, the content that was being requested is contained in the response, whether it is aTRACErequest fulfillment, the resource indicated by aGETrequest, the header returned in aHEADrequest, or the resource containing the result of action taken in aPOSTrequest.


##### 201 Created

This status code is reservedfor any situation in which the Web client's request requires an entity to be created in order to be fulfilled. In these cases,201 Createdis a successful response. One request that warrants a201 Createdresponse is aPUTrequest.

ALocationresponse header is required for this status code, and it contains the absolute URL of the resource that was created.

>**Note
**
Related material can be found in the description of thePUTrequest method in[Chapter 5](itss://chm/0672324547_ch05.html#ch05), as well as in the description of theLocationresponse header later in this chapter.


##### 202 Accepted

The202 Acceptedstatuscode serves the same purpose as201 Created, except that it is explicitly reserved for cases in which the Web server needs to respond prior to actually creating the entity on the server.

Although the Web client's request was not explicitly fulfilled yet, the idea is that the Web server does intend to fulfill the request, just at a later date. This is useful for cases whereby batch jobs run at night to fulfill the day's requests, human intervention is desired for ensuring all requests are safe, or any other case in which a delay of some sort is appropriate.

Unlike201 Created, this status code does not guarantee that the resource will be created; it simply indicates the intent to fulfill the request. Also, because the location of the resource is unknown prior to its creation, aLocationresponse header is not included.

>**Note
**
Related material can be found in the description of thePUTrequest method in[Chapter 5](itss://chm/0672324547_ch05.html#ch05).

##### 203 Non-Authoritative Information

Sometimes the resourcebeing requested by the Web client can be fulfilled, just not by the intended source. In these cases, a203 Non-Authoritative Informationstatus code is appropriate.

This status code is only returned in cases where the status code would otherwise have been200 OK.


##### 204 No Content

As its name implies,the204 No Contentstatus code indicates to the Web client that the response to its request does not require any content. It should, of course, include any appropriate headers that relay more information about the lack of content.

The definition explicitly states that Web clients handling a204 No Contentresponse from the server should not change the document view. Thus, this status code allows for the communication between the Web client and the Web server to take place without the explicit knowledge of the user.


##### 205 Reset Content

Because the204 No Contentstatus code explicitly states that the Web client should not change the document view after receiving such a response, the HTTP definition provides the205 Reset Content, which explicitly requires the Web client to reset \(refresh\) the document view. All other uses and attributes of this status code are the same.

##### 206 Partial Content

For cases whenthe HTTP requestincludes aRangerequest header specifically asking for a resource in part rather in its entirety, the206 Partial Contentstatus code indicates to the Web client that its request for partial content is being successfully fulfilled. A response with a206 Partial Contentstatus code will include the partial content that is being requested.

>**Note
**
Related material can be found in the description of theRangerequest header in[Chapter 5](itss://chm/0672324547_ch05.html#ch05).

#### Redirection \(300-399\)

Thereare many situations where the Web server wants to inform the client that the resource being requested is located elsewhere. These techniques are generally referred to as protocol-level redirects in order to distinguish them from similar methods used in HTML:

```
<meta http-equiv="refresh" content="0; url=http://httphandbook.org/">
```

Although, theoretically, redirection should serve as a method to point stale links to the correct resource, these methods are most often used when redirection is desired as a normal part of an application. For example, many Web applications use a 300-range response of some sort to redirect the users from a page where content was posted so that any reloads in the browser do not result in thePOSTrequest being resent and thus reprocessed.

>**Note
**
Several status codes in this range behave exactly the same way in practice. This is mostly due to Web agents improperly handling some of these status codes in previous versions of the definition. The HTTP/1.1 definition sought to add some status codes that were intended to be handled in the way some present ones were already being handled so that future implementations could remove the ambiguity that resulted from this situation. These cases are mentioned where appropriate.


Most HTTP responses with a status code in the 300-range are required to indicate thenew location of the resource in aLocationresponse header.

>**Note
**
Related material can be found in the description of theLocationresponse header later in this chapter.

##### 300 Multiple Choices

When a Web server cannotprovide the resource requested but wants to relay alternative resources to the client, it will use a300 Multiple Choicesresponse. The choices may be provided as the content in the response, or one can be provided in aLocationheader in the response.


##### 301 Moved Permanently

The301 Moved Permanentlyresponse indicates to the Web client that the resource is available elsewhere, and that it \(or any proxy\) should use the new resource for all future requests.

The new location for the resource is given in theLocationresponse header, which is required for this status code.

The most common example of this status code is when a URL specified in a request erroneously omits a trailing slash when the resource being requested is a directoryindex.

For example, if a request is made forhttp://httphandbook.org/directoryinstead of the proper URLhttp://httphandbook.org/directory/, a response similar to the following will be sent:

```
HTTP/1.1 301 Moved Permanently 
Date: Tue, 21 May 2002 12:34:56 GMT 
Location: http://httphandbook.org/directory/ 
Content-Type: text/html 
Content-Length: 186 

<html> 
<head> 
<title>301 Moved Permanently</title> 
</head> 
<body> 
<h1>Moved Permanently</h1> 
The document has moved 
     <a href="http://httphandbook.org/directory/">here</a>. 
</body> 
</html>
```

The user is generallyunaware that this transaction takes place, because the Web browser will immediately resubmit the HTTP request using the correct URL. At most, an attentive user may notice the URL change in the location bar when the trailing slash is appended.

>**Note
**
If you are using the Apache Web server, pay special attention to theUseCanonicalNamedirective in yourhttpd.conffile. This directive controls how the Web server constructs the URL that is returned in cases such as this one.

Although it may seem that this type of transaction would also occur when a request is made forhttp://httphandbook.orginstead ofhttp://httphandbook.org/, this is generally not the case. When no resource is specified by the user, the Web browser will assume document root when making the request. Without a resource, the Web browser would be unable to construct a proper request line.

>**Note
**
Related material can be found in the description of theLocationresponse header later in this chapter.

##### 302 Found

A302 Foundresponse issimilar to the301 Moved Permanentlyresponse, except that the new location should only be used for this request. The new resource location is indicated in aLocationheader, and the client should issue the same request method for the new resource as it used for the request that resulted in this response.

>**Note
**
Nearly all known Web clients do not adhere correctly to the HTTP definition with regard to this response code. Regardless of which request method is used in the initial HTTP request, the request method used to fetch the resource in its new location isGET. To allow for a status code that explicitly intends for this behavior, the HTTP/1.1 definition includes the303 See Otherstatus code, which specifies that aGETrequest method must be used for the resource in its new location. Thus, in practice, both302 Foundand303 See Otherare handled in the same manner. Because some older Web clients do not understand the303 See Otherstatus code, most current implementations of protocol-level redirection in which aGETrequest is desired rely on the302 Foundstatus code.

This response is what manyserver-side scripting languages such as PHP and ColdFusion currently use when a developer manually uses aLocationheader in the code \(although discussion is underway for PHP to allow the developer to override the response status code\). For example:

```
<? 
header("Location: http://httphandbook.org/"); 
?> 
```

This will result in a response similar to the following:

```
HTTP/1.1 302 Found 
Date: Tue, 21 May 2002 12:34:56 GMT 
Location: http://httphandbook.org/ 
Transfer-Encoding: chunked 
Content-Type: text/html 

0 
```

##### 303 See Other

The303 See Otherstatuscode is the client's way to explicitly ask the client to issue aGETrequest for the resource specified in theLocationheader. In practice, this status code is handled exactly the same by Web clients as a302 Foundstatus code, although present implementations of the302 Foundstatus code are relying on Web clients improperly handling it, as noted in the description of the302 Foundstatus code.


##### 304 Not Modified

When a client issues arequest and includes anIf-Modified-Sinceheader, the304Not Modifiedresponse is, as you might guess, the Web server's way of letting the client know that the resource has not been modified since the date that the client specified. No content is included in a304 Not Modifiedresponse.

The Web server must also include aDategeneral header unless it does not have a clock. It will also include anETagresponse header and aContent-Locationentity header in cases where it would have included these headers in a200 OKresponse.

Also, because this type of response is usually sent to caching proxies, a Web server will also include anExpiresentity header, aCache-Controlgeneral header, and aVaryresponse header if these differ from the values the proxy cached.

>**Note
**
Related material can be found in the description of theIf-Modified-Sincerequest header in[Chapter 5](itss://chm/0672324547_ch05.html#ch05).

##### 305 Use Proxy

The HTTP definitionreserves a status code for Web servers that want to explicitly force a Web client to use a proxy. When this is the case, the Web server will respondwith305 Use Proxy.

The requiredLocationresponse header in a305 Use Proxyresponse indicates the location of the proxy to be used. It is important to note that this type of response indicates that the use of a proxy is required only for the resource being requested and not necessarily for every resource residing on the current Web server.

>**Note
**
Related material can be found in the description of theLocationresponse header later in this chapter.
****

##### 306

The306status codeis deprecated and no longer used.

##### 307 Temporary Redirect

The307 Temporary Redirectstatus code was added to the HTTP definition due to the improper handling of the302 Foundstatus code by nearly all Web clients. This status code is interpreted just as the302 Foundstatus code was intended to be interpreted. The Web client issues a request to the new location of the resource using the same request method it used in the current transaction \(rather than always usingGET\).

>**Note
**
Related material can be found in the description of theLocationresponse header later in this chapter as well as in the description of the302 Foundstatus code earlier in this chapter.

#### Client Error \(400-499\)

The 400 range of statuscodes are reserved for all error conditions in which the fault lies with the client. Careful distinction must be made for status codes such as404 NotFoundbecause these types of errors seem as if they are not solely the fault of the client. However, each transaction must be viewed independently, much like how a Web server treats each transaction. If a client requests a resource that does not exist, the client is in error, even if it is using an invalid URL due to the HTML previously returned in an HTTP response.

##### 400 Bad Request

The most generic statuscode for a client error is the400 Bad Requeststatus code. This is how a Web server responds if you send it a malformed HTTP request. Because most major Web browsers adhere well enough to the HTTP definition to avoid this type of error, you will most likely never encounter it unless you try to communicate directly with a Web server using telnet, or you write your own Web client.

##### 401 Unauthorized

Using HTTP authentication, aWeb server's first response to a request for a protected resource requiring authorization is a401 Unauthorizedresponse. TheWWW-Authenticateresponse header is included with all HTTP responses with a401Unauthorizedstatus code. In most cases, the users are largely unaware that an "error" response was ever received, so long as the users enter the proper credentials, because the Web client will issue the subsequent request with the access information and display the resulting page.

>**Note
**
Related material can be found in the descriptions of theWWW-AuthenticateandAuthentication-Inforesponse headers later in this chapter, as well as in the description of theAuthorizationrequestheader in[Chapter 5](itss://chm/0672324547_ch05.html#ch05)and in[Chapter 17](itss://chm/0672324547_ch17.html#ch17), "[Authentication with HTTP](itss://chm/0672324547_ch17.html#ch17)."

##### 402 Payment Required

Several status codes areincluded in the HTTP specification in order to make allocations for potential future use. The402 Payment Requiredstatus code is reserved for cases when the resource being requested requires payment. Although there is consistent support for this status code, it is rarely used in practice.

##### 403 Forbidden

A status code of403 Forbiddenindicates to the Web client that the server has reached a permanent error condition and that it cannot fulfill the client's request due to insufficient privileges. This contrasts with the401 Unauthorizedstatus code, whereby the client can resubmit the request, with authentication information included, to obtain the resource.

In practice, the403 Forbiddenstatus code is usually seen after a maximum number of failed attempts at authentication or when a resource being requested cannot be accessed. This status code indicates that either the resource does not exist or that the Web client is not allowed to access it. The definition suggests the use of a404 Not Foundresponse for cases when you do not want to risk arousing the curiosity of a potential attacker.

>**Note
**
Related material can be found in the description of the401 Unauthorizedstatus code earlier in this chapter as well as in[Chapter 17](itss://chm/0672324547_ch17.html#ch17).

##### 404 Not Found

Arguably one of the mostrecognized status codes in HTTP, the404 Not Foundstatus code is used to indicate that the resource being requested simply does not exist. Ofcourse, because receiving any response indicates that a Web server was contacted, it can be assumed that the Web server indicated in the URL does exist. In most cases, this error message is displayed to users, so most people who browse the Web are familiar with it.

In fact,404is such a recognized HTTP status code that it has crept into hacker jargon. Its entry in the jargon file can be found at the following URL:

[http://www.tuxedo.org/~esr/jargon/html/entry/404.html](http://www.tuxedo.org/~esr/jargon/html/entry/404.html)

##### 405 Method Not Allowed

The405 Method Not Allowedstatus code alerts the Web client that the request method used in the HTTP request is not supported. The Web server will include valid methods in the response within anAllowheader.

>**Note
**
Related material can be found in the description of the Allow entity header in[Chapter 8](itss://chm/0672324547_ch08.html#ch08), "Entity Headers."

##### 406 Not Acceptable

When a Web server receivesa demand in an HTTP request in the form of anAcceptheader or any other in theAcceptfamily \(includingAccept-Charset,Accept-Encoding, andAccept-Language\), it will respond with a406 Not Acceptablestatus code when it cannot meet those demands.

The wording of this response can seem confusing, and it is easiest to understand if you consider it to be an admission of guilt on the part of the Web server because it cannot meet the Web client's demand\(s\).

##### 407 Proxy Authentication Required

The407 Proxy Authentication Requiredstatus code is used by proxies. The proxy uses this as a response to a Web client without contacting the server ofthe resource being requested.

In order to indicate how to properly authenticate itself, the proxy will provide the Web client with aProxy-Authenticateheader within this response. The Web client should then submit a second request with a properProxy-Authorizationheader.

>**Note
**
Related material can be found in the description of theProxy-Authenticateresponse header later in this chapter as well as in[Chapter 17](itss://chm/0672324547_ch17.html#ch17).

##### 408 Request Timeout

When the Web server requiresat least one additional HTTP request from the client prior to being capable of sending an appropriate response, the potential timeout condition is handled with a408 Request Timeoutstatus code. This situation can occur when the Web client neglected to include aConnection: closegeneral header when using HTTP/1.1 but fails to make an additional request.

The Web server closes the connection after sending an HTTP response with a408Request Timeoutstatus code.

>**Note
**
Related material can be found in the description of the504 Gateway Timeoutstatus code later in this chapter.


##### 409 Conflict

The409 Conflictstatuscode is reserved for conditions when the Web server is unable to fulfill the client's request due to a conflict with the state of the resource. Such a condition could arise when the resource has changed since the previous required conditions were agreed upon.

The exact description of theconflict should be included in the content of the HTTP response. This tactic, used in many cases, allows for flexibility in the definition, which in turn helps it adapt to new implementations.


##### 410 Gone

In the case in which aresource is no longer available, a410 Gonestatus code can be used in the response to indicate a permanent absent condition. In practice, a404 NotFoundis often used in cases where this status code would be more appropriate.

The410 Gonestatus code is more appropriate in cases where the Web server recognizes the requested resource as being removed rather than simply being unable to find it.

Note

Related material can be found in the description of the404 Not Foundstatus code earlier in this chapter.

##### 411 Length Required

In cases where the HTTPrequest includes content but fails to include aContent-Lengthentity header as appropriate, the Web server responds with a411Length Requiredstatus code to indicate the client's failure to specify the size of the included content.

##### 412 Precondition Failed

In response to a failureof the precondition set forth by the Web client in a request header such asIf-Match, the server uses a412 Precondition Failedstatus code.

##### 413 Request Entity Too Large

In cases in which thecontent included in an HTTP request is too large for the Web server tohandle, it will use a413 Request Entity Too largestatus code in the response.

##### 414 Request-URI Too Long

In cases in which the URLused in the HTTP request is longer than the Web server can handle, it will respond using a414 Request-URI Too Longstatus code.

Although the HTTP definition includes the414 Request-URI Too Longstatus code, it is not recommended that developers depend on a server properly handling any URI that is greater than 255 characters in length. When a substantial amount of data needs to be sent from the Web client, you should employ thePOSTmethod if possible.

##### 415 Unsupported Media Type

The415 Unsupported Media Typestatus indicates that the Web server cannot interpret the content part of the HTTP request. The media type used in the content of the HTTP response should be indicated in theContent-Typeentity header.

>**Note
**
Related material can be found in the description of theContent-Typeentity header in[Chapter 8](itss://chm/0672324547_ch08.html#ch08).

##### 416 Requested Range Not Satisfiable

Because theRangerequest header allows the Web client to specify any range, whether valid or not, the416 Requested Range Not Satisfiablestatus code allows the Web server to respond to out-of-range requests with a sensible error code.

>**Note
**
Related material can be found in the description ofRangerequest header in[Chapter 5](itss://chm/0672324547_ch05.html#ch05).

##### 417 Expectation Failed

When the Web client'sdemands included in anExpectheader cannot be met, the Web server includes a417 Expectation Failedin the HTTP response.

>**Note
**
Related material can be found in the description of theExpectrequest header in[Chapter 5](itss://chm/0672324547_ch05.html#ch05).

#### Server Error \(500-599\)

In cases in which theWeb server can provide a valid HTTP response, but it cannot provide the resource being requested due to a server error of some type, it will use a status code in the 500 range in the response.

Web developers see a status code in the 500 range when they are writing code that interacts with the Web server in some way, such as CGI programming or the development of server-side scripting languages. These types of status codes usually indicate that the Web developer has made an error.

##### 500 Internal Server Error

The most generic servererror status code is500 Internal Server Error. In some cases, the Web server will provide more details about the nature of the error in the content of the response. This is likely the most common status code that a developer creating a CGI script will encounter when things do not go as planned.

##### 501 Not Implemented

The501 Not Implementedstatus code indicates that the Web server does not support the request method being used in the HTTP request.

##### 502 Bad Gateway

If an intermediateproxy server receives an invalid response from either a proxy nearer to the origin server or the origin server itself, it will use a502 Bad Gatewaystatus code in the HTTP response that it sendsback to the client or proxy nearer the client.

##### 503 Service Unavailable

For any case in whichthe Web server is unable to satisfy the request temporarily, it will use a503 Service Unavailablestatus code in its response. It can also include aRetry-Afterheader to indicate when the Web client will be able to try the request again and likely receive a successful response.

>**Note
**
Related material can be found in the description of theRetry-Afterresponse header later in this chapter.

##### 504 Gateway Timeout

If an intermediate proxytimes out while waiting for a response from the Web server, it will send a504 Gateway Timeoutstatus code in the response to the client.

>**Note
**
Related material can be found in the description of the408 Request Timeoutstatus code earlier in this chapter.

##### 505 HTTP Version Not Supported

If the version of HTTPindicated in the request is not supported by the Web server, it will respond with a505 HTTP Version Not Supportedstatus code.

