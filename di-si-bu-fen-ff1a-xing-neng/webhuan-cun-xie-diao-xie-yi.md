#### Web Cache Coordination Protocol

Web Cache Coordination Protocol is a caching protocol that is more closely related to the network than those covered thus far. It was originally a proprietary protocol used by the Cisco Cache Engine, but it has since been opened. WCCP is simply a form of interception caching, which involves the HTTP request being intercepted and redirected to the caching proxy. Interception caching requires no special configuration on the Web client, which makes it very useful for providing transparent caching.

>**Note
**
Transparent caching has been the cause of some controversy, because it allows Internet Service Providers the capability to monitor the Web activities of their customers without the customers being aware.


Because of its association with lower-level networking fundamentals, interception caching can seem a bit daunting. The implementation is very straightforward, however, as it is basically implementing IP filtering and forwarding, which is a task that will be very familiar to a network administrator. The interception must take place on a machine that will be along the request path. All traffic destined for port 80 on the origin server is redirected to the caching proxy. Responses are then redirected to the Web client as expected.

>**Note
**
Linux users using a 2.4.x kernel can use iptables to achieve IP filtering and forwarding. For 2.2.x kernels, ipchains can be used, and for 2.0.x kernels, ipfwadm can be used. For more information on implementing various types of interception caching, see http://www.squid-cache.org/Doc/FAQ/FAQ-17.html. Specific information regarding WCCP (version 2) can be found on Cisco's Web site at http://www.cisco.com/univercd/cc/td/doc/product/webscale/webcache/ce23/swconfig/chap4.htm.


#### Summary

Although it is unlikely many Web developers will need to implement a Web cache, because this task is traditionally assigned to network administrators, all Web developers will interact with Web caches in one way or another. By understanding the procedures that Web caches use to achieve caching, you can be better prepared to debug complex situations.

I highly recommend creating a Web cache in order to help visualize and elaborate on the points made in this chapter. Squid is a freely available open source Web cache found at http://www.squid-cache.org/.

The next chapter covers a few methods of distributing Web traffic. This can help improve the performance of your applications by expanding the capacity of your Web serving environment.
