#### Secure HTTP Requests

A Secure HTTP request is basically a typical HTTP request that uses a special request method, Secure, and includes the original HTTP request in encrypted format as the content. Secure HTTP can be visualized as a wrapper for HTTP, as illustrated in Figure 20.1.

**Figure 20.1. Secure HTTP is a wrapper for HTTP.
**

The content of the Secure HTTP message (the original HTTP message) is protected with a cryptographic protocol. This is usually CMS, Cryptographic Message Syntax. Consider the following HTTP request:

`GET / HTTP/1.1 `
`Host: httphandbook.org `

An example Secure HTTP request that protects this request is as follows (content shown in decrypted format for convenience):

```
Secure * Secure-HTTP/1.4 
Content-Type: message/http 
Content-Privacy-Domain: CMS 

GET / HTTP/1.1 
Security-Scheme: S-HTTP/1.4 
Host: httphandbook.org 
```

Because the content is encrypted, the request offers very little information to anyone who may be snooping:

```
Secure * Secure-HTTP/1.4 
Content-Type: message/http 
Content-Privacy-Domain: CMS 

(content is encrypted) 
```

Because the requested URL can potentially contain sensitive information, a Secure HTTP request omits the resource and replaces it with an asterisk. However, in order to allow for the use of a proxy, a Secure HTTP message can also specify the host in the request line, as illustrated in the following example:

```
Secure http://httphandbook.org/* Secure-HTTP/1.4 
Content-Type: message/http 
Content-Privacy-Domain: CMS 

(content is encrypted) 
```

In this example, the relative path is still replaced with an asterisk, and in fact, the Secure HTTP message that the proxy forwards will be identical to the first example. The two headers shown in these examples let the Web server know that the content is an HTTP message and that it is protected with CMS, because Secure HTTP also supports a less popular format, MIME Object Security Services. There are also two other Secure HTTP headers defined for requests, Prearranged-Key-Info and MAC-Info.

The Prearranged-Key-Info header identifies keys that have been previously exchanged between the client and server. This header allows a method directive that specifies either inband or outband. A method of inband indicates that the key was previously included in a Key-Assign HTTP header, whereas a method of outband indicates that the method of key exchange lies outside of the scope of Secure HTTP. This flexibility would be helpful, for example, if the keys were exchanged through physical correspondence, which might be the case between a specific client and server.

The MAC-Info Secure HTTP header allows for a message authentication code to accompany the request. This allows the Web server the ability to ensure the integrity of the encrypted HTTP request using the strengths of MAC. A MAC is basically a mechanism that provides an integrity check based on a secret key. This is similar to the function of a message digest as discussed in Chapter 17, "Authentication with HTTP," except that message digests do not utilize a secret key.

The Key-Assign HTTP header just mentioned is one of several HTTP headers allowed by Secure-HTTP. This means that the HTTP message itself (which is the encrypted cargo of the Secure HTTP message) can have HTTP headers not defined under HTTP/1.1 that are used exclusively for Secure HTTP. These additional headers are as follows:

* Certificate-Info— This header specifies a digital certificate to be used to communicate and verify a public key to be used in public key cryptography.

* Encryption-Identity— This header identifies a recipient for whom a message could be encrypted.

* Key-Assign— This header allows for the exchange of a symmetric key to be used in future Secure HTTP transactions.

* Nonce— This header specifies a value to use in cryptographic operations so that presentation attacks are made much more difficult.

* Nonce-Echo— This header simply proves a way for the Web agent to return a nonce provided in a previous HTTP message's Nonce header.

* Security-Scheme— This header is required and should identify a scheme of S-HTTP/1.4.