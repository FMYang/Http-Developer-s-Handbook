#### Chapter 12. Other Methods of State Management

In Chapter 5, I discussed the GET and POST request methods. A Web client can utilize either of these methods to send data to the Web server. With a GET request, a client can send data by utilizing the query string of the URL. With a POST request, the client can utilize the query string and can also include data in the content section of the HTTP request.

To demonstrate how a GET request can send data by utilizing the query string of a URL, consider the following link:

`<a href="/example.php?unique_id=12345">Click Here</a>`
 
When a user clicks this link, the user's Web client will issue a GET request similar to the following:

`GET /example.php?unique_id=12345 HTTP/1.1`
`Host: httphandbook.org` 

In this example, unique_id is a variable that the example.php script has access to. For example, this variable can be referenced as $_GET["unique_id"] in PHP. This type of variable is called a URL variable, although many developers prefer the term GET variable.

To demonstrate a POST request, consider the following HTML form:

```
<form action="/example.php?unique_id=12345" method="post"> 
<p>Color: <input type="text" name="color"></p> 
<p><input type="submit" value="Submit"></p> 
</form> 
```

Figure 12.1 illustrates how this form looks in a browser.

**Figure 12.1. A browser renders a simple HTML form.
**

If a user types red in the text field and then clicks the Submit button, an HTTP request similar to the following is sent:

```
POST /example.php?unique_id=12345 HTTP/1.1 
Host: httphandbook.org 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 9 

color=red 
```

In this example, there are two variables, unique_id and color. As in the previous example, unique_id is a URL variable. The other variable, color, is called a form variable. Many developers refer to form variables as POST variables. In PHP, this variable can be referenced as $_POST["color"]. This example illustrates how both types of variables can be used together.

Finally, consider this HTML form:

```
<form action="/example.php" method="get"> 
<p>Color: <input type="text" name="color"></p> 
<p><input type="submit" value="Submit"></p> 
</form> 
```

This form looks identical to the one in the previous example when viewed in a browser. However, if the user again types red in the text field and then clicks the Submit button, the HTTP request looks quite different:

`GET /example.php?color=red HTTP/1.1` 
`Host: httphandbook.org`
 
In this example, color is a URL variable. Thus, even when an HTML form is used to query the user for the data, the browser sends the data as URL variables when the form method is GET.

>**Note
**
If the URL specified in a <form> tag includes URL variables and the form method is also GET, many browsers will only include the variables from the HTML form in the query string. This has been known to cause frustration for a few Web developers, as it results in the loss of all URL variables specified in the URL of the <form> tag. If you have data that you want the browser to include as URL variables when the form method is GET, you can include that data as hidden form variables.

In the previous chapter, I explained cookies, an additional way for the Web client to send data to the Web server that is intended specifically for the purpose of maintaining state. Because maintaining state requires that the Web client send a unique identifier with each HTTP request, you can utilize any one of these three methods, as well as various combinations of these methods, to maintain state:

* Form variables— The unique identifier is sent in the content section of an HTTP POST request.

* URL variables— The unique identifier is included in the query string of the requested URL.

* Cookies— The unique identifier is sent in an HTTP request header called Cookie.

In order to choose the most appropriate method of state management, it is important to assess the advantages and disadvantages of each approach. Recall the distinction made in the previous chapter between user authentication, client identification, and client data. Maintaining state with a properly constructed state-management mechanism requires that the client identify itself with each request by sending some sort of unique identifier using one of the three methods just mentioned (an example approach appears at the end of this chapter). Thus, state management only deals with client identification and not user authentication or client data. Additional information can be supplied to strengthen the security of the mechanism, as this chapter covers, but the unique identifier is all that is necessary to make state management work.

