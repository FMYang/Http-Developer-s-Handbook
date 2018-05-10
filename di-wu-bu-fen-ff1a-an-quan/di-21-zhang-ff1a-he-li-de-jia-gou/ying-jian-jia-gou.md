#### Hardware Architecture

There are a few key characteristics you likely want to focus on as you design your environment:

* Reliability

* Performance

* Security

Each of these characteristics is best approached separately, as often there is an exchange or balance to be made. This is especially true for performance and security, as these two characteristics often seem to conflict. It is especially challenging to find an acceptable balance between them.

#### Reliability

One of the keys to building a reliable environment is to expect everything to fail. This pessimistic approach is an essential characteristic of anyone responsible for the creation of the Web application environment.

There are many aspects of your environment that can fail. Servers can fail, network bandwidth can be depleted, and systems can malfunction. The best safeguard against any type of failure is to have multiple capable resources for every resource that can potentially fail. There are two terms used for such solutions, redundancy and failover. Although these two terms are often used interchangeably, they are not the same.

A failover solution is a solution in which multiple resources can fulfill the same function, so if one fails, another can take its place. It is not necessary that both resources operate concurrently; one may simply be waiting. An example of this type of solution is a typical athletic team. Every player on a team usually has at least one other player that can take his/her place in case of an injury or poor performance. With regards to computer systems, however, it is typical for the replacement to be an exact replica of the original. Most athletic team managers would love to have identical replacements!

A redundant solution, on the other hand, specifically refers to multiple resources sharing the responsibility, so if one fails, the other(s) endures a larger share of the responsibility. An example of this is a rope consisting of many strands of thread. If a single strand breaks, the rope does not. However, the rope has less strength when a strand breaks and is only its strongest when all of them are intact.

The general idea is that rather than trying to prevent failure, you simply expect and plan for it. One common type of failure is the failure of a server. There are three major types of servers in a typical Web environment:

* Web server

* Application server

* Database server

>**Note
**
Although these terms technically refer to software (Apache is an example of a Web server), their use with regard to hardware architecture is shorthand for the physical machine on which the software operates. Thus, a Web server when speaking about hardware is the machine that the Web server software operates on. This chapter uses this shorthand reference for convenience.

A Web server is the server responsible for responding to the HTTP requests received from the Web client. The application server is the server responsible for performing any necessary server-side logic required to generate the appropriate response. In many cases, these two are identical, although it is possible to separate the two, and there are sometimes benefits to doing so, which I will address shortly. Finally, the database server is responsible for the interaction between the application server and the data store.

Consider the structure of the Web environment illustrated in Figure 21.1. This environment has two Web servers, two application servers, and two database servers. Thus, if any single one fails, there is another to handle the associated responsibility. Both Web servers and both application servers are operational, whereas the failover database server is on standby. This is a typical scenario, as the complexity of data integrity is greatly increased when multiple database servers must have access to same data store simultaneously. Often the synchronization necessary to maintain data integrity more than exceeds any performance gains made by sharing the load, so this approach is rarely taken.

**Figure 21.1. A basic three-tiered Web application environment with redundant Web and application servers and a failover database server.
**

In most cases, redundant solutions such as this are also used to enhance performance, as the load can be divided among the Web and application servers. Each collection of identical servers is called a tier, and each tier may or may not be independent. Figure 21.1 illustrates an environment in which each tier is independent. For example, there could be three application servers and only two Web servers.

There are also many situations where a Web server and application server may logically behave as one unit. This situation can exist when both the application server software and Web server software operate on the same machine. It can also exist when a Web server only communicates with a single application server. In these cases, as illustrated in Figure 21.2, the number of Web and application servers must be identical, and the redundancy is in the logical units.

**Figure 21.2. A three-tiered Web application environment that is logically only two tiers.
**

Choosing which environment is best for you involves many decisions. The most important decision to make is whether your environment is being created to host a specific application, which is the case for most enterprise-scale applications, or whether your environment must be capable of hosting many Web applications with different characteristics.

The most flexible environment of the two described thus far is the one illustrated in Figure 21.1. However, this environment is also slightly more complex and more expensive than the one illustrated in Figure 21.2. Generally, you want to isolate logical pieces of your application that cause the heaviest load on your servers without wasting resources. The environment illustrated in Figure 21.2 risks wasting resources, and therefore money, because the performance requirements of the system may require many application servers, and a corresponding Web server must be purchased for each one regardless of whether the Web servers receive considerable load. Consider Figure 21.3 as an alternative.

**Figure 21.3. A two-tiered Web application environment.
**

In this case, the Web server and application server operate on the same machine and possibly as a single logical unit (such as Apache with mod_perl or mod_php). This environment is much easier to configure and maintain, but it runs the risk that a failure in either Web server or application server renders the entire node useless. This same risk exists in the environment illustrated in Figure 21.2, however, so the two-tiered approach is often a better alternative to a three-tiered approach that is logically only two-tiered.

