#### The World Wide Web 

As mentionedbriefly in[Chapter 1](itss://chm/0672324547_ch01.html#ch01), the Web consists of three key ideas working together: the Web protocol HTTP, the Web naming and addressing standard URI, and the Web data format HTML. There is much more that makes the Web possible, of course. As with most modern breakthroughs, the Web stands on the shoulders of previous technologies such as the Internet. It can be said that the popularity of the Web also owes much to the revolution of the personal computer. After all, it is the fact that so many people have computers and are connected to the Internet that makes the growth of the Web possible.

#### The Relationship Between the Internet and the Web

The Internet providesthe backbone of the Web. Because the Web involves the exchange of information between computers, the Internet provides the perfect medium for it.

The Web requires the ability to locate content, and the combination of IP addresses and DNS to locate a specific host helps make that possible. URLs use domain names or IP addresses—along with several other pieces of information such as resource paths—to specify a unique resource on the Internet.

The Internet also provides an existing infrastructure for communication with the addressing and routing characteristics of IPnetworking.

#### How the Web Works

When you type a URL into your browser or click on a link, several events occur. If the URL contains a domain name rather than an IP address for the host, this domain name is first resolved into an IP address using DNS. Next, a connection is made to that IP address \(I explain connections in the next chapter\). With a connection established, an HTTP request is sent to the Web server from your browser. This request is delivered as per any other message on the Internet. Once the request is received, it is interpreted by the Web server, and an HTTP response is sent from the Web server back to your browser. The content of the HTTP response is rendered by yourbrowser, and the Web page is displayed.

