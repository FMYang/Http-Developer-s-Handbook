#### Distributing Computational Load

Computational load refers to the effort required to generate responses under heavy traffic. The distribution of computational load varies widely depending on your application as well as your platform. In general, there are two types of Web applications with regard to computational load:

* Query-intensive applications

* Resource-intensive applications

Query-intensive applications spend a lot of time querying a data store, so load is created when all available resources are waiting for the database responses and incoming requests are having to be queued rather than served. The general approach to overcome this type of load is to make more resources available to generate requests, as a server can likely handle running more resources that are mostly waiting than it can those that would perform more resource-intensive work to generate a response. For example, the Apache Web server uses the MaxSpareServers directive to indicate the maximum number of allowed processes to be available to serve clients. If these processes require less effort in terms of resources to maintain (because they are busy waiting), the server can handle more. Thus, you should experiment with increasing this number, paying close attention to your processor's utilization, the server's memory consumption, and the number of queued responses. You want to make the most use of your processor cycles without overworking it to the point of sacrificing reliability.

>**Note
**
For ColdFusion developers, the number of threads can be increased using the ColdFusion Administrator to achieve the same results. Because ColdFusion is responsible for generating the responses rather than the Web server, you will achieve the greatest gains by tuning it.


Thus, for applications that are query-intensive, computational load can be distributed across more threads (or separate processes) than with resource-intensive applications, as there is less effort per thread. Much of the processing time is spent waiting for a response from the database.

Resource-intensive applications are tuned opposite of query-intensive applications. The load experienced on a resource-intensive application is usually the load on the server itself caused by an overworked processor or depleted memory. This load can be mitigated by limiting the number of resources available to serve client requests, but this must be balanced with the latency experienced by the users. For example, as you decrease the available resources (limiting the number of spare servers for Apache or the number of threads for ColdFusion), the server will experience decreased load (and less stress), but the request queue will grow and the users will experience slower responses. The same tuning process can be applied as with query-intensive applications, where processor utilization, memory consumption, and the request queue are monitored in order to identify the best balance. You should expect to use fewer processes/threads for a resource-intensive application, however, because each one is working harder.


#### Summary

There are many techniques to load balancing, and almost all professional Web applications that receive heavy traffic utilize one or more of these techniques. The most appropriate solution for your Web applications depends on many factors, but you should now have a good idea as to what kind of information to consider when making your decision. For more detailed information on load balancing, I recommend the book Server Load Balancing by Tony Bourke, published by O'Reilly and Associates.

The next several chapters focus on the security concerns associated with Web application development. They begin by discussing HTTP authentication.