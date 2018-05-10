#### Chapter 6. HTTP Responses

Once a Web server has received an HTTP request from the client, whether valid or not, it will send an HTTP response. HTTP has allocations for handling many types of error conditions as well as other unique situations.

It is important to remember that an HTTP response completes the HTTP transaction. Many people new to Web development have a difficult time distinguishing between server-side code (code that executes on the server) and client-side code (code that executes on the client). Scripting languages such as PHP, ColdFusion, and JSP are executed on the server, and their output is included in the HTTP response. In fact, their output is the content of the HTTP response, and most modern Web scripting languages also allow for some manipulation of the HTTP as well, such as altering or adding headers, changing status codes, and so on. Once the Web client receives the HTTP response, the transaction is complete. The Web client will then render the page, execute client-side scripts such as JavaScript, load images (by issuing separate GET requests), and so on.

With HTTP/1.1, persistent connections are the default behavior. This means that the Web server will not close the connection after sending the HTTP response unless the client intends to close the connection after receiving it. In this case, the client will include the following header in the HTTP request:

`Connection: close`

Alternatively, the server can close the connection upon sending the HTTP response, although it should be polite and include the same header as shown previously so that the Web client expects this action.

As you are developing your applications, it is often more convenient to ignore the additional HTTP transactions for images and other embedded resources, focusing solely on the transactions that affect the flow of logic. This approach can be very helpful for focusing on larger issues in your design. Be very careful with abstracting the details of the operation of the Web, however, and make sure that you do so only for convenience.

