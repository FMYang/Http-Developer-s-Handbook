#### Sample State-Management Mechanism

Rather than hand out code for a state-management mechanism with little or no explanation, I prefer to give good guidelines and methods. This allows you to gain a good understanding of the information being presented here prior to implementing something on your own. Also, you are the expert of your applications, and this makes you the best candidate for implementing the best solution for those applications. This book aims to give you the knowledge you need to decide which techniques are best for you. The book includes sample code where appropriate to demonstrate the points being discussed.

This sample mechanism uses a combination approach as mentioned in the previous section. I explain each step involved in the creation of this mechanism to help demonstrate how you can create your own.

First, you must decide which method(s) to utilize for the client to identify itself. For this example, I have chosen to use both a URL variable and a cookie.

Next, you must decide what will be contained in the cookie and what will be included on the URL (assuming you choose the same combination approach). For the cookie's value, I will use a token with the following format (separate cookies can be used as an alternative):

id=unique_id&ts=timestamp&ua=user_agent 
For the unique ID (id), I strongly suggest using an existing mechanism for generating a unique value rather than writing this logic yourself. For example, PHP users can use the uniqid() function.

For the timestamp (ts), I use a standard Unix timestamp, the number of seconds since the epoch (1970-01-01 00:00:00 GMT). This value is easily integrated into most programming languages because it can be used in most date calculations without requiring any parsing. This is very helpful because a state-management mechanism that requires heavy computation risks having serious performance problems under heavy load and is at increased risk of denial-of-service attacks.

The user agent (ua) is an additional piece of data I am using for this example for extra validation of the client's identity. It is URL-encoded so that it can be used in the format illustrated previously. Other options here include anything consistently provided in the HTTP headers of your users' Web clients. The more information you check, the more secure your mechanism is, but the less efficient and less reliable it becomes. The appropriate balance is best determined through experimentation and testing. To generate this string, use the following PHP code:

```
$id=uniqid(); 
$ts=gmmktime(); 
$ua=rawurlencode($_SERVER["HTTP_USER_AGENT"]); 
$cookie_token="id=$id&ts=$ts&ua=$ua"; 
```

For the value to pass on to the URL, I use the MD5 of the unique identifier (id) used in the cookie token. For example, this can be generated with the following PHP code:

`$url_token=md5($id); `

To see how this all fits together, consider the following example PHP script, state_example.php:

```
<? 
# Only validate a client if both tokens are present 
if (isset($_GET["url_token"]) && isset($_COOKIE["cookie_token"])) 
{ 
   # Assume identity is valid until proven otherwise 
   $identity_validated = 1; 

   # Parse cookie token into id, ts, and ua 
   $cookie_token_pairs = explode("&", $_COOKIE["cookie_token"]); 
   foreach ($cookie_token_pairs as $curr_pair) 
   { 
     list($curr_name, $curr_value) = explode("=", $curr_pair); 
     $parsed_cookie["$curr_name"] = $curr_value; 
   } 

   # Make sure the URL token is the MD5 of the client's ID 
   $url_token_should_be = md5($parsed_cookie["id"]); 
   if ($_GET["url_token"] != $url_token_should_be) 
   { 
      $identity_validated = 0; 
   } 

   # Make sure the user agent is correct 
   $ua_should_be = urldecode($parsed_cookie["ua"]); 
   if ($_SERVER["HTTP_USER_AGENT"] != $ua_should_be) 
   { 
      $identity_validated = 0; 
   } 

   if ($identity_validated) 
   { 
      # Update tokens 
      $id=$parsed_cookie["id"]; 
      $ts=gmmktime(); 
      $ua=rawurlencode($_SERVER["HTTP_USER_AGENT"]); 
      $cookie_token="id=$id&ts=$ts&ua=$ua"; 
      $url_token=$_GET["url_token"]; 

      # Update the cookie 
      header("Set-Cookie: cookie_token=$cookie_token"); 

      # Calculate the elapsed time in seconds 
      $curr_ts = gmmktime(); 
      $elapsed_time = $curr_ts - $parsed_cookie["ts"]; 

      # Display welcome message 
      ?> 
      <p>Welcome back, client [<? echo $parsed_cookie["id"]; ?>]</p> 
      <p>It has been [<? echo $elapsed_time; ?>] seconds since your last visit</p> 
      <p><a href="state_example.php?url_token=<? echo $url_token; ?>">Click Here</a></p> 
      <? 
   } 
} 
else 
{ 
   $identity_validated = 0; 
} 
if (!$identity_validated) 
{ 
   # Create tokens 
   $id=uniqid(); 
   $ts=gmmktime(); 
   $ua=rawurlencode($_SERVER["HTTP_USER_AGENT"]); 
   $cookie_token="id=$id&ts=$ts&ua=$ua"; 
   $url_token=md5($id); 

   # Set the cookie 
   header("Set-Cookie: cookie_token=$cookie_token"); 

   # Display welcome message 
   ?> 
   <p>Welcome, new client [<? echo $id; ?>]</p> 
   <p><a href="state_example.php?url_token=<? echo $url_token; ?>">Click Here</a></p> 
   <? 
} 
?> 
```

This is a very basic example of enforcing the use of both a cookie variable and a URL variable and using them to help validate the client's identity. Notice that this example treats any client that does not pass all validation steps the same as a new client visiting the page for the first time. It is a good idea to avoid providing any warnings that might give hints or even encouragement to a potential attacker.


#### Summary

This chapter and the example mechanism only demonstrate state management. The example assigns a unique identifier to each client and then identifies a returning client. For most developers, this is incomplete in regards to fulfilling the needs of a Web application. This is because most developers require session management to really get anything accomplished. With a solid method of maintaining state established, however, session management simply requires that you store some client data and retrieve it for future visits.

The following chapter builds on the information found here and in the previous chapter on cookies by demonstrating how to manage client data to allow for intelligent session management. It also covers ideas such as user authentication, which can be used to associate a particular user with a particular client (for example, the user chris is using the client identified as 12345). The example given in this chapter is augmented to provide more functionality by leveraging the benefits of a client data store.

