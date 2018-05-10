#### Cryptographic Message Syntax

Cryptographic Message Syntax, CMS, is basically a message format for cryptographically securing data. Secure HTTP integrates the CMS format with standard HTTP to achieve much of its strength. The CMS specification is RFC 2630, and this specification is a good resource for exploring the format further.

Secure HTTP uses two of the available content types in CMS:

`EnvelopedData` 
`SignedData`

**EnvelopedData** is basically encrypted data that is contained within a wrapper. The data itself is encrypted using symmetric key cryptography. Although the specification allows for several methods of distributing the symmetric key, the most popular method (as with SSL) is asymmetric cryptography, most often called public key cryptography. The envelope contains all digital certificates necessary for the cryptography as well as all certificates necessary to create a chain of trust from a root Certificate Authority for the public key cryptography (See Chapter 18, "Secure Sockets Layer," for more about CAs and the issue of trust). This usually includes at least the digital certificate containing the Web server's public key, which is the logical equivalent of an SSL certificate.

**SignedData** is a message digest that has been signed with the server's public key. The public key and corresponding certificate are contained within the message.

Secure HTTP uses each of these content types by first generating SignedData from the original HTTP message and then encrypting it to produce EnvelopedData. The result is used as the content of the Secure HTTP message.

#### Summary

Although most Web developers will not need to take more than an academic examination of Secure HTTP, the techniques used can be very helpful in establishing your own security practices. If you want to learn more about Secure HTTP, the only good resource is RFC 2660. The lack of alternative documentation is the likely result of the lack of popularity. The domination of SSL has all but eliminated any alternative security mechanisms in practical use.

The following chapter examines various architectures you can employ as you design and build your applications as well as when you design and build the environment to host those applications.