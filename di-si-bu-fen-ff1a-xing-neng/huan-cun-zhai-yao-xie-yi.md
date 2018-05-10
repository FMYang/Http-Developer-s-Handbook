#### Cache Digest Protocol

Cache Digest Protocol is a caching protocol that utilizes digests to help eliminate the necessity of peer queries. A digest is a compressed summary of a cache's contents, and a cache keeps a digest for each of its peers. Users of mailing lists are probably familiar with the digest mode available in most mailing lists. This option allows the subscriber to receive a single message per day with all of the day's messages rather than receive each message. A cache digest is similar in that it summarizes a specific cache's contents, but the format is compressed at the risk of not having perfectly accurate information in the digest. Much like a failed query using ICP, a failed lookup in a digest only makes an accurate response slower, thus this risk is acceptable.

Because digests are exchanged less often than ICP queries, TCP can be used to ensure reliable and accurate digests. The size and format of the digests are more important because lookups are much more frequent than digest exchanges. Additionally, digests are made available by caches as URLs, thus HTTP is generally used to exchange digests among peers. For example, a digest from proxy.localdomain can be fetched with a request similar to the following:

`GET /cache_digest HTTP/1.1 `
`Host: proxy.localdomain `

>**Note
**
For more information, I highly recommend the cache digest FAQ located at http://www.squid-cache.org/Doc/FAQ/FAQ-16.html. Although this site pertains specifically to the Squid caching software, it gives a nice overview of the basic theory of cache digests as well as Squid's implementation.

