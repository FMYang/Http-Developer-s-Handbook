#### Virtual Hosting

Virtual hosting presents a challenging situation in conjunction with SSL. Consider the following HTTP request:

`GET / HTTP/1.1 `
`Host: httphandbook.org `

If a Web server receiving this request serves many domains using the same IP address, it can still choose the correct host, because it can parse the Host header to determine which host is being contacted. However, consider the same request using SSL. Prior to receiving the encrypted HTTP request, the Web server must negotiate the SSL handshake with the client. Part of this process requires the Web server to present the SSL certificate to be used in the public key cryptography. How can the Web server choose the correct SSL certificate for httphandbook.org if it has not yet received the request with the Host header? The answer is that there must be something uniquely identifiable in the TCP/IP layer. This typically means that each host must have either a unique IP address or a unique port.

When each host has a unique IP address, the Web server can present the correct SSL certificate by identifying which host corresponds with the destination IP address of the client's communication. This technique is called IP-based virtual hosting. Because the destination IP address is likely public, this approach can be unrealistic for some environments due to the expense of having a separate public IP address for each host.

Unique ports per host can be used to help avoid using public IP addresses in this way because the port can be used to distinguish hosts, allowing for a single public IP address. However, URLs with ports may seem cumbersome or unprofessional. For example, consider the URL https://httphandbook.org:4887/ as opposed to https://httphandbook.org/.

In practice, administrators will often use name-based virtual hosts anyway, which forces the Web server to present the SSL certificate corresponding to the default host when performing the SSL handshake. Although this will cause a warning to appear for users connecting to any host except the default, this might be acceptable in some cases.

If you cannot afford for the user to experience SSL warnings (or the decrease in security when such a warning must be expected), and you do not want the cumbersome URLs that specify the port, you should use IP-based virtual hosting.