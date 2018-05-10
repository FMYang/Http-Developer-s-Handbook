#### Digest Authentication

Digest authentication mitigates the risk of exposing the username and password by utilizing a one-way cryptographic algorithm (also commonly called a hash or a message digest). These algorithms are called one-way algorithms because they are practically impossible to reverse. Although this might seem like a bold claim, consider that MD5 (Message Digest 5, a popular one-way algorithm) always returns a 128-bit digest. Thus, if you were to create a message digest of the text of this entire book, it would be 128 bits in length. If it were possible to generate the text of this entire book from a 128-bit message digest, MD5 would be an amazing compression algorithm!

>**Note
**
The fact that a message digest cannot be reversed does not mean that the original data is safe from discovery. However, the methods used to discover data from a message digest involve programmatically making many guesses, creating a message digest of each guess, and comparing the result to the original. If the result matches the original, the original data has been discovered. Two messages that have the same message digest can safely be considered equivalent.

This method of making guesses is why users are always encouraged to choose strong passwords regardless of the cryptographic techniques used to protect their passwords; the chance of a program guessing a strong password is considerably less.


The basic series of events for digest authentication is nearly identical to that of basic authentication and is illustrated in Figure 17.3.

**Figure 17.3. Digest authentication also involves two HTTP transactions.
**

The Web client makes the initial request (step 1 in Figure 17.3) for the protected resource as normal, including no authentication information, because it is initially unaware that the resource requires authentication. The authentication begins with the server's 401 Unauthorized response (step 2 in Figure 17.3), in which it includes the WWW-Authenticate response header. This header, which is explained shortly, indicates the type of authentication required as well as several other details.

The client's second request (step 3 in Figure 17.3) includes the Authorization header, which includes the message digest as well as some additional information about the authentication. Finally, the resource is returned in the second HTTP response (step 4 in Figure 17.3). This response also includes the Authentication-Info header, which completes the mutual authentication.

This process is examined in more detail in the sections that follow by focusing on the key HTTP header of each step, beginning with the WWW-Authenticate response header in the server's first HTTP response (step 2 in Figure 17.3) .

#### WWW-Authenticate Response Header

In the Web server's initial HTTP response, a status code of 401 Unauthorized indicates to the client that HTTP authentication is required. Within this response is the WWW-Authenticate response header. This header relays all information necessary for the Web client to provide the correct Authorization header in its next request.

The following example illustrates the simplest form of the WWW-Authenticate response header for digest authentication:

```
HTTP/1.1 401 Unauthorized 
WWW-Authenticate: Digest realm="HTTP Developer's Handbook", 
                  nonce="a4b8c8d7e0f6a7b2c3d2e4f5a4b7c5d2e7f" 
```

This example specifies digest authentication and includes the realm directive as well as an additional mandatory directive, nonce, which is used in the creation of the message digest. Some optional directives of the WWW-Authenticate header that are not included in this example are as follows:

* algorithm— The algorithm to be used in the creation of the digest, either md5 or md5-sess. If absent, md5 is implied. A value of md5-sess requires that the qop directive also be present. The difference between these algorithms is discussed in the explanation of the digest computation.

* domain— A space-delimited list of URLs for which this authentication applies.

* opaque— A value that, if included, is returned by the Web client in the Authorization request header of its subsequent HTTP request.

* qop— The quality of protection indicator, a list of values supported by the server. This directive may be absent, but this allocation is only to provide compatibility with older implementations. Valid values are auth, indicating authentication only, and auth-int, indicating authentication with integrity protection.

* stale— A value of true indicates that the nonce used by the Web client to generate the digest included in the Authorization header of the previous HTTP request is stale, but that the username and password are otherwise valid. Thus, a Web browser can use the new nonce included in this response to regenerate the message digest without having to prompt the user for the username and password again. A value of false indicates an invalid username and/or password.

An example HTTP response that makes use of some of the optional directives of the WWW-Authenticate header is as follows:

```
HTTP/1.1 401 Unauthorized 
WWW-Authenticate: Digest realm="HTTP Developer's Handbook", 
                  algorithm="md5-sess", 
                  qop="auth-int", 
                  nonce="a4b8c8d7e0f6a7b2c3d2e4f5a4b7c5d2e7f" 
```

The algorithm directive's value of md5-sess as well as the qop directive's value of auth-int indicate alternative calculations of the response directive in the Authorization header of the Web client's subsequent HTTP request. This calculation is explained in the next section.

#### Authorization Request Header

