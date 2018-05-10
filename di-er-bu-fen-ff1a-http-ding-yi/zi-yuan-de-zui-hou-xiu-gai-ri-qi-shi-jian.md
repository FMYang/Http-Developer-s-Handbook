
#### Last-Modified

The Last-Modified header contains a date in HTTP format (See Chapter 9, "Formatting Specifications," for more information about date formats). This date is used in many calculations to determine the age of a resource, especially by caching systems.

`Last-Modified: Tue, 21 May 2002 12:34:56 GMT `

The Last-Modified header can be calculated in different ways. For static content being returned by the Web server, it usually indicates the last modification date of the file, although some filesystem operations can alter the last modification date without actually altering the content. For dynamic content, it might be the current date or the date of the most recently modified dynamic part.

As a developer, you should try to ensure the accuracy of this header so that it does not mislead another Web agent. A common mistake is to consider any dynamic resource to be last modified when it is generated. If this perspective is taken, you would risk considering every resource in a dynamic application to be immediately stale, thus eliminating most of the benefits of caching. Consider the freshness of the content being returned rather than the date it was generated.

>**Note
**
An HTTP date is accurate to the second (such as Tue, 21 May 2002 12:34:56 GMT) rather than the more traditional idea of a month, day, and year only.


If a situation arises in which the calculation of the last modification date of the resource generates a date in the future, the Web agent will use the date in the HTTP request. Although the HTTP definition does not restrict the use of the Last-Modified header for resources contained in HTTP responses only, this is the only method a developer will encounter in practice.

>**Note
**
Related information on HTTP date formats can be found in Chapter 9.


#### Summary

All types of HTTP headers have now been covered, and you should have a good understanding of the communication that takes place between a Web server and a Web client. It is not essential that you recall the specific syntax and meaning of everything presented in the HTTP definition, because you can reference these chapters later as needed. As the book continues, you will learn how to apply the concepts you have learned here to your development.

The next chapter covers several of the formatting specifications that have been referenced in the last few chapters. The coverage of the HTTP definition then completes with a brief introduction to media types.