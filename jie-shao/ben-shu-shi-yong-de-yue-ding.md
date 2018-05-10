There are many terms for Web content these days. There are home pages, Web pages, Web sites, Web applications, and even Web services. With the common tendency to combine two related words into one, this at least doubles the number of terms in use.

In writing this book, I use the terms that I am accustomed to using and that I believe are accurate and descriptive. I describe them here for your reference. These are not given as formal and absolute definitions but rather a guideline to how these terms are used in this book.

* **Home page**—An informal term for a personal Web page, or the intended entry page of a Web site.

* **Web page**—The rendered output of a single URL.

* **Web site**—A collection of Web pages.

* **Web application**—An application with a Web interface.

* **Web service**—A service offered over the Internet.

* **Browse**—The activity of visiting Web pages.

* **Surf**—Informal term used by some people instead of browse.

  ```
   **Note**

   The word Web is capitalized when used as an abbreviation for the World Wide Web.
  ```

A Web page can be a single page of a Web application, so it is not necessarily static content. A Web application can be referred to as a Web site in this manner, because it can be described as a collection of dynamic Web pages. However, a Web site does not necessarily have to be a Web application; a Web site can be a collection of static Web pages. In use, the Web prefix is sometimes omitted when it is clear that the discussion is about Web content.

Another important distinction is between static content and dynamic content. I will attempt to distinguish these in cases where it is significant, such as a static Web page versus a dynamic Web page. However, most of the principles of HTTP apply to all content, so this should be of little concern. Static content is best explained as a resource that the Web server sends to the client without any manipulation, such as an image or an HTML file. Dynamic content, on the other hand, is usually content that is created at the moment the user requests the page. To give an analogy, static content is similar to a television show, whereas dynamic content is more like an interactive video game.

In order to help you visualize many of the principles described in this book, there are many figures depicting various situations. Although every attempt is made to make these figures intuitive, I will list the meaning of a few of the images here.

**Note**

In most figures, a simple situation is illustrated so that the focus is not lost in the complexity. In cases where your Web serving environment is a multi-tiered environment consisting of many servers, you should consider the entire Web serving environment to be equivalent to the Web server shown in the figures.

**Figure In.1. Client \(computer with the Web browser\).**

**Figure In.2. Server \(computer with the Web server, or any other type of server\).**

**Figure In.3. An HTTP message.**

**Figure In.4. The Internet.**

**Figure In.5. Example HTTP transaction.**

The following conventions are used in the format of the text.

**fixed-width font**—Denotes computer code or key words in messages such as the HTTP headers.

**fixed-width italicized font**—Denotes computer code where you may use any valid name or value you choose. For example,**Content-Type:** **text/html **denotes that **text/html **can be substituted with any valid value.

