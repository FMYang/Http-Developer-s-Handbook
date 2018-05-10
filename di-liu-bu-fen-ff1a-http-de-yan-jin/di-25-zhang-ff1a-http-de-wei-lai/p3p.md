#### P3P

P3P, Platform for Privacy Preferences, is a standard created by the W3C to allow users more control over their personal information. It allows an automated way for a user's privacy preferences and a Web site's privacy policy to be compared for agreement so that users can gain more control over the use of their personal information without having to make redundant decisions at every Web site.

P3P essentially defines two standards:

* A standard format for specifying a privacy policy

* A discovery method for locating a privacy policy

Privacy policies are defined in XML documents called policy statements. A good example of a policy statement is one of the W3C's policy statements:

```
<?xml version="1.0"?> 
<POLICIES xmlns="http://www.w3.org/2002/01/P3Pv1"> 
 <EXPIRY max-age="604800"/> 
 <POLICY name="public" 
         discuri="http://www.w3.org/Consortium/Legal/privacy-statement#Public"> 
  <ENTITY> 
   <DATA-GROUP> 
    <DATA ref="#business.name">World Wide Web Consortium</DATA> 
    <DATA ref="#business.contact-info.postal.name">MIT/LCS</DATA> 
    ... 
   </DATA-GROUP> 
  </ENTITY> 
  <ACCESS><nonident/></ACCESS> 
  <DISPUTES-GROUP> 
   <DISPUTES resolution-type="service" service="http://www.w3.org/" 
   short-description="site-policy@w3.org"> 
    <LONG-DESCRIPTION> 
     The Webmaster and our Communications Team will carefully consider 
     the input and correct errors. If you discover privacy invasive 
     behavior, please don't hesitate to contact us. 
    </LONG-DESCRIPTION> 
    <IMG src="http://www.w3.org/Icons/WWW/w3c_home" width="72" 
         height="48" alt="Logo World Wide Web Consortium"/> 
    <REMEDIES><correct/></REMEDIES> 
   </DISPUTES> 
  </DISPUTES-GROUP> 
  <STATEMENT> 
   <CONSEQUENCE> 
    We collect normal Web-Logs. They are used for Server administration, 
    Web protocol research, Statistics of usage and Security. 
   </CONSEQUENCE> 
   <PURPOSE><current/><admin/><develop/></PURPOSE> 
   <RECIPIENT><ours/></RECIPIENT> 
   <RETENTION><indefinitely/></RETENTION> 
   <DATA-GROUP> 
    <DATA ref="#dynamic.clickstream"/> 
    <DATA ref="#dynamic.http.useragent"/> 
    <DATA ref="#dynamic.http.referer"/> 
   </DATA-GROUP> 
  </STATEMENT> 
 </POLICY> 
</POLICIES> 
```

A Web site can have many policy statements such as this, and each one is identified in a policy reference file. This reference file is one piece of the discovery mechanism defined by P3P. The W3C's policy reference file is as follows:

```
<META xmlns="http://www.w3.org/2002/01/P3Pv1"> 
  <POLICY-REFERENCES> 
    <EXPIRY max-age="172800"/> 
    <POLICY-REF about="/2001/05/P3P/public.xml#public"> 
      <INCLUDE>/*</INCLUDE> 
      <EXCLUDE>/Member/*</EXCLUDE> 
      ... 
      <METHOD>GET</METHOD> 
      <METHOD>HEAD</METHOD> 
      <METHOD>PUT</METHOD> 
    </POLICY-REF> 
    <POLICY-REF about="/2001/05/P3P/member.xml#member"> 
      <INCLUDE>/Member/*</INCLUDE> 
      ... 
      <METHOD>GET</METHOD> 
      <METHOD>HEAD</METHOD> 
    </POLICY-REF> 
    <POLICY-REF about="/2001/05/P3P/member.xml#member"> 
      <INCLUDE>/*</INCLUDE> 
      <METHOD>PUT</METHOD> 
      <METHOD>DELETE</METHOD> 
    </POLICY-REF> 
    <POLICY-REF about="/2001/05/P3P/telecon.xml#bridge"> 
      <INCLUDE>/1998/12/bridge/*</INCLUDE> 
    </POLICY-REF> 
  </POLICY-REFERENCES> 
</META> 
```

This reference file references four policy statements. Each one is identified in a POLICY-REF tag. The example policy statement is the first one identified in this reference file, denoted by the opening tag:

`<POLICY-REF about="/2001/05/P3P/public.xml#public"> `

The discovery process requires one more piece of information—the location of the reference file for a Web site. The specification identifies three methods that can be used for this:

* Place it in /w3c/p3p.xml.

* Reference it in an HTTP header.

* Place it in an HTML link.

The specification recommends placing the privacy policy at the location /w3c/p3p.xml if possible. In some cases, this will not be possible, as there may be multiple Web sites that share the same document root. The following HTTP transaction illustrates the use of the HTTP header P3P for specifying the location of the reference file:

```
HEAD / HTTP/1.1 
Host: www.w3.org 

HTTP/1.1 200 OK 
Date: Tue, 21 May 2002 12:34:56 GMT 
Server: Apache/1.3.26 (Unix) PHP/3.0.18 
P3P: policyref="http://www.w3.org/2001/05/P3P/p3p.xml" 
Cache-Control: max-age=600 
Accept-Ranges: bytes 
Content-Length: 20069 
Content-Type: text/html; charset=us-ascii 
```

A Web client that supports P3P will use the URL identified in the P3P header to locate the reference file and then the appropriate privacy statement(s). The early support for P3P in Microsoft Internet Explorer 6.0 has generated a great deal of momentum with regard to its adoption. There are many Web sites already conforming to the P3P standards. If you want to create a P3P-compliant Web site, there are a few resources to help you:

* http://www.w3.org/P3P/develop.html— This is a description of the tasks a developer must complete to conform to P3P.

* http://www.w3.org/P3P/validator.html— This is a helpful utility that can validate a policy statement for proper format or perform a complete validation of your Web site.

>**Note
**
For more information about P3P, see http://www.w3.org/P3P/.


#### Summary

The ubiquity of HTTP makes it a popular choice for exchanging information on the Internet, and although the Web will likely remain the focus of its use, new and exciting technologies will inevitably continue to utilize HTTP's strengths.

Although technically an application-level protocol, HTTP is often used as a transport protocol, as this chapter demonstrates. New technologies such as the three described here basically implement their own protocol on top of HTTP. Although it is doubtful that you will ever see the term HTTP/TCP/IP, future endeavors on the Internet that use HTTP as a transport protocol will serve to strengthen its importance. As firewalls that only allow TCP/IP traffic destined for ports 80 or 443 continue to appear, more and more functionality will likely be integrated into HTTP.
