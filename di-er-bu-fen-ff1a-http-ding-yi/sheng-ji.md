#### Upgrade

The Upgrade header provides a method for the Web server and Web client to negotiate a change in protocol. The typical negotiation takes place with the client requesting the upgrade in protocol in the HTTP request, such as the following example request:

```
GET / HTTP/1.1 
Host: 127.0.0.1 
Upgrade: TLS/1.0 
Connection: Upgrade 
```

The Web server, as discussed in the section on status codes, will use a 101 Switching Protocols status code in the HTTP response to indicate a willingness to upgrade. Along with this, it also includes an Upgrade header and the same value for the Connection general header as used in the HTTP request:

```
HTTP/1.1 101 Switching Protocols 
Upgrade: TLS/1.0, HTTP/1.1 
Connection: Upgrade 
```

After the Web client receives a successful response such as this, all further communication on the current connection will use the new protocol.

The multiple values in the Web server's response acknowledges the multiple protocols being used at the application layer. Because TLS utilizes HTTP as a means of transportation (much like HTTP uses TCP), the Web server lists both protocols in a comma-delimited list, specifying versions when appropriate.

>**Note
**
Related material can be found in the description of the 101 Switching Protocols status code in Chapter 6.