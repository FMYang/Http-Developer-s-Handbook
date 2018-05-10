#### Presentation Attacks

One extremely common type of attack is the presentation attack, often referred to as a replay attack. This type of attack generally involves an attacker posing as a legitimate user of your application by presenting information previously stolen from that user. These types of attacks are made possible by designs that only consider the legitimate uses of the application, which is why you should avoid this approach.

Many presentation attacks use the Cookie header to present someone else's cookies. Although SSL can be used to protect cookies in transit, browser vulnerabilities exist that can allow a user's cookies to be read by an unauthorized site. Consider the following scenario:

User logs in to site A.

Site A sets a cookie called user_id with an encrypted value IZZHRTCTSGHTKLIQFRWGGA.

User visits site B.

Site B exploits a browser vulnerability and reads cookie user_id.

Attacker from site B visits site A with cookie: user_id=IZZHRTCTSGHTKLIQFRWGGA.

>**Note
**
There are several methods that can be used to gain access to the cookie in step 4, including cross-site scripting attacks, which are discussed in the next section.


Even though the attacker cannot decrypt the cookie, this step is not necessary. Site A will decrypt the cookie, so all the attacker has to do is obtain a valid user's encrypted cookie and present it to site A.

>**Note
**
All versions of Internet Explorer from 4.0 to 6.0 have vulnerabilities that allow unauthorized access to your cookies. It is suggested that you either install the latest security patches or use an alternative Web browser. It may also be appropriate to warn your users who have vulnerable browsers if your application depends on cookies for state management.


There are many attacks similar to the scenario just described, and these often prey on the erroneous assumption that preventing potential attackers from discovering data (for example, by encrypting the value of the cookie) keeps them from initiating a presentation attack with that data. Because a resourceful attacker will take the easiest route, you should take care not to focus too much on one area, because you can be surprised by a technique that you are not expecting.

Defending against presentation attacks only requires that you never trust data from the client. For example, consider the following HTTP request:

```
GET / HTTP/1.1 
Host: httphandbook.org 
Cookie: user_id=IZZHRTCTSGHTKLIQFRWGGA 
```

If the user_id cookie only contains a user's unique identifier (whether encrypted or not), there is nothing in this request that can help to verify that this user is who he/she claims to be. Thus, you are forced to trust this data from the client in order to maintain state. With such a simple method of state management, trusting data from the client is inevitable, and this opens the door for presentation attacks.

>**Note
**
The state management mechanism is often the most vulnerable part of a Web application. Review Chapter 12, "Other Methods of State Management," for some suggestions on creating a secure state management mechanism.