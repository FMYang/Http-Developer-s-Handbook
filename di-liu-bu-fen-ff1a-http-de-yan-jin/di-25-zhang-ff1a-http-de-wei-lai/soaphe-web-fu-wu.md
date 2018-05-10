#### SOAP and Web Services

Web services is a term that has gained much attention. It is also a term of much debate, as the term itself is very misleading and generally considered to be an inaccurate description of the idea behind it. Most people prefer to speak directly of the protocol being used, and the protocol standardized by the W3C for Web services is SOAP.

What is SOAP? SOAP stands for Simple Object Access Protocol. First, however, it is important that you are familiar with a markup language called XML, Extensible Markup Language. Whereas HTML is designed specifically for the layout of information, XML is designed for the reliable interpretation of data. Consider the following two examples.

HTML:

```
<table> 
   <tr> 
      <th>First Name</th> 
      <th>Last Name</th> 
   </tr> 
   <tr> 
      <td>Chris</td> 
      <td>Shiflett</td> 
   </tr> 
</table> 
```

XML:

```
<?xml version="1.0" encoding="iso-8859-1" ?> 
<name> 
   <first>Chris</first> 
   <last>Shiflett</last> 
</name> 
```

To appreciate the distinction, consider being responsible for reliably extracting the data from each of these documents. If you must rely on parsing the HTML, you might encounter problems as soon as the layout changes. For example, the maintainer of the document might decide to abandon the use of an HTML table, add more columns to the table, and so forth. With the XML document, the addition or rearrangment of information will not affect parsing. For example, as long as the first element remains a member of the name parent element, you can reliably obtain the first name from the document.

In practice, XML's popularity is its strength. It is far more well-defined and adhered to than HTML, and it is likely that your programming language of choice provides native support for parsing XML documents in order to return a useful data structure such as an array.

The creative idea of using XML documents as an interface to remote procedure calls (RPC) can be credited to Dave Winer. Traditionally, a remote procedure call allows a system to execute a procedure on a remote system and receive the output just as if executing it locally. When combined with the ubiquity of HTTP as a method of trans-portation and XML as a format, the possibilities of remote procedure calls became boundless. The combination of these ideas is XML-RPC, and it is the major foundation of SOAP.

The most common means of sending a SOAP request is to use the HTTP POST method. Consider the following example that is a Google search for the phrase "HTTP Developer's Handbook" using Google's API.

```
POST /search/beta2 HTTP/1.1 
Host: api.google.com 
Content-Type: text/xml 
Content-Length: 864 
SOAPAction: "urn:GoogleSearch" 
Connection: close 

<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" 
       xmlns:xsi="http://www.w3.org/1999/XMLSchema-instance" 
       xmlns:xsd="http://www.w3.org/1999/XMLSchema"> 
   <SOAP-ENV:Body> 
      <ns1:doGoogleSearch xmlns:ns1="urn:GoogleSearch" 
         SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"> 
         <key xsi:type="xsd:string">00000000000000000000000000000000</key> 
         <q xsi:type="xsd:string">"HTTP Developer's Handbook"</q> 
         <start xsi:type="xsd:int">0</start> 
         <maxResults xsi:type="xsd:int">10</maxResults> 
         <filter xsi:type="xsd:boolean">true</filter> 
         <restrict xsi:type="xsd:string"></restrict> 
         <safeSearch xsi:type="xsd:boolean">false</safeSearch> 
         <lr xsi:type="xsd:string"></lr> 
         <ie xsi:type="xsd:string">latin1</ie> 
         <oe xsi:type="xsd:string">latin1</oe> 
      </ns1:doGoogleSearch> 
   </SOAP-ENV:Body> 
</SOAP-ENV:Envelope> 
```

>**Note
**
This example actually requires a key, designated by the key element in the SOAP envelope. A fake one is given here, and you can register for a free key at http://www.google.com/apis/.

Although this example does include the HTTP header SOAPAction, which is not part of the HTTP specification, notice that the major distinction between a SOAP message and a typical HTTP message is the content. Thus, for the most part, SOAP defines the structure of the content and adds very little to HTTP itself.

Google responds to this request with the following HTTP response (SOAP envelope body omitted for brevity):

```
HTTP/1.1 200 OK 
Date: Tue, 21 May 2002 12:34:56 GMT 
Server: e h c a p a 
Content-Length: 4887 
Connection: close 
Content-Type: text/xml; charset=utf-8 

<?xml version='1.0' encoding='UTF-8'?> 
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" 
                   xmlns:xsi="http://www.w3.org/1999/XMLSchema-instance" 
                   xmlns:xsd="http://www.w3.org/1999/XMLSchema"> 
<SOAP-ENV:Body> 
... 
</SOAP-ENV:Body> 
</SOAP-ENV:Envelope> 
```

Because most Web scripting languages provide native support for both SOAP and HTTP POST operations, Web developers can build useful services into their own applications. This example demonstrates the underlying communication required to interact with Google's API. The construction of the SOAP envelope, as well as the parsing of the SOAP response, can be done manually, and indeed you can try this example by issuing the command telnet api.google.com 80 and pasting in this example request (with a valid key). However, most programming languages (even those not native to the Web environment) provide native support for SOAP, so this work is probably trivial.

>**Note
**
For more information about SOAP, see the World Wide Web Consortium's site at http://www.w3.org/.