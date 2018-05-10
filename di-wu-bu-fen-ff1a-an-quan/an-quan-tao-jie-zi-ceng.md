#### Chapter 18. Secure Sockets Layer

When the Web was used primarily for the exchange of public information, there was little concern about the security of the information being exchanged. Most information was intended to be public, and the Web helped to make information more attainable.

As the Web began to be used to exchange more sensitive data, a method was needed to protect the HTTP messages being exchanged. One key characteristic of HTTP that you may have noticed thus far is that the messages are sent in the clear, meaning that anyone who can view these messages can easily interpret them. Even HTTP authentication fails to protect the messages themselves from eavesdropping.

This is especially troubling when you consider how client data is communicated back to the server. Whether the data is appended to the URL, sent in a Cookie header, or included in the content section of a POST request, it is clearly visible to anyone who can view the message.

A common analogy used to describe the insecurity of email is a postcard. You have no guarantee that someone else did not read the postcard prior to its arrival. In addition, you have no guarantee that someone else did not alter the writing on the postcard or even forge the identity of the sender, possibly using a previous postcard as an example of the sender's handwriting and signature. HTTP carries these same risks.

Consider the following HTTP request passed in clear text:

```
POST /search HTTP/1.1 
Host: 127.0.0.1 
User-Agent: Mozilla/5.0 Galeon/1.2.5 (X11; Linux i686; U;) Gecko/20020606 
Connection: keep-alive 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 71 

credit_card_num=1234567890123456&exp_date=2006-05&name=Chris%20Shiflett 
```

If this were my real credit card information, anyone who sees this message can potentially make unauthorized purchases with my card. Because HTTP requests are usually sent across the public Internet, this is a real danger.

An elegant solution to these types of problems is SSL, Secure Sockets Layer. In 1994, Netscape released the specification of Secure Sockets Layer. By 1995, version 3.0 of SSL was released, and it has since taken the Web by storm. SSL has dramatically changed the way people use the Web, and it provides a very good solution to many of the Web's shortcomings, most importantly:

* Data integrity— SSL can help ensure that data (HTTP messages) cannot be changed while in transit.

* Data confidentiality— SSL provides strong cryptographic techniques used to encrypt HTTP messages.

* Identification— SSL can offer reasonable assurance as to the identity of a Web server. It can also be used to validate the identity of a client, but this is less common.

Before I explain the implementation of SSL in conjunction with HTTP transactions, it is first necessary to give a brief introduction to two types of cryptography that closely relate to SSL.

