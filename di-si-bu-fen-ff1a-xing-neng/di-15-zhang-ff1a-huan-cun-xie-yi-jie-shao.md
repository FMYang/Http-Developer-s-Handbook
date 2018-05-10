#### Chapter 15. Introduction to Caching Protocols

In many of the examples in this book, a very simplistic environment is used to focus on the topic at hand. Realistically, computer networks are more complex versions of the fundamental ideas presented here. This is especially true with caching proxies.

In the case of caching proxies,there are situations that cannot be illustrated quite as simplistically. For example, when a cache determines that the cached copy of the resource being requested is stale, the best course of action is not necessarily to contact the origin server. If the origin server is unreachable, non-responsive, or simply many networks hops away, a better alternative might be to contact another nearby cache that might have a fresh copy of the resource.

In practice, this idea of cooperation among caches is quite common. There are two main categories of organizational schemes used by cooperating caches:

* Hierarchical

* Peer

[Figure 15.1](itss://chm/0672324547_ch15.html#ch15fig001)illustrates the idea ofa hierarchical caching scheme. When a client makes a request, the local cache will return the HTTP response if it has a copy. Otherwise, it will query the central cache to see if it has a copy. The central cache will respond directly to the local cache if it has a copy of the HTTP response, otherwise, it will contact the origin server and then respond to the local cache. In the latter case, both the central and local caches will keep a copy of the HTTP response.

##### Figure 15.1. Caching proxies can be organized hierarchically.



A peer cache is anyorganization where the roles and responsibilities of the individual caches are similar or even shared. In many situations, the distinction between organizational schemes is unclear. In fact, some organizational schemes found in practice mix the two. For example,[Figure 15.2](itss://chm/0672324547_ch15.html#ch15fig002)illustrates a hybrid organizational scheme. Although this configuration appears to be simplistically hierarchical, the fact that local caches A and B reside within the same company's local network means they can be configured to cooperate with one another to provide localized caching. Company B might also implement a similar configuration. In many cases, this type of localized caching is effective because the browsing habits of employees in the same company are often similar.

##### Figure 15.2. Local caches within the same company may cooperate as peers.



When a caching proxy needs to fetch a resource from another cache, the HTTP protocol is adequate. In this case, the requesting proxy becomes the HTTP client, whereas the other cache plays the role of the HTTP server. HTTP can be used to query whether a cache has a copy of a specific resource as well, but more efficient protocols have been created specifically for inter-cache communication. These types of protocols are calledcaching protocols. This chapter discusses four commoncaching protocols:

* Internet Cache Protocol \(ICP\)

* Cache Digest Protocol

* Cache Array Resolution Protocol \(CARP\)

* Web Cache Coordination Protocol \(WCCP\)

These protocols are closely related to HTTP, and this brief introduction gives you a better understanding of the behavior of caching proxies. This will allow you to better appreciate the performance benefits caching can yield in your own Web applications as well as allow you to participate in the design and implementation of caching proxies within your organization.

