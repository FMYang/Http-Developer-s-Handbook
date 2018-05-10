#### Chapter 11. HTTP State Management with Cookies

A Web server's task is to fulfill each HTTP request that it receives. Everything that the Web server considers when generating the HTTP response is included in the request. If two entirely different Web clients send identical HTTP requests to the same Web server, the Web server will use the same method to generate the response, which may include executing server-side programming logic.

If a unique response per client is desired, one of two things must happen:

* The method that the Web server uses must generate a unique response somehow, even though the method itself will not vary. For example, the server-side logic being executed can use the client's IP address to generate unique content per IP address.

* Something in the HTTP request itself must be unique.

Depending on the first method to distinguish unique clients is unwise, because the same client may appear to be different when only the data in the environment is used for identification. For example, the common use of round-robin proxies may make the same client appear to be coming from a different IP address for every request. For example, this is the case for America Online (AOL) users.

In contrast, two different clients may be mistaken for the same client due to a similar reason. These types of situations can occur when the clients are separate workstations in a school's computer lab, for example. The lab may be configured to masquerade the IP address of all outgoing communication so that each computer appears to be coming from the same IP address (See Figure 11.1). In addition, most lab environments will run identical software, so there is very little chance of identifying anything in the environment that can be used to distinguish clients. Thus, you should always depend on the second method for client identification.

**Figure 11.1. An HTTP request appears to originate from a proxy.
**

The lack of state management within HTTP poses a challenge with the development of most modern Web applications. Without a method of associating one transaction with another (state management), it is impossible to maintain any idea of the user's status within the application (session management), such as whether the user is currently logged in, whether the user has saved items to be purchased later, and so on. In this chapter, I will discuss maintaining state with HTTP's state management mechanism, cookies.

>**Note
**
Without studying other protocols, within which the client's current state is an integrated part of the semantics of the communication protocol, it is impossible to contrast HTTP's characteristics, which is very helpful for clarifying these issues. For this reason, I highly recommend studying another application protocol such as FTP to further advance your expertise.