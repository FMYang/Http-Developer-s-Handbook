#### Combinations

When speaking about using combinations with regard to state management, I refer to:

* The use of multiple variable types (URL variables, form variables, and cookies)

* The use of multiple identifiers (multiple variables) from the client to verify identification

It is sometimes appropriate to use combination methods of maintaining state. Whether you enforce that each method actually be used depends on your needs. For example, you might want to allow either a cookie or a URL variable to be used to communicate the unique identifier, so the users who prefer not to use cookies can have a unique identifier appended to each URL, whereas those who prefer cookies will not have anything passed on the URL. Alternatively, you may want to require that both methods be used, only maintaining state for those that provide both types of data, so that potential attackers have more obstacles to overcome.

The most common combination method for state management is the use of cookies combined with URL variables. Using the aforementioned distinction, allowing either cookies or URL variables is more of a convenience for the user, being otherwise nearly identical functionally (for example, referencing $_COOKIE["unique_id"] versus $_GET["unique_id"] in PHP).

Alternatively, requiring a combination demands a bit more work but is perceived by many to be the most secure method of maintaining state without compromising too much user-friendliness (there is a trade-off). The idea is basically to maintain similar data in the cookie and in the URL but to use a different format for each, granting yourself a way to validate the unique identifier while making it extremely difficult to guess a value for one if given the other.

This latter approach can help to guard against browser vulnerabilities and other security holes that can lead to the disclosure of information within cookies without requiring that someone actually eavesdrop on the HTTP messages (the risk of eavesdropping can be mitigated with SSL—See Chapter 18, "Secure Sockets Layer"). The reason for using different formats for the data is so that compromising one does not exclude the requirement for an attacker to compromise the other. Additionally, if one is encrypted using a message digest algorithm such as MD5, the contents cannot be determined, and a presentation attack (replay attack) is the only risk. See Chapter 23, "Common Attacks and Solutions," for more applicable information on presentation attacks.

Although the unique identifier is all that is necessary to maintain state, additional data can also be used in order to further strengthen the security of your state-management mechanism. For example, the value of the User-Agent request header can be added so that attackers trying to present someone else's data must identify themselves as using the same Web browser. Obviously, an attacker can satisfy this extra check, and the value of the User-Agent request header is not any more reliable than other data sent from the client. However, every extra step that a potential attacker must undergo strengthens your security.

If it is less obvious which credentials are being checked, an attack becomes even more difficult. For example, you can encode the unique identifier and additional data to be supplied as the value of a single parameter. Not only does this make it more difficult to distinguish the unique identifier from the other data, but it also makes it more difficult to determine which credentials are being checked for each request.