When the Web client issues its second HTTP request (step 3 in Figure 17.3), it includes an Authorization header that continues the process of authentication. This header contains the calculated message digest, as well as other information that helps the Web server deduce the extent of authentication being used and which method was used in the calculation of the digest.

The following is an example of an HTTP request that a client can issue after receiving the first example HTTP response given in the previous section:

```
GET / HTTP/1.1 
Host: httphandbook.org 
Authorization: Digest username="myuser", 
               realm="HTTP Developer's Handbook", 
               uri="/", 
               nonce="a4b8c8d7e0f6a7b2c3d2e4f5a4b7c5d2e7f", 
               response="47d5aaf1b20e5b3483901267a3944737" 
```

The realm and nonce are just repeated values of these directives as included in the previous HTTP response. The username is the username provided by the user and used in the generation of response, which is the message digest. The uri is the URL being requested and is identical to the URL in the request line that the Web client generates (even if it is later altered by intermediaries). Each of these directives is required, and there are optional directives that may also be included in the Authorization header. These optional directives are as follows:

* algorithm— The message digest algorithm used in the creation of the digest. Valid values are md5 and md5-sess. If absent, md5 is implied.

* cnonce— A nonce generated by the client to be used for server authentication, just as the nonce is used for client authentication. This directive is allowed, and in fact required, only if the qop directive is present.

* nc— A hexadecimal value that indicates the number of times that a Web client has issued a request using the current value of nonce. The Web server ensures that this value is sequential.

* opaque— If it is provided in the HTTP response, this is the value of the opaque directive that the Web server included in the WWW-Authenticate header.

* qop— If the server specified more than one value for the qop directive in the response (denoting that the client can choose), this directive is included to specify the client's choice. Otherwise, the server's indicated value of qop is implied, and this directive is unnecessary.

The generation of the response directive can be performed in a few different ways, depending on the options agreed upon between the Web server and Web client. The calculation is performed with the following steps:

1.Construct a string called A1. If algorithm is not specified or is md5, this string consists of the username, realm, and password, each delimited by a colon:

`<username>:<realm>:<password>`
 
If algorithm is md5-sess, MD5 is applied to this same string, then the nonce and cnonce are appended, each delimited by a colon:

`MD5(<username>:<realm>:<password>):<nonce>:<cnonce>`
 
2.Construct a string called A2. If qop is not specified or is auth, this string consists of the request method and the uri, delimited by a colon:

`<request method>:<uri> `

If qop is auth-int, this string consists of the request method, the uri, and the MD5 of the entity body, each delimited by a colon:

`<request method>:<uri>:MD5(<entity body>) `

3.If qop is not specified or is auth, construct a string consisting of the MD5 of A1, the value of nonce, and the MD5 of A2, each delimited by a colon:

`MD5(A1):<nonce>:MD5(A2)`
 
If qop is auth-int, construct a string consisting of the MD5 of A1, the value of nonce, the value of nc, the value of cnonce, the value of qop, and the MD5 of A2, each delimited by a colon:

`MD5(A1):<nonce>:<nc>:<cnonce>:<qop>:MD5(A2) `

Calculate the MD5 of the string created in step 3. This is the message digest to use in the Authorization header as the value of the response directive.

An example calculation of the response directive in PHP is as follows:

```
if ($directives["algorithm"] == "md5-sess") 
{ 
   $a1 = md5($directives["username"] . ":" . 
         $directives["realm"] . ":" . 
         $password) . ":" . 
         $directives["nonce"] . ":" . 
         $directives["cnonce"]; 
} 
else 
{ 
   $a1 = $directives["username"] . ":" . 
         $directives["realm"] . ":" . 
         $password; 
} 

if ($directives["qop"] == "auth-int") 
{ 
   $a2 = $_SERVER["REQUEST_METHOD"] . ":" . 
         $directives["uri"] . ":" . 
         md5($entity_body); 
   $message = md5($a1) . ":" . 
              $directives["nonce"] . ":" . 
              $directives["nc"] . ":" . 
              $directives["cnonce"] . ":" . 
              $directives["qop"] . ":" . 
              md5($a2); 
   $response = md5($message); 
} 
else 
{ 
   $a2 = $_SERVER["REQUEST_METHOD"] . ":" . 
         $directives["uri"]; 
   $message = md5($a1) . ":" . 
              $directives["nonce"] . ":" . 
              md5($a2); 
   $response = md5($message); 
} 
```

