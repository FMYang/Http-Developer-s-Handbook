#### Content-MD5

The Content-MD5 header is a very useful indicator for ensuring that the resource has arrived safely. The MD5 (Message Digest 5) algorithm is an extremely popular message digest algorithm that most developers are probably familiar with. It is sometimes referred to as a one-way algorithm, alluding to the fact that the digest cannot be converted back into the original document. This characteristic is due to the MD5 algorithm always producing a 128-bit (16-byte) result, regardless of the size of the input.

To include a Content-MD5 header in an HTTP message, the Web agent must calculate the MD5 sum of the content part of the message and convert it to a Base-64 format (see RFC 2045 for more information on Base-64). The reason for this is that an MD5 sum's 128 bits are not necessarily going to be capable of being displayed as 16 printable ASCII characters (it would be quite unlikely, actually). Rather than use the ASCII characters that represent the sum in hexadecimal (such as 1a to represent 26), the value is Base-64 encoded so that it can be accurately printed in ASCII without making the interpretation ambiguous.

`Content-MD5: ZTFmZDA5MDYyYTMzZGQzMDMxMmIxMjc4YThhNTMyM2I=`
 
This is an example of my name, Chris Shiflett, after it has been passed through both MD5 and Base-64 encoding.

The receiving Web agent will check the integrity of the resource by performing the same operations on the content once it has received the HTTP message.