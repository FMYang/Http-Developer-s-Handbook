#### Never Trust Data from the Client

This is truly the golden rule of Web development, and it is likely the rule most commonly broken by inexperienced developers. Most developers who violate this rule do not actually realize that they are trusting the client for anything. Adhering to this rule requires that you truly understand what you are trusting and why, because it is easy to unintentionally trust data that can compromise your application. Now that you have a better understanding of HTTP and how the Web operates, you should be able to easily identify where data originates and how it travels across the Internet.

The most common example of data sent from the client is the HTTP request resulting from an HTML form submission. Consider the following example:

```
<form action="/form_receive.php" method="post"> 
<p> 
First Name: <input type="text" name="first_name" maxlength="15"> 
<br> 
Favorite Color: 
<select name="favorite_color"> 
   <option value="red">red</option> 
   <option value="green">green</option> 
   <option value="blue">blue</option> 
   <option value="other">other</option> 
</select> 
<br> 
Gender: 
<input type="radio" name="gender" value="male"> Male 
<input type="radio" name="gender" value="female">Female 
</p> 
<p><input type="submit" value="Validate"></p> 
</form> 
```

This HTML is rendered in a browser to create the form illustrated in Figure 22.1.

**Figure 22.1. An HTML form is rendered by a browser.
**

Each of these form fields has restrictions placed on it. The name that a user enters cannot exceed 15 characters. The favorite color can only be red, green, blue, or other. Finally, the gender can only be male or female. It may seem like these restrictions are trustworthy, especially because you are the one who generated the HTML form, but unfortunately this is not the case.

As a simple example of how these safeguards can be removed, consider that the previous page is located at http://httphandbook.org/form.html and submits data to http://httphandbook.org/form_receive.php. Imagine the following HTML, written by an attacker:

```
<form action="http://httphandbook.org/form_receive.php" method="post"> 
<p> 
First Name: <input type="text" name="first_name"> 
<br> 
Favorite Color: 
<select name="favorite_color"> 
   <option value="yellow">yellow</option> 
</select> 
<br> 
Gender: 
<input type="radio" name="gender" value="none"> None 
</p> 
<p><input type="submit" value="Validate"></p> 
</form>
``` 

This form submits to exactly the same resource as the previous form (by using an absolute URL), and the resulting HTTP request is indistinguishable from an HTTP request generated from the authentic form. This rogue form has removed the length restriction on the name and offers unexpected values for both favorite color and gender. Although this example shows a relatively harmless attack, it demonstrates how easily such restrictions can be removed and why you should never place any trust in them.

When you consider that someone can send their own HTTP requests, you can see that attacks can be much more sophisticated, as this offers an attacker complete flexibility in the type of data they send. Once an attacker uses your Web application legitimately and receives your HTML form, he has all of the information needed to launch an attack. You should always treat all data coming from the client as data that can truly be anything.

Another common pitfall is to rely on client-side scripting (such as javascript) to validate incoming data. Obviously, this approach assumes that the data will be submitted from the expected HTML form and originate from a Web client capable of executing the client-side logic. Although client-side validation is often considered to be user-friendly, it is never a substitute for proper server-side validation. Disabling this type of validation is as simple as disabling the corresponding client-side scripting, although the previous techniques of using a rogue HTML form or forging an HTTP request can be used instead for greater flexibility.

All data should be validated on the server (using server-side logic such as PHP) to be sure it is in an acceptable format. For example, when validating the name submitted in the form, you might want to only allow alphabetic characters, spaces, hyphens, and apostrophes. Other characters can also be allowed depending on the types of names you expect.

The idea is to create a "white list" as opposed to attempting to make a "black list." This means that you decide exactly what characters are allowed, and you reject everything else. This is a much safer approach than trying to reject specific characters. The danger is that you may miss rejecting a dangerous character only to discover your mistake after your application is compromised.

By ensuring that only safe characters are allowed, your concern becomes making sure there is no way an attacker can avoid your data validation. The previous chapter mentioned some software architectures that can help in this regard, and there are also some naming conventions you can adhere to in order to help strengthen this assurance.

When you have finished validating data, you can assign each variable to a variable of another name that is reserved. For example, I often use a prefix of clean_ to denote a validated variable. Thus, immediately after determining that the variable favorite_color is acceptable, I can rename it to clean_favorite_color. This makes it easy to identify data that has been validated and to distinguish it from data that is potentially dangerous.

The one danger to this approach is the user submitting data by the reserved name. For example, an attacker familiar with this method might attempt to submit a form variable named clean_favorite_color. If you do not handle this possibility explicitly, you may unexpectedly assume that this data has been validated when it has not.

To protect against this potential problem, it is absolutely essential that you reject all data beginning with clean_ originating from the client (if you choose this naming convention), and this must be done globally. This is one of the many reasons why a strong software architecture will use a technique that guarantees that server-side security logic executes prior to any other. The previous chapter mentions the use of a parent script that includes all necessary modules for the application, or a security module that is included in all scripts in the application can be used instead.