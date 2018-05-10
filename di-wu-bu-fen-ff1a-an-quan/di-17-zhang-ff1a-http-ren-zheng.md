#### Chapter 17. Authentication with HTTP

Authentication refers to the process by which you verify the identity of a user to a reasonable extent. This generally consists of a user providing a username for identification and a password to verify that identity.

HTTP offers integrated mechanisms for authenticating users. Collectively referred to as HTTP authentication, these mechanisms provide a way for users to be authenticated without the necessity of any server-side programming logic. This can be especially helpful for restricting access to static resources (such as images or HTML files). Of course, server-side scripts can also implement HTTP authentication, although Web developers often authenticate users in the application logic itself.

There are two basic types of HTTP authentication:

* Basic authentication

* Digest authentication

This chapter discusses both. There are security risks associated with basic authentication, and there are compatibility concerns associated with digest authentication. Thus, the most appropriate solution depends on your specific needs. The following sections discuss each of these types of authentication and further explain the advantages and disadvantages of each approach.

>**Note
**
The official specification for HTTP authentication is RFC 2617, "HTTP Authentication: Basic and Digest Access Authentication."