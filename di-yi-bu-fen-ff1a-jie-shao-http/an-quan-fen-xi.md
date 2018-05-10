#### Analyzing Security

It has become a fairly common assumption that security is only a concern for people such as network and systems administrators. This is due to the idea that the Internet is a battlefield, and these people are responsible for defending your fortress against outside attacks. Thus, an application running in a secure environment is considered to be a secure application.

Unfortunately, this could not be further from the truth. Security is something that has to be everyone's responsibility. Because an attacker will focus on the weakest link, it is important to identify the weak links in any system and make sure to mitigate the weaknesses as much as possible. As a Web developer, your focus should be to create a secure application in terms of the programming techniques you use. In fact, this is arguably the most important factor in creating a secure Web application, because Web applications are exposed to the general public by their very nature. This gives casual users plenty of opportunity to analyze the behavior of your application prior to attempting an attack.

Chapters 17-23 contain a great deal of information that can help you strengthen the security of your Web site. In general, however, there is one general rule that every Web developer should always adhere to—never trust data from the client.

This guideline is considered by some to be the golden rule of Web development. From what you have learned so far, it should be clear how easily it is to generate your own HTTP request (as illustrated in Figure 4.2). You should never assume that the request received from a client is trustworthy or even the result of some action taken on a Web page you previously sent.

For example, if your Web pages include HTML restrictions on the length of form fields by utilizing the maxlength attribute, you should not assume anything about the length of the fields received and still verify that they are of a valid length. This same philosophy applies to any client-side scripting that tries to ensure that form data adheres to a specific format. Although client-side data validation can add to user convenience by avoiding unnecessary HTTP transactions, you should never depend on this technique to ensure the data is valid. To do so would be to trust the user in a way analogous to a teacher trusting students to grade their own work. If the user is a potential attacker, the danger of this unfortunately common practice is clear.

Security is often viewed as a discrete attribute of an application rather than something that can be measured qualitatively. For example, many people believe an application is either secure or insecure. Many times the business requirements for something specify security as a requirement. Thus, it becomes the responsibility of the developer to not only define and quantify that requirement, but also to provide it.

One of the most crucial topics in security is the security of information. Although some people focus on attacks on a network or system, it is likely that such attacks are the result of the attacker gaining key information that aids in circumventing the security measures put into place.

Without knowing what data is being sent and received from your Web application, you cannot hope to adequately secure that data or even assess whether it needs securing. There are many common mechanisms within HTTP that allow you to restrict access and encrypt data, and I will also cover more programming techniques and software architectures that help you maintain confidence in the security of the data you use in sensitive areas of your application.

#### Summary

This should give you a few common techniques that you can apply to your Web development as well as a few thoughts to keep in mind as you continue. Understanding HTTP in a general sense is very beneficial in providing a broad perspective with which to approach problems.

The next six chapters build on this general understanding and go into the details of the HTTP protocol, focusing on syntax and definitions. These chapters should be considered a reference, and they are organized to allow you to locate information easily.

