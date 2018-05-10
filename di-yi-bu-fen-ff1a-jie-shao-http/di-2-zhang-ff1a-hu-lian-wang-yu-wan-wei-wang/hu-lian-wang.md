#### The Internet

So what exactly is the Internet? Physically, the Internet is a worldwide network of computers. When you are connected to the Internet, you become a part of it; your computer is a member of this worldwide network \(see [Figure 2.1](itss://chm/0672324547_ch02lev1sec1.html#ch02fig001)\). What makes the Internet so important is how widely available it is combined with the usefulness and popularity of its services.

**Figure 2.1. Your personal computer connected to the Internet.**

In this sense, the Internet is similar to a newspaper. Physically, a newspaper is merely paper and ink. There is nothing really extraordinary in that regard. A successful newspaper, however, is one that is both widely distributed and read and respected by a large audience of people. It is this combination of popularity and availability that makes a successful newspaper an important medium for communication. The Internet is similar in this regard; the concept is its most important aspect.

Although a newspaper can provide a distribution medium for photographs, drawings, articles, advertisements, and the like, it has the limitation that it can only provide a medium for those items that can be printed on paper. A newspaper also has limited availability, although some are admittedly very widely distributed. The Internet, on the other hand, is both more flexible in terms of the content and services it can provide, but it also boasts worldwide availability and more popularity.

In order to operate, the Internet requires a common means of communication. For all the computers connected to the Internet to be able to communicate, it is important that they all speak the same language. In this case, thatlanguage is IP, Internet Protocol.



#### Internet Protocol

We have alreadydiscussed briefly what a protocol is. In a book on HTTP, it might seem off-topic to describe additional protocols. However, we need to skim the surface of networking in order to understand how HTTP fits into the bigger picture, and Internet Protocol is the common foundation for networking on the Internet. There are two important keys to Internet Protocol that I discuss:addressingandrouting.

As just noted, every computer connected to the Internet is a part of it, including your computer when you are online. Corresponding to this, every computer connected to the Internet has a unique address assigned to it, the IP address. Technically, the IP address is assigned to an interface on a computer, such as a network card or a modem. Because computers can have more than one interface, thus more than one IP address, this distinction is important. For simplicity, I will refer to the computer as the entity assigned the IP address when it only has one.

An IP address is really just a binary number. Binary numbers consist of 0s and 1s. Each digit in a binary number is called a bit, and eight bits make up a byte. The term byte is used more often in conversation, and you are probably familiar with disk space being measured by some denomination of bytes such as kilobytes or megabytes.

32 bits make up an IP address. It is commonly written as four octets\(8 bits each\) separated bydecimals, where each octet is represented as a decimal number so that it is friendlier to read. For example, 127.0.0.1 is an IP address. In this case, 127 is the decimal representation of the first octet. The eight bits that make up 127 are 01111111. For the purposes of understanding HTTP, however, we will not discuss binary numbering further and will refer to IP addresses using decimal notation, such as the example of 127.0.0.1.

The other fundamental idea of Internet Protocol we need to discuss isrouting. AnIP address specifies a unique location on the Internet, but it is routing that discovers a deterministic path from the local computer to anothercomputer on the Internet specified by an IP address.

With the staggering amount of computers connected to the Internet, it might seem surprising that messages sent from your computer will arrive safely and consistently at any location on the Internet. This is accomplished through routing. Each computer on the Internet has a routing table that it references to determine where to send a message according to the message's destination. Of course, a computer can only send messages directly to interfaces of other computers to which it is connected, so the fewer interfaces a computer has, the simpler its routing table is. For example, if your personal computer has no network connections aside from a modem, all of your IP packets \(messages\) will be sent through that interface regardless of the destination.

In turn, each computer that receives a message for which it is not the final recipient will forward the message along according to its own routing table. Eventually, the message will reach its destination. A computer that forwards messages in this manner is referred to as agateway, and each routing table has at least one default gateway where all messages are sent that have a destination not specifically mentioned in the routing table.

This gives us enough information about IP for the purposes of discussing HTTP, but I highly recommend extending your education beyond this simple introduction. There are many quality books on networking, such asSams Teach Yourself TCP/IP in 24 Hoursby Joe Casad, and it is a good idea to at least have a reference available in case you requiremore detailed information at some point.



#### Domain Name System \(DNS\)

The Domain Name Systemallows for friendlier names to be associated with IP addresses. A domain name is something likehttphandbook.org. DNS is the system that primarily provides the resolution of domain names into IP addresses.

When discussing HTTP, we rarely speak of IP addresses because most URLs use domain names in order to be easier to remember. However, it is important to remember that domain names are resolved into IP addresses prior to the HTTP request being sent.

I am often asked how DNS works, because the idea that you can use a friendly name instead of an IP address makes it seem like there needs to be an authoritative DNS server \(name server\) that keeps up with all domain names and that everyone should use this one. Otherwise, it seems like there would be a risk of having a domain name resolve to different IP addresses for different people. Because there are in fact many name servers rather than just one, the entire system can seem confusing.

Thetruth is, there is an authoritative registrar, Network Solutions, Inc. \(NSI\), which keeps up with all of the registered domain names. Each domain name specifies which name server\(s\) to use for that domain name.



> **Note**
>
As Network Solutions, Inc. isno longer the only registrar, more and more domain names are being registered with alternative registrars. In these cases, the registration information, including which name server\(s\) to use, are stored with whatever registrar was used to register the domain name. An entry will be kept in NSI's master database to determine which registrar needs to be contacted for the information, so NSI still plays the role of the authoritative registrar.



Consideran example. If you want to visitwww.google.com, your computer uses a name server \(usually one your ISP assigns to you\) to resolve the IP address. The registered domain name isgoogle.com. Your local name server looks upgoogle.comin Network Solution's database to find out which name server\(s\) keep current information aboutgoogle.comand all of its subdomains, such aswww.google.com. So, while you obtain the IP address forwww.google.comfrom your local name server, it actually gets its information from another name server using the same methods your computer does. This cooperative system \(DNS\) of exchanging information about domain names is similar to the Web.



>**Note**
> 
Your local name server will likely cache the results of previous lookups for a certain period of time. For this reason, changes    to the domain's record in the authoritative name server can sometimes take a few days to propagate.



If you want to learn more about DNS, you might want to experiment with thewhoisutility. UNIX users can type the following command:

```
whois google.com 
```

Alternatively, there are Web sites that will performwhoissearches for you. For example,tryhttp://www.internic.net/whois.html.  
  


