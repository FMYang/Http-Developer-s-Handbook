#### Internet Cache Protocol (ICP)

Internet Cache Protocol (ICP) is arguably the most popular caching protocol. A major contributing factor to its popularity is its use by Squid (http://www.squid-cache.org/), one of the most common Web proxy caches in use.

ICP is defined in RFC 2186, "Internet Cache Protocol (ICP)." Another RFC of interest is RFC 2187, "Application of Internet Cache Protocol (ICP)," which explains how to apply ICP to hierarchical Web caching.

ICP is basically a lightweight protocol that gives peer caches a method of querying one another for a resource. Peer caches respond to these queries with either a HIT or a MISS denoting whether they possess a copy of the resource or not. This interaction is very similar to two people playing the popular board game Battleship.

In order to make such queries as timely as possible, all peer caches are queried simultaneously, and the first peer to respond with a cache hit is assumed to be the best choice for obtaining the resource. The actual fetch of the cached resource lies outside of the scope of ICP and traditionally involves a standard HTTP transaction.

ICP is an application-layer protocol commonly implemented on top of UDP, the User Datagram Protocol. Unlike TCP (Recall the discussion in Chapter 3, "HTTP Transactions," about TCP), UDP is a connectionless protocol. There are many required messages when using TCP to handle error conditions (three messages are required just to establish a connection). Although this makes TCP a very reliable transport-layer protocol, it creates far too much overhead than caching systems can afford. UDP is much less reliable because there is no guarantee that a message was received, but this makes it very efficient. When a cache queries a peer for a cached resource, timeliness is essential. Additionally, because a failure in the communication is only equivalent to a cache miss, the risk associated with UDP is well worth the added efficiency.