#### Sample Session Management Mechanism

In order to elaborate on the principles introduced in this chapter, this section introduces an example PHP script that implements some basic session-management features. This example borrows heavily from the example state management mechanism introduced in the previous chapter in order to demonstrate the characteristic that session management is an enhancement to state management.

>**Note
**
This example only makes use of a session data store and does not utilize a separate data store for persistent user data. The session data store used in this example is flexible enough to accommodate most situations, and the persistent data store for user records (such as where you might store a username and password) varies from application to application. All you must do to integrate persistent server-side data storage into this example is to assign the primary key of your user data store to a session variable after you have authenticated a user, thereby allowing you to associate the current session with a specific user.


Because the database being used is irrelevant for this discussion, I use a generic function called query() that executes a SQL statement and returns an array of the result set (if there is one). An example of this function that uses the MySQL database is as follows:

```
function query($sql) 
{ 
  mysql_connect("localhost", "myuser", "mypass"); 
  mysql_select_db("mydatabase"); 
  $result = mysql_query($sql); 
  mysql_close(); 
  return @mysql_fetch_array($result); 
} 
```

>**Note
**
This function is inefficient when multiple calls to query() occur in the same script due to the multiple database connections that are created and destroyed. It is given only to help provide a complete and working example mechanism.


This example uses the same session data store given earlier in the chapter. It consists of three fields: unique_id, last_access, and session_data.

When a new client visits, a cookie token and URL token are created (similar to the ones described in the example state management mechanism), and an entry is added to the session data store. For example:

```
# Create tokens 
$id=uniqid(); 
$ts=gmmktime(); 
$ua=rawurlencode($_SERVER["HTTP_USER_AGENT"]); 
$cookie_token="id=$id&ua=$ua"; 
$url_token=md5($id); 

# Set the cookie 
header("Set-Cookie: cookie_token=$cookie_token"); 

# Create a record in the session data store 
query("insert into session_data_store values('$id', '$ts', '')"); 
```

Rather than rely on the cookie token to determine the timestamp of the client's last access, the session data store can be used for this. This offers slightly increased reliability in the integrity of this timestamp over the approach used in the example state management mechanism.

Because one of the reasons to use session management is to identify a specific user rather than just the client, this example prompts the user for his/her name. In practice, this association is normally accomplished through a username and password authentication, and the user's name is stored in a persistent user data store, but this example will trust the user for the sake of simplicity.

Once the session and URL tokens are created and the session record established, the following welcome message can be displayed:

```
<p>Welcome, new client [<? echo $id; ?>]</p> 
<p>Please provide your name below</p> 
<form action="session_example.php?url_token=<? echo $url_token; ?>" method="post"> 
<p>First Name: <input type="text" name="first_name"></p> 
<p>Last Name: <input type="text" name="last_name"></p> 
<p><input type="submit"></p> 
```

An example of this welcome message as rendered in a browser is given in Figure 13.2.

**Figure 13.2. A new user is greeted and asked to provide his/her name.
**

When a new user provides a first and last name, the script will validate the cookie and URL tokens and then update the session record. Both the session data and the last access timestamp are updated. The first and last name can be stored in an array and serialized for storage, as the following code demonstrates:

```
# This data should actually be validated first 
$session_data["first_name"] = $_POST["first_name"]; 
$session_data["last_name"] = $_POST["last_name"]; 

# Update the session data store 
$data = serialize($session_data); 
$curr_ts = gmmktime(); 
query("update session_data_store set session_data='$data' where unique_id='$id'"); 
query("update session_data_store set last_access='$curr_ts' where unique_id='$id'"); 
```

The session data can be retrieved for a returning client as follows:

```
# Retrieve session data 
$session_data_store = query("select * from session_data_store where unique_id='$id'"); 
$unique_id = $session_data_store["unique_id"]; 
$last_access = $session_data_store["last_access"]; 
$session_data = unserialize($session_data_store["session_data"]); 
```

The following listing illustrates a full example that combines each step:

```
<? 
# Only validate a client if both tokens are present 
if (isset($_GET["url_token"]) && isset($_COOKIE["cookie_token"])) 
{ 
 # Assume identity is valid until proven otherwise 
 $identity_validated = 1; 

 # Parse cookie token 
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
  if (isset($_POST["first_name"]) && isset($_POST["last_name"])) 
  { 
   # Retrieve last access timestamp 
   $id = $parsed_cookie["id"]; 
   $session_data_store = 
    query("select last_access from session_data_store where unique_id='$id'"); 
   $last_access = $session_data_store["last_access"]; 

   # This data should actually be validated first 
   $session_data["first_name"] = $_POST["first_name"]; 
   $session_data["last_name"] = $_POST["last_name"]; 

   # Update the session data store 
   $data = serialize($session_data); 
   $curr_ts = gmmktime(); 
   query("update session_data_store set session_data='$data' where unique_id='$id'"); 
   query("update session_data_store set last_access='$curr_ts' where unique_id='$id'"); 
  } 
  else 
  { 
   # Retrieve session data 
   $id = $parsed_cookie["id"]; 
   $session_data_store = 
    query("select * from session_data_store where unique_id='$id'"); 
   $unique_id = $session_data_store["unique_id"]; 
   $last_access = $session_data_store["last_access"]; 
   $session_data = unserialize($session_data_store["session_data"]); 

   # Update the session data store 
   $curr_ts = gmmktime(); 
   query("update session_data_store set last_access='$curr_ts' where unique_id='$id'"); 
  } 

  # Update tokens 
  $ua=rawurlencode($_SERVER["HTTP_USER_AGENT"]); 
  $cookie_token="id=$id&ua=$ua"; 
  $url_token=$_GET["url_token"]; 

  # Update the cookie 
  header("Set-Cookie: cookie_token=$cookie_token"); 

  # Calculate the elapsed time in seconds 
  $curr_ts = gmmktime(); 
  $elapsed_time = $curr_ts - $last_access; 
  
  # Display welcome message 
  ?> 
  <p> 
  Welcome back, 
  [<? echo $session_data["first_name"] . " " . $session_data["last_name"]; ?>] 
  </p> 
  <p>It has been [<? echo $elapsed_time; ?>] seconds since your last visit</p> 
  <p> 
  <a href="session_example.php?url_token=<? echo $url_token; ?>">Click Here</a> 
  </p> 
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
 $cookie_token="id=$id&ua=$ua"; 
 $url_token=md5($id); 

 # Set the cookie 
 header("Set-Cookie: cookie_token=$cookie_token"); 

 # Create a record in the session data store 
 query("insert into session_data_store values('$id', '$ts', '')"); 
  
 # Display welcome message 
 ?> 
 <p>Welcome, new client [<? echo $id; ?>]</p> 
 <p>Please provide your name below</p> 
 <form action="session_example.php?url_token=<? echo $url_token; ?>" method="post"> 
 <p>First Name: <input type="text" name="first_name"></p> 
 <p>Last Name: <input type="text" name="last_name"></p> 
 <p><input type="submit"></p> 
 <? 
} 
?>  
```

#### Summary

Every Web application is different, but you should now have the information you need to create a secure and efficient mechanism for maintaining session in any programming language. The techniques introduced in this chapter should become intuitive as your understanding of HTTP strengthens.

The most important point to remember in these past three chapters is that you should take the time to consider the most appropriate technique for maintaining state and session prior to implementing anything. Rather than just "switching on" session management in your Web scripting language of choice and adding things as you go, take the time to carefully consider the information you need to maintain per user and the characteristics of each element in the design phase. This simple step can allow you to make better decisions about the techniques you employ as well as save you from possible frustrations during development.