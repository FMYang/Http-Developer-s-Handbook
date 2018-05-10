#### Distributing Transactional Load

When considering transactional load alone, the most common techniques of load balancing involve dividing the HTTP requests across multiple Web servers. Thus, each Web server will only serve a fraction of the total requests, and the overall capacity of your application is multiplied (ignoring load balancing overhead) by the number of Web server nodes in your environment. I will discuss two very different methods of achieving transactional load balancing—DNS round robin and hardware load balancers.

#### Load Balancing with DNS

Sometimes referred to as the poor man's load balancing solution, DNS round robin can be an effective means of distributing transactional load. In Chapter 2, "The Internet and the World Wide Web," the fundamentals of DNS were explained. Typically, a Web client will perform a DNS lookup for a domain name (such as httphandbook.org) in order to obtain the IP address to which it will send its HTTP request. This series of events is illustrated in Figure 16.1.

Figure 16.1. A Web client performs a DNS lookup.

graphics/16fig01.gif

With DNS round robin, the load balancing actually takes place during the DNS lookup. Multiple IP addresses will be associated with a single domain name so that DNS lookups will return different IP addresses for different requests. Figure 16.2 illustrates this situation. Although 10.1.1.2 is returned for the DNS lookup illustrated, the next lookup will return 10.1.1.1, thus the next request will be serviced by the 10.1.1.1 Web server.

**Figure 16.2. DNS lookups return different IP addresses for different requests.
**

>**Note
**
Although private (local network) IP addresses are shown in the illustrations for DNS round robin, public IP addresses are indeed necessary for public Web sites.


Although DNS round robin has the advantages of being inexpensive and simplistic, it provides no method of failover and lacks server affinity.

Failover refers to the capability to handle a server failure. For example, if the 10.1.1.1 Web server in Figure 16.2 were to fail, there is nothing to keep a client from attempting to access this server directly, likely receiving an error message. Even if the 10.1.1.1 Web server were removed from the name server's host lookup table, the common use of caching name servers on the Internet would require a few days for this change to propagate. Hopefully your Web server will be fixed by then!

Server affinity is a common phrase that refers to the capability to direct a request to the most appropriate server. The most common use of this is to maintain the same server for a particular client. This is also commonly referred to as sticky sessions, and there are generally strong and weak versions of this behavior. Methods of strong server affinity can almost guarantee that the same server is used for a particular client, and an error is returned otherwise. This generally requires an inspection of the HTTP request for a unique identifier much like the method used to maintain state. Methods of weak server affinity are more efficient and generally do not involve an inspection of the HTTP message but rather the TCP/IP packet. For example, a common method of weak server affinity is to use the client's IP address as a way to identify the client. That is certainly weak!

The lack of server affinity is rarely a concern for a well-designed Web application. Because any type of server affinity (even weak) requires additional overhead, it will adversely affect performance. Instead, an application should be designed so that no particular server is expected to serve a client's request. If client data is maintained in a shared data store, such as in a database, the most common need for server affinity is removed.

#### Load Balancing with Hardware

A popular alternative to DNS round robin is a hardware load balancing appliance. This type of load balancing involves a load balancer that communicates directly with the Web client and each Web server, thus providing a gateway for the communication. In this case, the IP address associated with the domain name will be the virtual IP address (the load balancer's IP) for the entire environment. HTTP requests sent to this IP address will be received by the load balancer and forwarded to an available Web server as appropriate. See Figure 16.3 for an example of a Web server configuration utilizing a load balancer.

**Figure 16.3. A load balancer can distribute traffic across multiple Web servers.
**

There are many types of load balancers. The major difference between the various types is the method used to determine the most appropriate Web server to receive a request. Some load balancers monitor the load on each server in order to determine the server under the least amount of load. Some simply distribute requests based on HTTP messages alone, ignoring potential differences in the effort required to generate responses.

One common characteristic found in many load balancers is the capability to detect failover. Although the methods used to determine server failure vary, this characteristic allows the load balancer to immediately remove a failed server from its pool so that no requests get forwarded to that server.

Another advantage to load balancers is the security afforded by "hiding" the Web servers from the Internet. By only allowing access to Web servers through the load balancer, all incoming traffic can be subject to the same firewalling rules, and the Web servers themselves cannot be accessed directly.

Because all incoming HTTP requests are received first by the load balancer, server affinity is possible. As mentioned in the previous section, the methods of achieving server affinity vary depending on how much effort is given to ensuring the same server responds to a particular client. Because a well-designed application rarely requires server affinity, this advantage should be of little concern.

>**Note
**
There is an Apache module called mod_backhand that you can use to create your own load balancer. For more information, check out http://www.backhand.org/mod_backhand/ .