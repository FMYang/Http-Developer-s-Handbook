#### Brief History and Purpose of HTTP

To learn aboutthe history of HTTP, it is appropriate to learn how the Web as we know it began. HTTP was one piece of a larger picture in the mind of a man named Tim Berners-Lee. In fact, HTTP was simply designed out of necessity to fulfill the ideaof the Web.

In March of 1989, Berners-Lee submitted a proposal to CERN, Conseil Europeen pour la Recherche Nucleaire\(European Organization for Nuclear Research\), entitled, "Information Management: A Proposal." This proposal is considered by most to represent the invention of the World Wide Web. Although the proposal focuses on the general idea rather than specifying every detail, it defines the key concepts that have since changed the perspective of the world. Berners-Lee announced the World Wide Web in August of 1991 on a newsgroup. The original announcement can still be viewed on Google Groups\([http://groups.google.com/groups?selm=6487%40cernvax.cern.ch](http://groups.google.com/groups@selm=6487_2540cernvax.cern.ch)\).

The following time line outlines a few of the important early milestones of theWeb. \(Sources:[http://www.w3.org/History.html](http://www.w3.org/History.html)and[http://www.google.com/googlegroups/archive\_announce\_20.html](http://www.google.com/googlegroups/archive_announce_20.html).\)

* March 1989— Tim Berners-Lee writes "Information Management: A Proposal."

* August 1991— Tim Berners-Lee announces the World Wide Web.

* March 1993— Marc Andreesenannounces NCSA Mosaic 0.10\(Web browser\). It has support for the&lt;img&gt;tag, bookmarks, and a friendly graphical interface under X.

* September 1993— NCSA releases Mosaic for X, Macintosh, and the PC.

* March 1994— Marc Andreesen and colleagues leave NCSA to form Mosaic Communications Corporation\(later Netscape\).

* June 1994— Brian Pinkertonannounces WebCrawler, a Webindex.

* August 1994— Jeff Bezosrecruits for Amazon.

* October 1994— Marc Andreesen announces Mosaic Netscape 0.9 Beta.

* October 1994— Tim Berners-Lee founds World Wide Web Consortium\(W3C\).

* December 1994— First W3C meeting held at MIT.

* September 1995— eBayfounder advertises auctioning service.

* March 1998— First mention ofGoogle.

You may be wondering how HTTP adapted through the years to accommodate the creativity that was being poured into the Web. Because it was originally a simple protocol that allowed for located content to be retrieved, it was insufficient to provide the types of business applications available today. So, as the industry evolved, so didHTTP.

#### HTTP 0.9

The first versionof HTTP is extremely basic. It allows a Web client one method of requesting content,GET. Itincludes no HTTP headers, nor does it even include the version number of HTTP. There is only a single line in a request:

`GET /index.html`

Because thereare no headers in HTTP 0.9, there is only one type of content available, plain text. Of course, documents can still be formatted with HTML, but there are no media types allowing images or any other type of content. This version is very basic, of course, but it fulfills its intended purpose.

The basic series of events in an HTTP 0.9 transaction consist of the TCP connection being established, a request such as the previous example being sent, and the content \(if it exists\) being returned. There are no error codes to speak of, so if the requested content does not exist, nothing is returned. The TCP connection is closed after the content, if any, is returned.

Although all future implementations of HTTP are supposed to allow backward compatibility to previous versions, most modern Web clients and servers are capable ofcommunicating with at least HTTP 1.0.

#### HTTP 1.0

Because HTTP 1.0 wasintroduced early in the life of the Web \(the RFC is dated 1996\), many people never used the Web under the restrictions of HTTP 0.9. My early memories of the Web consist of using the Lynx Web browser to access content, and there is no concept of graphical interfaces, images, or using a mouse to browse. When HTTP 1.0 became widely adopted, which happened rather quickly, the raw potential of the Web became clear.

One of the most noticeable differences in HTTP 1.0, at least to users of the Web, is the capability for the users to send data back to the Web server via HTML forms. This is due to the addition of thePOSTmethod, which I cover in detail in Part II, "HTTP Definition." It is during this time that CGI \(Common Gateway Interface\) programs became popular, and people began creating guest books, forums, and other forms of interactive content. Perl gained a great deal of popularity during this time, mostly due to both the ease with which it handles parsing posted form data and also its wide availability on multiple platforms.

Another quite evident addition wasmedia types, made possible by HTTP headers. With this comes the capability to support Web content that is not plain text, most notably images. With support for the&lt;img&gt;tag in Netscape, graphical Web pages became the norm. It is during this time thatpeople became interested in designing Web pages rather than focusing solely on the content.

Other enhancements to HTTP are not as obvious to the end users. Aside from some additional request methods inaddition toPOST, there are persistent connections, caching mechanisms, and authentication.

Persistent connectionsallow multiple HTTP transactions to use the same TCP connection. This allows a Web browser to connect to a Web server, send an HTTP request, get an HTTP response, and if there is more content required to satisfy the initial request, such as images, requests for this content can be made prior to closing the connection. We speak more of this when speaking of theConnectionheader.

Some very basiccaching mechanismsexist to allow for the use of Web proxies. These are theLast-modifiedandIf-Modified-Sinceheaders. The idea is to allow a Web proxy to store a copy of the Web pages and to send the client that copy if the content has not changed since the copy was made. Although the average user never notices such a thing, aside from possible browser configurations, the introduction of caching turns out to be an important breakthrough.

Authentication is a somewhat rarely used feature of HTTP. If you have evervisited a Web site where you have to enter a username and password into a separate window \(See[Figure 1.1](itss://chm/0672324547_ch01lev1sec1.html#ch01fig001)\), that site is using HTTP authentication. I speak more about this in[Chapter 17](itss://chm/0672324547_ch17.html#ch17), "Authentication with HTTP."

##### Figure 1.1. A Web site requires HTTP authentication.

With all of these new features and several others, HTTP 1.0 can be credited with spurring the amazing growth of the Web in the mid to late 1990s. All major Web browsers and servers still honor backward compatibilitywith HTTP 1.0.

#### HTTP 1.1

According tothe World Wide Web Consortium's Web site, activity on HTTP ceased in May 2000. Although many new ideas are constantly being proposed for the Web, and many of these require interaction with or even integration with HTTP, HTTP 1.1 is the last version of the protocol, and it is considered a stable specification.

There are quite a few notable improvements made in HTTP 1.1, although I will outline only a few. Many of the features introduced in HTTP 1.0 were improved in HTTP 1.1, and several new features were added.

One of the new features was theHostheader. This header is included, and in fact required, in the HTTP request. It specifies the host being contacted. For example, when a request is made for the URL[http://httphandbook.org/](http://httphandbook.org/), theHostheader is

```
Host: httphandbook.org
```

At first, this might seem insignificant. However, it allows a single Web server to host many domains without having to have a separate IP address for each domain. We will speak briefly about the Internet protocol and IPaddressing soon, but it is sufficient for now to know that many domains can resolve to or "point to" the same IP address, where an IP address is a unique address on the Internet. Prior to the inclusion of theHostheader, HTTP had no means by which to accommodate multiple domain names that resolved to the same IP address; a host was considered a unique IP address. Consider the following valid HTTP request:

`GET /index.html HTTP/1.0`

Because/index.htmlis a common resource, several Web sites might have their own/index.html, although their actual content is most likely different. Because this example shows a complete and correct HTTP 1.0 request, the need for more information is evident. Consider the following HTTP 1.1 request as an alternative:

`GET /index.html HTTP/1.1`  
`Host: httphandbook.org`

With thisrequest, a Web server can distinguish this/index.htmlfrom all others, regardless of how many Web sites use the same IP address. This is sometimes referred toasmultihoming.

Another new feature introduced in HTTP 1.1 istherange request. With this feature, a client can request a portion of the entire content, specified by a range of bytes of that content. This adds a great deal of flexibility to the way Web content is requested, and it allows for such features as streaming media.

A similar idea is that of the Web serversending the response back in pieces. HTTP 1.1 also gives us chunked responses. With this feature, the Web server does send the entire response after a single request, unlike the range requests I just mentioned where multiple requests are necessary, but the response is sent in pieces. Each piece has a specific length that is communicated to the client prior to it being submitted. This allows responses to be given as they are generated, so that the end users can begin to see content rather than having to wait for the entire response to be generated prior to seeing any feedback. The same TCP connection is used. I will cover range requests more in[Part II](itss://chm/0672324547_part02.html#part02), "HTTP Definition," as well as in[Chapter 14](itss://chm/0672324547_ch14.html#ch14). HTTP 1.1 also improves on previous features such as caching and authentication. Additionally, persistent connections are now the default behavior rather than an option. Because of this, Web servers must specify the length of the response so that the Web client knows when it is free to send another request on the same TCP connection.

Hopefully you are now familiar with the basic evolution of HTTP and what types of topics this book covers. All of these ideas mentioned here are explained in much more detail, although everything is organized in such a way that you can focus on the things you feel are mostimportant.

