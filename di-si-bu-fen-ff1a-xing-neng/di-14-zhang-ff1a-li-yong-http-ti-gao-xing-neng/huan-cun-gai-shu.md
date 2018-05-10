#### Caching Overview

Caching can refer to many concepts. The general meaning of cache is to store a copy of something to prevent the necessity of retrieving it again. When speaking of Web development, there are three main types of caching:

* Caching on the server— Storing a complete or partially generated resource on the server to keep from having to regenerate it.

* Caching on the client— Storing a resource on the client to keep from having to receive the entire resource again.

* Proxy caching— Storing a resource on a proxy to allow direct replies to an HTTP request rather than having to receive the entire resource from the origin server again.

Although there are many side advantages to caching, there are three core benefits:

* Improve response time from a user perspective— This is what most Web developers focus on, the user experience.

* Lessen network load— Many Web developers overlook this metric because bandwidth is often viewed as an expendable resource, where more can be purchased as needed.

* Lessen server load— This metric is more difficult to overlook, as it directly impacts the user experience in terms of performance and reliability (stressed servers fail more often).

One perceived benefit of caching, particularly with proxies, is that network load can be less centralized. By keeping copies of responses closer to the client and forwarding only necessary requests to the origin server, backbone traffic is lessened and moved to the "fringes" of the Internet. Figure 14.1 illustrates this perception.

**Figure 14.1. Caches located near the client help to decentralize network load.
**

There are some key concerns that you should be aware of with regard to cached responses. Unfortunately, it is these concerns that lead some developers to make drastic decisions about their use of caching, such as making every effort to disable it completely. Although you should consider the following concerns, you can often avoid these problems with a little effort:

* The user receives a stale response.

* The user receives personalized content intended for another user.

* Sensitive information is cached, exposing it to a greater risk of compromise.

>**Note
**
Caching is deliberately defeated by some Web applications in an attempt to ensure that accurate and current HTTP responses are sent to the Web client. Caching should rarely be disabled in this manner. By applying a small amount of effort, and by possessing a general working knowledge of caching behavior as it relates to the Web, you can greatly enhance the performance of your applications as well as potentially reduce the load on your network and servers. The next section, "Controlling Caching with HTTP," provides several methods that you can use to take advantage of caching.


#### Caching on the Server

There are several types of caching that can occur on the Web server (or Web serving environment). All caching techniques on the server attempt to ease the generation of content. This is achieved primarily by using one of two techniques:

* Store previously generated content to avoid regenerating identical content for multiple requests.

* Cache pre-compiled code, database queries, or anything else that can decrease the time necessary to dynamically generate content.

There are numerous methods of achieving some sort of caching on the server. Most application platforms allow caching of precompiled code or at least caching (storing in memory rather than on disk) of the code required to generate dynamic content.

Some developers employ their own techniques to perform caching behavior on the server. For example, the popular Web site http://slashdot.org/ uses independent Perl scripts (running as daemons) to generate the dynamic content. Rather than execute a script for each visit to the site, these daemons execute the scripts at regular intervals, thus generating static content. It is this static content that is actually sent to the Web clients interacting with the site, which is why some users may recognize a delay (a few seconds) in the content updates. This static content is a type of cached content.

This approach also allows you to take advantage of all of the performance benefits that HTTP has to offer by placing this responsibility on the Web server. Thus, not only do you benefit from the server-side performance savings in not having to generate dynamic content for each request, but you also benefit from the network bandwidth savings that the Web server will undoubtedly provide by handling all of the performance-enhancing features of HTTP.

In general, caching on the server lies outside of the scope of HTTP and depends largely on your application. However, these general guidelines can help you achieve the greatest performance from your environment, and your focus in this area should be to avoid the regeneration of identical content when possible or to ease the effort required by the server to generate content.

#### Caching on the Client

Most Web browsers will perform a fair amount of caching on their own. In fact, because a Web browser must retrieve content prior to rendering it, it will store a copy of all content it receives in memory, on disk, or both. Thus, a browser will cache by its very nature, although the HTTP headers determine whether the browser will use the cached copy or retrieve a fresh resource when the user initiates a request.

The benefits of Web client caching affect only the user of the particular Web client. In general, the closer to the Web client that caching occurs, the less latency the user will experience and the less benefit there will be for other users. For example, some organizations employ a caching proxy for the collective benefit of all users of their network. Although this allows more people to benefit from the caching, the benefit is slightly less for each user than the caching their own Web clients perform.

#### Caching Proxies

A proxy is a server that implements a certain protocol on behalf of its clients in order to provide a gateway (based on protocol) between a local network and the Internet. For example, a Web proxy is one that implements HTTP. When you use a browser configured to use a proxy, all of your browser's HTTP requests are actually sent to the proxy. The proxy then plays the role of a Web client and requests the resource from the Web server on the client's behalf, relaying the server's response back to your browser as if the proxy itself is replying to your request. From the Web server's perspective, the proxy is the Web client. From the Web browser's perspective, the proxy is the Web server. In a typical office environment, a Web proxy is the only way that clients on a local network can browse Web sites on the Internet.

Most Web proxies implement some form of caching. These proxies are servers that reside somewhere between the Web client and the Web server, and they can help shorten the request path by replying to the Web client directly using a previously cached response from the Web server.

Proxies are generally categorized into two groups:

* Public— Proxies that are shared by multiple clients.

* Private— Proxies that are not shared.

In many cases, the location of a proxy will help to identify its type. Private proxies are located very near the clients that they serve (often on the same computer), whereas public proxies are nearer the origin server so that they may serve a larger user base. The distinction between these two types of proxies becomes important in the interpretation of the semantics of HTTP because some directives apply differently depending on whether the proxy is public or private. These details are explained in the following section.

Always consider the behavior of caching proxies when you are developing an application to be used by the general public. HTTP/1.1 gives Web developers a considerable amount of control over the caching behavior employed by Web proxies. Unfortunately, too many Web developers do not take advantage of this opportunity.

Proxies will generally cache responses while adhering very consistently (more consistently than Web clients) with the HTTP protocol. A few protocols specific to caching servers are introduced in Chapter 15, "Introduction to Caching Protocols."