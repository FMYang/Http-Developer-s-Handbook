#### Web Servers and Clients

One of the most obvious applications of HTTP is the creation of Web servers and clients. This is software such as the Mozilla Web browser (HTTP client) and the Apache Web server (HTTP server). When creating software that handles the HTTP communication, strict adherence to the standard becomes essential so that your software can be compatible (can interoperate) with all other existing software. Common lore will tell you to be strict in what you send and lenient in what you accept. This approach will give your software the greatest chance of being compatible with other HTTP agents.

Professional Web browsers and Web servers are not the only types of HTTP applications. Lightweight Web clients are quite common and also require strict adherence to HTTP. A Web spider is an example of such an application. A spider's duty is to interact with Web servers by following links in the HTML to gather vital information used to index portions of the Web.

In addition to this, there are many situations in which automating the behavior of a Web client can provide useful functionality for a Web application. For example, many Web sites have improved on the idea of linking to other related sites by actually syndicating the content of those sites so that the related content is integrated for the users on one convenient page. The most popular method for accomplishing this feat is RSS, Rich Site Summary.

RSS is basically a standard format that provides a summary of your site's content. It is an XML-compliant document whose definition is located at http://my.netscape.com/publish/formats/rss-0.9.1.dtd (the most common version used).

>**Note
**
The latest version of RSS can be found at http://purl.org/rss/1.0/spec and is referred to as RDF Site Summary, because it conforms to the W3C's RDF specification.


XML, Extensible Markup Language, is similar to HTML, except that its purpose is to make information easily distributed among computer systems rather than to provide formatting for information.

>**Note
**
For more information about XML and RSS, see Applied XML Solutions by Benoit Marchal, published by Sams Publishing.


Most Web scripting languages provide a way to automate common HTTP tasks such as **GET** and **POST**. For example, PHP allows you to open a URL (using **GET**) just as if it were a file by using the **fopen()** function. If you need to send a **POST** request instead, you can utilize the **Net_Curl** class from PEAR (http://pear.php.net/package-info.php?pacid=30). ColdFusion provides the `<cfhttp>` tag, which supports **GET** and **POST**.

**Figure 4.1** shows how a Web server can syndicate content from another Web server. Once the HTTP request is received from the Web client, the generation of the response includes programming (such as the techniques mentioned in the previous paragraph) to obtain content from the second Web server. Unlike embedding images from remote servers in a Web page, this technique involves only a single transaction.

**Figure 4.1. A Web server syndicates content from another server.
**

There are many possibilities related to this technique. If you realize that a Web server can play the role of a Web client in communication with another server by sending its own HTTP request, you will likely be able to generate many creative ideas of your own.


