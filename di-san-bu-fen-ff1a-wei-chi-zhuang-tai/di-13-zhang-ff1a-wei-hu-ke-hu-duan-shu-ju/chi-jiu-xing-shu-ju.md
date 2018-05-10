#### Persistent Data

For data that needs to persist across multiple user sessions, the same questions must be asked as with session-only data: Which elements are sensitive and which can risk exposure over the Internet?

For data that can risk exposure, you can choose between storing the data on the server or on the client. The options for keeping the data with the client are fewer, however, because form variables are not a reliable option. URL variables can be used, but this requires the user to keep up with this information, perhaps by bookmarking a specific page. Thus, the most common option for this approach is to use cookies.

>**Note
**
Cookies are sometimes used to store information such as user preferences. Although cookies have been given a bad reputation with respect to privacy, this technique can actually offer users increased privacy by enabling some extra features without requiring personal information. This technique negates the necessity for the user to log in or for the Web client to identify itself in order to retrieve information stored on the server. The user's preferences can be communicated in the cookie itself, leaving the user otherwise anonymous.


Sensitive data should be stored on the server. Depending on how long this data needs to persist, you should consider whether it should be stored in the session data store, which is more volatile, or with the user record itself (where a username and password might be stored, for example). Most persistent data is stored in the data store for user records rather than with the session data. Persistent data can be assigned to session variables for the life of the current browser session.

Whether using files or a database for the user data store, each record should be identified by a unique identifier. This unique identifier can (and in most cases, should) be different than the unique identifier used to identify the session. An example of the unique identifier for a user record is a username, and an example of the unique identifier for a session record is a randomly generated string.

>**Note
**
The association between the user record and the session record should be that the unique identifier for the user record is stored with other session data in the session record once the user has logged in. Prior to this, there is no association between the two; the user is anonymous.