#### Chapter 20. Secure HTTP

Unlike SSL/TLS, Secure HTTP is rarely implemented. However, it offers some interesting differences and is more tightly integrated with the HTTP protocol itself. This chapter provides a basic overview of Secure HTTP as well as some example transactions that employ it for security. It is unlikely that you will need to utilize Secure HTTP, but studying alternative security mechanisms can sometimes grant you a much better perspective with which to make important decisions regarding security.

The most notable characteristic of Secure HTTP is that a separate port is unnecessary. This characteristic lies at the heart of the differences between Secure HTTP and SSL/TLS. Secure HTTP uses a more conventional approach while still integrating strong cryptography.

This chapter first examines the basic structure of a Secure HTTP request and how to construct an HTML link that uses Secure HTTP. It then provides a brief overview of the technical details.