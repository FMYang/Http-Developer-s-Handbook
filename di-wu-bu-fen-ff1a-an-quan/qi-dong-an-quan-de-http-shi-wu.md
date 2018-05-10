#### Initiating a Secure HTTP Transaction

Because Secure HTTP does not require the multiple messages that an SSL handshake requires, initiating Secure HTTP from an HTML link is much more complex. Whereas an SSL link is simply an ordinary link that begins with https:// rather than http://, a Secure HTTP link includes a great deal of information required for the negotiation of cryptographic options. The following gives an example HTML link:

```
<a href="shttp://httphandbook.org/" 
   dn="CN=Shiflett CA, O=HTTP, Inc., C=US" 
   cryptopts="SHTTP-Privacy-Domains: recv-optional=MOSS, CMS; 
                 orig-required=CMS 
              SHTTP-Certificate-Types: recv-optional=X.509; 
                 orig-required=X.509 
              SHTTP-Key-Exchange-Algorithms: recv-required=DH; 
                 orig-optional=Inband,DH 
              SHTTP-Signature-Algorithms: orig-required=NIST-DSS; 
                 recv-required=NIST-DSS 
              SHTTP-Privacy-Enhancements: orig-required=sign; 
                 orig-optional=encrypt"> 
```

The href attribute specifies the URL of the resource, which is identified with the shttp:// scheme. The dn attribute is the distinguished name. The distinguished name in the previous examples consists of three elements: the common name (CN), the organization name (O), and the country name (C). The semantics of a distinguished name are described in RFC 1779. The cryptopts attribute specifies all of the cryptographic information needed to perform the necessary cryptography. The options for cryptopts are as follows:

* SHTTP-Certificate-Types— Indicates the format of digital certificates.

* SHTTP-Cryptopts— Indicates cryptographic options used.

* SHTTP-Key-Exchange-Algorithms— Indicates the algorithm to use to exchange the symmetric keys.

* SHTTP-Message-Digest-Algorithms— Indicates the message digest algorithm to employ.

* SHTTP-Privacy-Enhancements— Indicates privacy enhancements to be used.

* SHTTP-Privacy-Domains— Indicates the message format specified in the Secure HTTP messages with the Content-Privacy-Domain header.

* SHTTP-Signature-Algorithms— Indicates the cryptographic algorithm used to sign messages.

* SHTTP-Symmetric-Content-Algorithms— Indicates the cryptographic algorithm used to encrypt the messages.

* SHTTP-Symmetric-Header-Algorithms— Indicates the cryptographic algorithm used to encrypt the message headers.

In addition to the anchor tag just described, a digital certificate accompanies this link within a separate HTML tag. It is denoted in the common Base-64 format. The following example illustrates the basic form of this tag:

```
<certs fmt="pkcs-7"> 
certificate 
</certs> 
```

This example shows the HTML tags used to designate a Base-64 encoded PKCS-7 certificate.

Now that I have given an overview of the syntax of Secure HTTP transactions and how they are initiated, I will briefly discuss the most common message format used in Secure HTTP messages, CMS.

