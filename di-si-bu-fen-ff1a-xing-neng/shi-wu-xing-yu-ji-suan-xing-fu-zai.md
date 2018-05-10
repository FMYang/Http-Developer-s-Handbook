#### Transactional Versus Computational Load

When deciding on the optimal configuration for performance, it is important to cater your Web serving environment to a specific application whenever possible. When you must construct an environment suitable for a wide array of Web applications, you are not afforded this luxury, so you have to estimate the most common types of applications your environment will host.

There are two different types of load you need to plan for:

* Transactional load— Load on the network or a server caused by heavy traffic

* Computational load— Load on a server caused by complex computations and/or heavy traffic

Transactional load is the load on a Web serving environment that is primarily the result of the HTTP messages themselves. For example, if your Web server is serving static pages, the only type of load you will likely be concerned with is transactional load. Therefore, you will focus on load-balancing techniques that allow you to support as many HTTP transactions as possible.

Computational load is the load that becomes a concern for most dynamic Web applications. This type of load is also due to heavy traffic, but it is the load that will burden your servers responsible for generating the dynamic content.

In most cases, you will experience both types of load. The reason for this is that any Web application receiving enough traffic to warrant concern for transactional load is most likely going to experience heavy computational load as a result. The methods for alleviating these types of loads basically involve distributing the load across multiple resources. The method of distribution, however, varies depending on the type of load being distributed.