#### Chapter 8. Entity Headers

Entity headers relay information about the content of an HTTP message, although the specification does cite that they can also be used to relay information about the resource identified in an HTTP request. They can be used in either HTTP requests or responses, but because HTTP requests often contain no content, they also often lack entity headers as well.

These headers relay specific information about the content within an HTTP message and are intended to help the receiving Web agent properly handle the message. The following sections describe each of the entity headers defined in HTTP/1.1.