#### Certificate Authorities

As it turns out, there is no easy solution to the problem of identification. In order to assure that a particular public key belongs to a particular person (or domain name, for example, httphandbook.org), a certificate authority (CA) is used. A certificate authority is a trusted third party that assures the identity of a public key's owner with a digital certificate. A digital certificate is a document that declares that a particular public key is owned by a particular Web site (see Figure 18.3). The CA's role is very similar to a notary whose responsibility is to ensure the correct identity of people signing a legal document.

**Figure 18.3. A digital certificate assures the identity of a public key's owner.
**

The digital certificates used in SSL are commonly called SSL certificates. In order to purchase an SSL certificate from a CA, you must go through a process that verifies that you are the rightful owner of the domain you want to protect and also ensures that the public key to be used in the certificate belongs to your Web server.

An essential part of this process is the generation of the Certificate Signing Request, CSR. This is a digital file that you must create with the Web server destined to receive the SSL certificate. The CSR contains your Web server's public key, among other things. This is a standard file that is requested by all major CAs prior to generating an SSL certificate.

A guide to creating a CSR for Apache can be found at http://httpd.apache.org/docs-2.0/ssl/ssl_faq.html#realcert. If you are using a different Web server, consult your Web server's documentation.

You may be wondering why it is necessary to buy an SSL certificate from a company such as Thawte or VeriSign when it is possible to create your own. The short answer is that such companies are trusted by default by all of the major Web browsers, including Mozilla, Netscape, and Internet Explorer.

The more accurate answer is that these companies have digital certificates that are included in the trusted root certificate store of the major Web browsers. Most browsers allow you to view and manage these CA certificates, as Figure 18.4 illustrates. In order to be included in this certificate store, a CA must prove that its operation is secure and reliable by undergoing an extensive Certification and Accreditation (C&A) process. This process is typically very expensive, and this is one of the reasons that you must pay for an SSL certificate that is going to be trusted by default. However, the C&A process lends a great deal of strength to SSL.

**Figure 18.4. You can view and manage your Web browser's trusted root certificates.
**

The SSL certificates that you can purchase from a CA are signed by one of these root certificates. When the root CA certificates are used to issue an SSL certificate, there are two important characteristics that may be given:

* The certificate can be used as an SSL certificate.

* The certificate can be used as an SSL certificate and can also be used to issue other SSL certificates on behalf of the CA, thereby acting as a remote authority (RA).

Thus, a CA may grant another entity permission to issue SSL certificates on its behalf. A Web browser will follow the resulting chain of trust back to the root certificate in order to verify an SSL certificate.

An unfortunate vulnerability exists in Internet Explorer versions 5.0 through 6.0 regarding the chain of trust. Vulnerable browsers do not check to ensure that intermediate certificates are granted the right to issue SSL certificates.

Thus, anyone with a valid SSL certificate issued by a trusted CA may issue a valid SSL certificate for any domain they choose, and a vulnerable browser will not even warn the user when this certificate is used by a Web site. More information about this vulnerability can be found at http://www.thoughtcrime.org/ie-ssl-chain.txt. It is important to note that this vulnerability only exists due to an error in the implementation of SSL and not in SSL itself.

>**Note
**
Public key cryptography is also quite popular for encrypting email. In general, certificate authorities are not used in these cases, because public keys can be distributed in person or on a business card, for example, so that you are assured of the identity of its owner. The Web is less personal than email, thus the need for CAs.

