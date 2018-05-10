#### Exposure

The Internet is a public medium for communication. Although there are core networks that make up the backbone, the majority of the Internet can be considered a cooperative effort of smaller, shared networks. This type of environment poses a serious risk of exposure to any data that is being transmitted in the clear.

Many utilities exist that can assist an attacker in sniffing traffic (a term that refers to the discovery nature of observing raw data as it travels across a network), and many people use these utilities for recreational purposes (much like people use scanners to snoop cellular phone conversations for entertainment).

The best rule of thumb is to consider all data being transmitted to and from your application (the HTTP requests and HTTP responses, respectively) to be public. For most cases, this is probably not a threat, because the information itself might be public. However, sensitive information must be guarded.

In many cases, you may also want to prevent your HTTP responses from revealing information about your software via headers such as Server. This type of information can help a potential attacker to launch attacks intended to exploit a specific vulnerability in your software. For this reason, many people deliberately alter the Server header to misrepresent the software their server is using.

The simplest way to protect your application from exposure is to employ SSL. This can ensure that each HTTP message is safe from unauthorized observation. However, the performance impact of SSL may be unacceptable, and an alternative means might be necessary.

You should take care to protect your application against presentation attacks while considering methods to protect sensitive data from exposure. For example, do not let the fact that a specific piece of information is encrypted cause you to necessarily assume that the encrypted information originated from the legitimate user.

#### Summary

There are many resources available for educating yourself further about common types of attacks. It is essential that you familiarize yourself with the latest trends in security attacks so that you can maintain an educated perspective with regard to your application design. These techniques are constantly evolving as attackers get more creative and more experienced.

The most important point to learn from this chapter is to avoid making assumptions. This tendency lies at the heart of almost all mistakes with regard to security on the Web. By taking a speculative approach to your design, and by considering every detail with a fresh perspective, you will find that you can make better decisions and create more secure applications.

This chapter completes the part of the book on security. The following chapter introduces some important standards organizations, and the book then completes with a chapter that discusses some future trends regarding the Web.