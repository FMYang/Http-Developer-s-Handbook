#### Software Architecture

The characteristics of intelligent hardware architecture are the same characteristics that you should focus on when designing your software. There are many principles that apply to every Web scripting language and application environment, and in fact, an experienced Web developer can apply these principles to any Web application.

#### Reliability

Reliability is more difficult to achieve with software than hardware simply because an error in your application cannot be resolved with redundancy; it can only be resolved with a fix. Thus, reliability with regard to software architecture is achieved by employing a design that results in the fewest possible errors.

The focus of your software design should always be to use the simplest and most elegant approach. An overly complicated approach is more prone to contain errors, as it is more complicated and confusing to any developer. One common method of keeping things simple is to use the Unix philosophy—do one thing and do it well. When applied to Web development, this typically involves a modular design, where the application is broken into pieces. Each module focuses on a specific task. For example, consider the following pseudocode for a login script:

```
authenticate user 
if login is valid 
     begin session 
     display success page 
else 
     display failure page
```
 
Although this procedure can easily be achieved with a single script, it might be easier to break it into three. Separate scripts can be used to authenticate the user, begin the session, display the success page, and display the failure page. By doing this, it is easier for you (and/or other developers) to focus on one specific task at a time, it is easier to divide tasks among a group of developers, and it keeps each module extremely simple and less prone to be confusing to anyone.

Another characteristic of this particular example is that using modules as just described helps separate presentation from logic. If the modules for authenticating the user and beginning the session output nothing, they can be used anywhere that these tasks are necessary without modification. In addition, many Web sites will undergo many changes in presentation, and combining presentation with logic poses the risk of creating errors in the logic when the only changes intended deal with the presentation. In order to build on this approach, consider a parent template that decides which of these modules to execute. Figure 21.6 illustrates this approach.

**Figure 21.6. A parent template can be used to include or execute the appropriate modules.
**

Using a parent template in your design may help in many areas, most notably security and session management. In addition, it gives you a perfect place to get an overview of the logical flow of the associated modules. By hiding the complexities of the specific modules, you can focus on issues of a broader scope.

#### Performance

There is generally very little to be gained in terms of performance from a well-designed application. If you use the approach in the previous section of applying the simplest and most elegant solution to your design, it is likely that you will also create a very efficient application in the process. However, there are a few characteristics worth avoiding that can diminish the performance potential and scalability of your application.

One particularly common pitfall for Web developers is to generate unnecessary traffic. Using the example login scenario from the previous section, consider the following steps as an alternative:

```
authenticate user 
if login is valid 
     goto success page 
else 
     goto failure page 
```

Although the user experience may be exactly the same, the extra HTTP transactions necessary to retrieve the success or failure page effectively double your traffic. Thus, this approach halves the capacity of your application. The goto in this example can be either a meta refresh or a protocol-level redirect involving the Location response header. Instead of generating this unnecessary traffic by using either of these methods, the modular approach mentioned in the previous section allows you to simply include the appropriate markup (HTML) in your initial response, avoiding the second HTTP transaction.

A similar pitfall briefly mentioned in Chapter 14, "Leveraging HTTP to Enhance Performance," is to exclude the trailing slash of a directory, forcing two HTTP transactions to occur rather than one. This mistake is common in HTML links. Consider the following link:

`<a href="http://httphandbook.org/dir">Click Here</a>`
 
If dir is in fact a directory, a user who clicks this link will generate two HTTP transactions. The first request will be for http://httphandbook.org/dir, and the second request will be for http://httphandbook.org/dir/. This is because the Web server responds to the first request with a 301 Moved Permanently response that directs the Web browser to the proper URL.

If the link simply includes the proper URL (complete with the trailing slash), only one HTTP transaction is necessary.

#### Security

Security is arguably the most important characteristic of software architecture as well as the characteristic that can gain the most improvement from a well-designed application.

Using a parent template as previously described is highly recommended. If this technique cannot be used, an alternative is to include a common module at the beginning of every script accessible from a URL. For example, consider Figure 21.7.

**Figure 21.7. An alternative to a parent template is to include a common module in all scripts.
**

When this method is used, it still allows you to wrap every available resource with protective logic that guards against many types of attacks.

Thus, either in a common module (security.inc in Figure 21.7) or at the beginning of a parent template, you can implement session logic, check the names and types of incoming data, and take any other protective measures that you can implement globally (all with server-side logic, of course). This approach ensures that certain safeguards cannot be avoided.

>**Note
**
Chapter 22, "Programming Practices," further explores software design, explaining several key guidelines that can be used in combination with the architectures described here.

#### Summary

The architecture of a Web application is an important step in the development process. There are many example architectures that can be found on the Web. Some are language-specific, and some are not. However, there are a few key characteristics that you should try to achieve:

* Consistency— By implementing the same overall design in several applications, you will help to improve on your methods, which can help improve both present and past applications. In addition, this approach helps team members stay better organized and allows them to focus on the programming rather than learning a new design for every application.

* Simplicity— Although most example architectures are very detailed and complex, this approach is not always best. Many applications are built with very little thought given to the software architecture, and this is likely due to the fact that many sample architectures are too complex and would slow development. In order to be helpful, an architecture should be easy and intuitive for Web developers to employ, and it should allow a certain amount of flexibility, so that it can be applied to multiple applications. A simple design is better than no design.

The following chapter builds on the ideas mentioned here by introducing several programming practices that can help to improve the reliability, performance, and security of your applications.
