#### Privacy and Security Concerns with Cookies

Cookies have become a source of privacy concern in recent years. As with most technologies in the computer industry, this reputation has been earned by the misuse of the technology more than the technology itself. As noted earlier, many Web browsers have the use of cookies enabled by default (without user warnings), and many people have taken advantage of this situation by profiling customer tendencies, collecting unnecessary personal information, and so on. The semantics of cookies are fairly well designed for the task they are intended to accomplish. The abuse, however, has resulted in cookies having a rather negative connotation.

The most common misuses of cookies are as follows:

* User profiling with the intention of targeting advertisements (as opposed to profiling customers in order to provide more useful services within your own application)

* Including personal or sensitive data in cookies that unnecessarily exposes the user to security risks

The first misuse is primarily accomplished through the use of banner advertisements. Participating Web sites will include an image from a third-party Web site, as shown in Figure 11.4. Because images are collected by the Web browser separately after receiving HTML that references them, the Web browser actually makes a GET request for a URL located on the third-party's Web server. This allows this third-party Web server to read and set cookies. If the URL for the image (in the HTML) also includes additional information in the query string, the third party can track other information about a user's browsing habits.

**Figure 11.4. A third-party Web site serves an advertisement.
**

With the restrictions that can be placed on cookies, such as the secure attribute, some developers place too much confidence in their security. If cookies are trusted with personal or sensitive data, the user is exposed to software vulnerabilities and other risks that can be easily avoided by storing this type of information securely on the server.

For example, Microsoft Internet Explorer (version 4.0 through version 5.0) has a security vulnerability that allows any Web site to read and write cookies from any domain, even secure cookies (cookies possessing the secure attribute). This vulnerability was originally revealed at the following URL:

http://www.peacefire.org/security/iecookies/

In addition, Microsoft Internet Explorer (version 5.5 through version 6.0) possesses a similar vulnerability with regard to cookies that allows even easier access to cookies from any domain. This vulnerability was originally revealed at the following URL:

http://www.solutions.fi/iebug/

With the popularity of Microsoft Internet Explorer combined with the tendency of people to not keep current with security patches, these two vulnerabilities alone yield a staggering amount of Web browsers that have major security vulnerabilities with regard to cookies. This situation creates two risks:

Web applications that rely on cookies alone for state management are at risk for presentation attacks (sometimes called replay attacks), where a legitimate user's cookies are read by an imposter and presented in a subsequent HTTP request.

Personal or sensitive data that is stored in cookies is at risk of being read by a criminal.

The first risk can be mitigated with a more intelligent state-management mechanism that does not place too much trust in the value of the cookie being presented. Imagine, for example, that you utilize two cookies for state management, one for the unique identifier and one that you use for extra verification. For example:

`unique_id=123456789 cv=181b9b77ed9a54699a5619a2a4d3c661`

In this example, the unique_id cookie would be the typical cookie used to identify the client. Can you guess what the cv cookie means? Likely you cannot, and that is the point. It is an MD5 sum of the following string:

`SECRETPADDINGMozilla/5.0 Galeon/1.2.6 (X11; Linux i686; U;) Gecko/20020916 `

This string consists of the user agent prepended with a secret string, SECRETPADDING. This prevents an attacker from guessing the algorithm used (MD5 is quite common) and simply reconstructing this value based on his/her own user agent as a guess. If you check to ensure that the value in the cv cookie matches the result of performing this same calculation on the current user agent, you can at least force an attacker to be using the same Web browser. This can complicate an impersonation.

Unfortunately, this technique still has weaknesses. The most problematic weakness is that most cookie vulnerabilities involve the victim visiting an attacker's Web site. Thus, the attacker has access to all of the same information about the legitimate user as you do on your Web site. Using this information, the attacker can fake a request (perhaps by telnetting to your Web site and manually typing in an HTTP request, for example) and use the same HTTP headers as the legitimate user would. Although this type of attack is slightly more advanced than a simple presentation attack, it is not as uncommon as it may seem.

A stronger approach typically involves the use of a combination of state-management methods, which is a topic that I cover in the next chapter. The purpose of combination approaches is generally to require an attacker to eavesdrop on the actual HTTP request sent by the client rather than just exploit a browser's vulnerability. When SSL (Secure Sockets Layer, a topic covered in Chapter 18) is also used to encrypt the HTTP transactions, as well as ensure the identity of the Web agents, this can make an attack extremely difficult and provide your users with very strong security.

The second risk, the risk of personal or sensitive data being read by a criminal, can be mitigated by not including personal or sensitive data in cookies at all. You should respect your users enough to protect their personal and sensitive information to the best of your ability, including avoiding exposure over the Internet. As discussed previously in this chapter, client data is much more secure when kept on the server rather than being transmitted over the Internet unnecessarily. The only information necessary for state management is information used to identify the Web client.

#### Summary

Cookies are a point of frustration for many Web developers, and this chapter should give you a much better understanding of cookies as well as the challenges involved in maintaining state.

Most importantly, being more aware of the actual HTTP communication gives you important insight into how cookies are implemented. This alone gives you a great advantage over other developers in situations where you must resolve technical problems related to the use of cookies or explain their use to others.

The following chapter expands on the topics introduced here by presenting additional methods of state management. I also discuss the use of combination methods to defend against certain types of attacks.