Something that might seem conflicting is that each of these examples uses a single data store. Because the data store is a physical disk (hard drive), redundancy is achieved in a slightly different manner. The most common type of filesystem redundancy is RAID, Redundant Array of Independent Disks (originally Redundant Array Of Inexpensive Disks, but the inexpensive characteristic is unfortunately absent in many modern RAID configurations). There are four common levels of RAID:

* RAID 0 does not offer redundancy but does improve performance with a technique called data striping. This involves spreading a filesystem across several physical disks, allowing simultaneous access.

* RAID 1 provides disk mirroring, meaning the filesystem is actually mirrored across several physical disks. This provides redundancy much like using multiple identical servers, except that instead of enhancing performance, RAID 1 actually decreases it. This is because each write operation must write in two places, nearly doubling all I/O.

* RAID 3 provides data striping, where a filesystem is spread across several physical disks, but it also employs a separate disk that provides error checking for the other disks. The combination of these two characteristics not only allows error detection but also allows data recovery in cases where only one disk fails. It does, however, have a risk of the error-checking disk failing, effectively reducing this configuration to RAID 0.

* RAID 5 is the most common type of RAID used in the industry. It is similar to RAID 3 except that it offers data striping and error checking per byte. This has been found to be a good balance of reliability and efficiency.

>**Note
**
For more information on RAID, a good place to begin your research is http://directory.google.com/Top/Computers/Hardware/Storage/Subsystems/RAID/.


#### Performance

Many of the same approaches to provide increased reliability can also achieve increased performance. In terms of hardware architecture, the most common approaches to improve performance involve dividing the load, which can be achieved with the redundant architectures mentioned in the preceding section combined with the load distribution techniques described in Chapter 16, "Load Distribution."

Building on the two-tiered architecture illustrated in Figure 21.3, consider that there may still be specific functions of your application that are worth isolating beyond your core architecture. For example, if your application displays images of fine art that users can purchase, the delivery of the images might place a strain on your environment. In this case, it might be best to isolate a dedicated image server used to respond to all image requests. To further explain this, first consider the environment illustrated in Figure 21.4. This is a two-tiered environment with a dedicated image server.

**Figure 21.4. A two-tiered Web application environment with a dedicated image server.
**

The load balancer illustrated in Figure 21.4 can provide load balancing as well as failover between the two nodes. If it notices that a node is not responding, it can direct all traffic to the functioning node. In addition, it allows for a single IP address (commonly called virtual IP) or hostname to refer to the entire environment. The image server often uses a separate hostname (as shown in Figure 21.4). Some load balancers, however, offer strong server affinity by inspecting the HTTP transactions. These can direct all requests for resources ending in certain extensions (such as .png, .jpg, and .gif) to the image server so that a separate hostname is not required.

Consider a simple GET request for http://webserver.localdomain/ that returns the following HTML:

```html
<html>
<img src="http://imageserver.localdomain/image1.png"> 
</html>
```
 
Recall that the Web client will issue a separate HTTP request for embedded resources such as images. For example, the Web client will issue an additional HTTP request such as the following after receiving the previous HTML:

```
GET /image1.png 
Host: imageserver.localdomain 
```

Thus, this request gets sent to and serviced by the image server, and the Web environment is relieved of this responsibility.

>**Note
**
More information about load balancing can be found in Chapter 16, "Load Distribution," and information about SSL acceleration can be found in Chapter 18, "Secure Sockets Layer."


#### Security

Adding to the principles from the previous two sections, enhancements can be made in order to provide additional security. In general, everything that increases security beyond the architectures already described adds complexity and decreases performance, so your needs in this area will vary widely from application to application.

In general, Web applications are going to be exposed to many risks simply by the fact that they must be exposed to the Internet. In fact, many Web environments are considered demilitarized zones (DMZs), alluding to the idea that they are the fringes of the battlefield and generally less protected than the local network. Figure 21.5 shows a typical layout for a DMZ. The first firewall (firewalla.localdomain) allows network traffic required for legitimate users to use the services made available by the applications hosted in the DMZ (typically this involves at least incoming traffic for ports 80 and 443), whereas the second firewall (firewallb.localdomain) can operate logically as a one-way valve, allowing outgoing traffic and corresponding responses only.

**Figure 21.5. A DMZ is typically more exposed to risks than a local network.
**

The second firewall can also be used as a proxy so that users on the LAN are not exposed to software vulnerability risks. A proxy, by definition, implements the protocol itself, so it does much more than route network traffic. For example, an HTTP proxy will receive the HTTP requests from local users and then play the part of a Web client when it communicates with the Web server. Once it has received the response, it will respond to the client as if it were the Web server. This approach can protect users from potential vulnerabilities in their Web client software, although not all types of vulnerabilities are protected in this way. It does not, however, protect users from security risks in the Web applications they interact with.