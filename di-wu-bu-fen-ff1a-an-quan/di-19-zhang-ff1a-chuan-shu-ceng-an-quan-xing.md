#### Chapter 19. Transport Layer Security

Transport Layer Security (TLS) is a formally standardized version of SSL. The biggest difference, in fact, is that TLS is defined and maintained by an international standards body, the Internet Engineering Task Force (IETF). SSL is developed and maintained by Netscape.

The differences between SSL version 3.0 and TLS are extremely subtle. In fact, the authors deliberately intended to adopt SSL version 3.0 as the standard, adding very minor changes that they felt would strengthen security as well as the obvious change in name.

One of the advantages of the IETF's involvement in TLS is that they also control the HTTP protocol. This situation can possibly be credited for RFC 2817, which describes a method for using the Upgrade general header to upgrade to HTTP over TLS. The significance of this is that it allows for a change in protocol without having to utilize a separate port. Thus, a Web server that supports this technique can implement TLS over port 80. An example of a Web client's request is the following:

```
GET / HTTP/1.1 
Host: 127.0.0.1 
Upgrade: TLS/1.0 
Connection: Upgrade 
```

A Web server that accepts this upgrade will issue an HTTP response similar to the following:

```
HTTP/1.1 101 Switching Protocols 
Upgrade: TLS/1.0, HTTP/1.1 
Connection: Upgrade 
```

At this point, a typical SSL handshake will take place over the current connection. It is sometimes confusing to consider that the SSL handshake can take place over port 80 at this point while the Web server can still accept normal HTTP requests over the same port. Note that the upgrade only affects the current TCP connection. Just as a Web server does not (barring extremely odd memory collisions) send the wrong HTTP response to the wrong Web client, it can also keep protocol upgrades straight.

There are two disadvantages to this upgrade that do not exist in typical SSL transactions. Notice that the original HTTP request is sent prior to the SSL handshake. Although this may appear harmless at first, the exposure of the URL is often considered a risk. Consider the following HTTP request as an alternative:

```
GET /?unique_id=1234 HTTP/1.1 
Host: 127.0.0.1 
Upgrade: TLS/1.0 
Connection: Upgrade 
```

A compromise of the unique_id might be something that the use of SSL or TLS is specifically intended to protect against. In the case of upgrading over an existing HTTP connection in this way, the unique_id is not protected. The suggested cure for this risk is a fake HTTP request that is intended for the specific purpose of upgrading the connection. Once the connection has been upgraded, the real HTTP request is transmitted. The following is an example of such an initiation request:

```
OPTIONS * HTTP/1.1 
Host: 127.0.0.1 
Upgrade: TLS/1.0 
Connection: Upgrade 
```

Once the connection has been upgraded, the HTTP transaction(s) can take place in a secure fashion. This situation is more akin to the standard SSL transaction that typically transpires on port 443; the handshake is performed prior to the actual transaction.

Another disadvantage to a client-requested upgrade to TLS is that the Web server is allowed to simply return the requested resource without upgrading. Thus, information that the client intended to be protected by TLS will be transmitted in the clear. Of course, this risk can also be mitigated by employing the technique of a fake HTTP request, as shown in the previous example.

A Web server can also request an upgrade by including the same Upgrade general header as the client. In some cases, it might be more appropriate to force an upgrade with an HTTP response similar to the following:

```
HTTP/1.1 426 Upgrade Required 
Upgrade: TLS/1.0, HTTP/1.1 
Connection: Upgrade 
```

When a Web client receives such a response, it must initiate an upgrade by issuing any of the HTTP requests given in the previous examples. Thus, a server-initiated upgrade will require an HTTP response, HTTP request, and another HTTP response prior to the start of the SSL handshake.

As mentioned briefly in the discussion of the CONNECT request method, a Web client will issue a CONNECT request when it is using a proxy to create a tunnel from the proxy to the origin server. This simply means that it ensures that the proxy passes all messages along exactly as they are sent without any investigation or manipulation. If the proxy were to perform the SSL negotiation, the HTTP messages would be sent in clear text between the proxy and origin server, and therefore would be vulnerable.

This presents a challenge when the Web server is initially unaware that a resource requires TLS. This is of course not the case when the scheme is clearly different (https://) and the port is as well (443). If a Web client wants to offer the Upgrade general header in all of its requests, it must negate the benefit of proxies by using the CONNECT request method to establish a tunnel for every connection.