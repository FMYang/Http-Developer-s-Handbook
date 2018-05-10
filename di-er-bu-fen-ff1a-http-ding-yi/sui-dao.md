#### Connection

TheConnectionheader providesthe primary method for connection management in HTTP. There are two values that this header usually uses:

```
Connection: Keep-Alive 
Connection: Close 
```

Because a connection can only be open or closed, these two values, when used in HTTP requests and HTTP responses, allow for the handling of all possible scenarios within an HTTP transaction.

One of the most important distinctions between HTTP/1.0 and HTTP/1.1 is how connections are treated. In both versions, persistent connections are supported. However, with HTTP/1.0, persistent connections were not the default behavior, so the use of aConnection: Keep-Aliveheader requesting persistent connections had to be used. With HTTP/1.1, persistent connections are the default behavior, so theConnection:Closeheader is used to indicate when the connection is to be closed after the completion of the current transaction.

Another form of theConnectionheader used is within requests for a protocol upgrade and the corresponding responses. The form used in these cases is as follows:

```
Connection: Upgrade 
```

Note

Related material can be found in the description of theUpgradegeneral header later in this chapter.

