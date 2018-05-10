#### Denial of Service

Denial of service (DoS) attacks are less common than media reports may lead you to believe, because they generally require substantial hardware resources to perform adequately. However, some DoS attacks focus on the applications themselves and are easier to accomplish.

In general, there are two types of DoS attacks, network and application. The most popular DoS attacks mentioned in the media are network attacks, often characterized by attempts to flood a particular network with traffic. Application attacks are those that attack a specific application.

#### Network DoS

Most people are more familiar with DoS attacks that focus on flooding a network. There are utilities that exist to help people launch a DoS attack of this nature. However, this type of attack requires that the victim network be smaller than the attacking network in terms of resources. For example, the Linux cluster that powers Google would easily be able to launch a denial of service attack on a small Web site because of its power and available bandwidth. However, because qualified professionals maintain most large-scale networks, these types of attacks are difficult to achieve.

A closely related attack is DDoS, distributed denial of service. This type of attack overcomes the necessity of abundant resources by using many computer systems to launch the attack. This is usually performed by exploiting a software vulnerability on many public systems to install software that coordinates the attack. An example of this is the Code Red worm that was said to be designed to launch a DDoS on the whitehouse.gov domain on a particular date. Due to the staggering number of infected Microsoft IIS servers on the Internet, this was a real threat. It was luckily discovered that the worm based its attack on an IP address, so the domain was pointed to a new address, and traffic destined for the old IP was dropped.

>**Note
**
An innocent example of a DDoS attack is when a particular Web site receives media attention and attracts more visitors than it can support.

This is a common situation on the popular news site Slashdot, because the stories generally include links to related material, and the staggering number of visitors can quickly overwhelm a moderately sized Web server. This has lead to the common use of the term slashdot effect to describe such an event.


Network administrators and operations managers are generally responsible for securing a network and defending against these types of attacks. Even with a properly configured network, however, your applications might still be vulnerable to attack.

#### Application DoS

In order to appreciate the possibility of an application-based DoS attack, consider the following hypothetical situation:

* An attacker can request a resource from your application at the rate of 10 requests per second.

* Each request initiates programming logic that requires three seconds to execute.

It should be clear that consistent requests for the resource in this scenario could lead to a depletion of computing resources. With this type of attack, it is not necessary that the attacker utilize an environment with superior resources, because the required work by your environment is far greater. This characteristic makes these types of attacks much more convenient and therefore far more common. In addition, prevention of this type of attack is generally the responsibility of the Web developer.

The key characteristic that you want to avoid is having computationally complex scripts available to an anonymous Web client. For example, many DoS attacks utilize a Web application's login page as a platform, because sometimes the complexity involved in determining whether the user has been successfully authenticated can cause too much load on the server if too many such requests are received. An attacker can automate the login process easily enough and launch a process that continues indefinitely.

One method for preventing application-based DoS attacks is to test your applications under extremely heavy load for long periods of time to identify bottlenecks. Not only can this help you improve the performance of your applications, it can also help you to protect your applications against DoS attacks.

In many cases, the security requirements of your application may require that you employ sophisticated cryptography, sometimes on every request received. In these cases, a technique can be used to limit the frequency of requests allowed from a single client within a window of time. This technique is called throttling. A polite request that the user pause before trying again will be enough to ensure that legitimate users are not wrongfully punished.

>**Note
**
Apache users can utilize mod_throttle to provide throttling. See http://www.snert.com/Software/mod_throttle/ for more information.


More information about DoS attacks can be found at http://www.cert.org/tech_tips/denial_of_service.html.