The use of md5-sess as the algorithm (resulting in an alternative calculation of A1 that includes additional parameters, nonce and cnonce) creates a session key that is unique for each authentication session, because the values of nonce and cnonce vary from session to session. This offers increased protection for repeat Web clients, because a profiler cannot benefit as much by analyzing accumulated data (the response directive, specifically). In addition, the use of MD5 for the username, realm, and password allows a server to perform authentication without knowing the password, as this digest can simply be appended to the current values of nonce and cnonce, each delimited by colons.

The entire entity body is used in the calculation of A2 when qop is auth-int (indicating integrity protection). This technique helps to prevent an attack similar to the one illustrated in Figure 17.4. This type of attack is possible if the content is not included in the calculation, because an attacker can simply replay the HTTP header information that performs the authentication and alter the actual content of the message as desired.

**Figure 17.4. Without integrity protection, an attacker can modify an HTTP message's content.
**

#### Authentication-Info Response Header

The final step in digest authentication (step 4 in Figure 17.3) is the Web server's transmission of the resource originally requested by the client in step 1 (and again in step 3). The HTTP response that contains this resource also contains the Authentication-Info response header, which completes the authentication.

Among other things, this header helps to ensure the identity of the Web server (by completing a mutual authentication between the client and server) so that a faked response is not a suspicion. Many of the same security benefits gained from the Authorization header included in the Web client's previous HTTP request are achieved with the inclusion of Authentication-Info in the final HTTP response.

An example HTTP response that includes the Authentication-Info header is as follows:

```
HTTP/1.1 200 OK 
Authentication-Info: qop="auth-int", 
                     rspauth="5913ebca817739aebd2655bcfb952d52", 
                     cnonce="f5e2d7c0b6a7f2e3d2c4b5a4f7e4d8c8b7a", 
                     nc="00000001" 
```

The following are the possible directives for the Authentication-Info header:

* cnonce— This is equivalent to the cnonce directive sent by the Web client.

* nc— This is equivalent to the nc directive sent by the Web client.

* nextnonce— This is a nonce value that the Web server requests and that the client uses for authentication in its next HTTP request.

* qop— This is equivalent to the qop directive sent by the Web client.

* rspauth— This is the server's equivalent of the response directive, which includes the digest calculated by the Web server based on several directives.

The calculation of rspauth can be achieved in a few ways. The most basic calculation is given in the following steps:

1.Construct a string called A1. If algorithm is md5, this string consists of the username, realm, and password, each delimited by a colon:

`<username>:<realm>:<password> `

If algorithm is md5-sess, MD5 is applied to this same string, then the nonce and cnonce are appended, each delimited by a colon:

`MD5(<username>:<realm>:<password>):<nonce>:<cnonce>`
 
2.Construct a string called A2. If qop is not specified or is auth, this string consists of a colon followed by the uri:

`:<uri> `

If qop is auth-int, this string consists of a colon followed by the uri and the MD5 of the entity body, delimited by a colon:

`:<uri>:MD5(<entity body>)` 

3.If qop is not specified or is auth, construct a string consisting of the MD5 of A1, the value of nonce, and the MD5 of A2, each delimited by a colon:

`MD5(A1):<nonce>:MD5(A2)`
 
If qop is auth-int, construct a string consisting of the MD5 of A1, the value of nonce, the value of nc, the value of cnonce, the value of qop, and the MD5 of A2, each delimited by a colon:

`MD5(A1):<nonce>:<nc>:<cnonce>:<qop>:MD5(A2) `

4.Calculate the MD5 of the string created in step 3. This is the message digest to use in the Authentication-Info header as the value of the rspauth directive.

As is evident from these steps, this calculation is exactly the same as the client's calculation except that the request method is excluded.

If you are using the Apache Web server, you can find more information about implementing digest authentication at http://httpd.apache.org/docs-2.0/mod/mod_auth_digest.html.


#### Summary

HTTP authentication is a useful tool for adding some minor protection to resources. It is especially appropriate when you want to protect a collection of static data. For example, if you have some documentation in HTML that only paying customers are authorized to access, employing HTTP authentication is one of the simplest methods of achieving this through the Web.

Although there are some risks associated with HTTP authentication, if you only allow access to the protected resources over a secure SSL connection (the topic of the next chapter), virtually all risks of either type of HTTP authentication are eliminated.

The following chapter examines Secure Sockets Layer (SSL), arguably the most popular technology used to secure HTTP transactions. The chapter begins with a brief introduction to cryptography and then explains how cryptographic techniques are integrated into HTTP transactions to offer top-notch security.