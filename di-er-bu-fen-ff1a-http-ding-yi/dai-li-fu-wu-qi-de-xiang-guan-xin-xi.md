
#### Via

The Via header provides a method for intermediate proxies to identify themselves without otherwise altering the HTTP messages. Because the definition requires proxies to add a Via header (or add their name to an existing one) as they forward messages, this information can be relied upon to be complete.

In addition to adding their names to the Via header, proxies also identify the version of HTTP they are using, which can be very helpful when identifying problems in an HTTP transaction.

The TRACE request method is often used to inspect the Via header for more information about a transaction. Figure 5.1 (shown on page 51) shows a complete HTTP transaction involving two intermediate proxies.

>**Note
**
Related material can be found in the description of the TRACE request method in Chapter 5, "HTTP Requests."