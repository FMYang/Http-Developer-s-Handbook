
#### Keep-Alive

The Keep-Alive header is very loosely defined in the original HTTP/1.1 definition (RFC 2068), which has been obsoleted by a newer document (RFC 2616). The definition makes no declarations about the values that are allowed for this header, but it does require that the Connection general header have a value of Keep-Alive.

In practice, some browsers are known to transmit the Keep-Alive header in the following format:

`Keep-Alive: 300 `

Although there is no definitive reference for how this is to be interpreted, the value is given in seconds, and the meaning is intended to be interpreted as a maximum amount of time that the TCP connection can remain open.