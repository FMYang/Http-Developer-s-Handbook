#### Cache Array Resolution Protocol

The idea of Cache Array Resolution Protocol (CARP) is to allow multiple caching proxies to function as a single proxy. Each HTTP response that is cached by the group is labeled with its URL and the identity of the cache that is storing it, although a message digest of this data is used to be more efficient and consistent.

Because cached resources are indexed by URL and the identity of the storing cache, any participating cache can use CARP to determine exactly where a copy of a particular resource is located rather than having to query several peers. This deterministic characteristic is one of the key advantages of CARP and is similar to the idea of cache digests.

The URLs cached by participating proxies are partitioned across the proxies using the hash just mentioned. Although this is what allows a deterministic path to be created, it also potentially overworks some caching proxies while under-utilizing others. If a cache is unfortunate enough to store URLs that are far more popular than the others, the cache can receive a majority of the overall queries. This lack of load balancing is one disadvantage of CARP.

Because the message digest of cached resources is the key to CARP, any changes in the participating members of the array require a complete recalculation of this hash, including the distribution of URLs. For example, the addition of another caching proxy to aid in the workload of the array involves a redistribution of URLs.

>**Note
**
CARP support in the caching software Squid is available by running Squid's configure script with the --enable-carp option.