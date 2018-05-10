#### Applying Cryptography to HTTP

SSL is basically a protocol that employs both symmetric and asymmetric cryptography to protect messages that use TCP as the transport-level protocol. Because of the high performance expense of asymmetric cryptography, it is only used to exchange the randomly generated symmetric key that is then used for the symmetric encryption of the HTTP messages. Figure 18.5 illustrates this point. The same symmetric key is used as long as the TCP connection remains open.

**Figure 18.5. SSL utilizes both symmetric and asymmetric cryptography.
**

When used to protect Web communication, SSL's position in the protocol stack is just between TCP and HTTP, as illustrated by Figure 18.6.

**Figure 18.6. SSL operates between TCP and HTTP.
**

Whenever a Web browser connects to a Web site over a secure connection, it requires that the SSL certificate the Web server presents meets three main conditions:

* The domain name on the certificate must match the domain name the Web browser believes itself to be requesting a resource from.

* The certificate must be valid (not expired).

* The certificate must be signed by a trusted certificate authority (CA).

If any of these conditions are not met, the Web browser will issue a warning similar to the one shown in Figure 18.7.

**Figure 18.7. A Web browser will warn if an SSL certificate does not meet all required conditions.
**

Most Web browsers will attempt to explain which condition(s) the SSL certificate fails to meet. For example, in Figure 18.7, the domain name being requested is different from the domain name presented in the certificate. Web browsers vary widely in the way they present this information, but most are quite clear if you take the time to read the warnings (which you should), and you can also view the certificate in question.

>**Note
**
Many users interpret SSL warnings to be some type of error.


Figure 18.8 illustrates an HTTP transaction that uses SSL to encrypt the messages. The exchanges prior to the HTTP request and response are often called the SSL handshake, alluding to the agreement nature of SSL. This is similar in nature to the three-way handshake of TCP mentioned in Chapter 3, "HTTP Transactions." As explained earlier, SSL operates between TCP and HTTP. Thus, the SSL handshake takes place once a TCP connection has been established between the Web client and Web server and before the initial HTTP request is sent.

**Figure 18.8. SSL requires the exchange of several messages.
**

The exchanges illustrated in Figure 18.8 assume a typical SSL handshake where only the server is authenticated. Because most users of the Web do not possess personal digital certificates, SSL is very rarely used to authenticate the client. However, applications that employ client authentication can boast greater security, especially concerning the identity of the client.

>**Note
**
Due to the major differences in initiating SSL compared to normal Web transactions, a different default port of 443 is assigned to SSL traffic. Thus, a different listener is required for standard (port 80) traffic and SSL (port 443) traffic, although many Web servers can manage both using separate child processes.


To denote the difference in protocol, the scheme of a URL requested over a secure SSL connection is https rather than http. This is a very important point that applies directly to the submission of HTML forms. If the action of a form is a URL that uses the http scheme, all data sent from the Web client will not gain the benefits of SSL. The form must be submitted to an https URL in order to be protected.

>**Note
**
Although it is only required that a form be submitted to a secure URL in order for the data to be protected, it is a common practice to make the HTML form itself a secure URL. This is so that the lock icon (See Figure 18.9) appears to the user while the form is being filled out. This gives important assurance to a user who relies on this icon to determine whether information is submitted securely, even though it really offers no assurance.

**Figure 18.9. A Web browser will display a lock icon when the user is visiting a secure URL.
**

Unfortunately, because of this practice, a common pitfall is to use a secure URL only for the form itself and submit the data to an insecure URL. This exposes the user's data and creates a dangerous security hole in your application.

The presence of the lock icon depends on the URL of the current resource (displayed in the browser's location bar). This is why no lock icon is shown on a Web site that uses frames if the parent is not a secure resource, even if the child frames are secure.