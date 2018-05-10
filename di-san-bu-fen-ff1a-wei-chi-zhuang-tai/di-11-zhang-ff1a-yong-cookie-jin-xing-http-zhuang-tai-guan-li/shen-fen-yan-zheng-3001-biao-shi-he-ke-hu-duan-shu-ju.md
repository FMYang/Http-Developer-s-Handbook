#### Authentication, Identification, and Client Data

One important distinction that eludes some Web developers, especially those who are new to developing applications that require session management, is the distinction among user authentication, client identification, and client data. Distinguishing these concepts is essential to being able to provide the most appropriate session management mechanism for your Web applications.

The authentication of the user is the process by which you determine to a reasonable extent that the user is who he/she claims to be. This typically involves the user supplying a unique identifier, such as a username, and providing the answer to a challenge, such as a password. Authentication typically happens once. After a user has been authenticated, you need only to identify the client in future requests, not re-authenticate.

Identification is the method by which you associate multiple transactions together. For example, if you assign a unique identifier to a Web client, you can distinguish that particular Web client from all others and determine which HTTP requests originate from it. Once a user is authenticated, user identification only requires that you identify the Web client on each request. Thus, identification refers to Web client identification. User identification is a logical relationship you provide in your application logic that ties a particular user to a particular Web client.

Client data refers to all data associated with a particular Web client. In cases where you have associated a unique user to a particular Web client, this can be the user's data.

Once the user is logged in, the state of the user, along with all other user data, can (and in most cases should) be stored on the Web server. All data associated with the user that may be used by the application should be stored in such a way as to be associated with the user's unique identifier. This typically involves storing the data in a database, whereby the table required to store the data uses the unique identifier as a primary key.

A unique identifier is the only piece of data that is essential for the client to communicate back to the server during each request in order to allow for state management. So that the unique identifier is difficult to guess by a potential attacker, it should be different from the unique identifier that the user uses for authentication. In fact, many Web applications establish state management prior to user authentication and can allow anonymous users to access certain features.

>**Note
**
The terms state management and session management are often exchanged freely. However, as I refer to these terms in this book, state management refers solely to any technique that allows for Web client identification. This is generally accomplished by associating multiple transactions together through the use of a unique identifier assigned to the client upon entering the site.

Session management also requires the maintenance of client data. It relies on state management because client data must be distinguished, but it also involves the maintenance of all data specifically associated with the application. For this reason, most Web scripting languages boast native session management, because this is much more involved than simply distinguishing clients.


I have seen many odd implementations that re-authenticate the user for each transaction by having the user pass the authentication information from page to page. This not only requires extra execution time for the authentication to occur for each request, it also greatly diminishes security by exposing the authentication information far more than necessary.

The most likely reason for this mistake is a misunderstanding of what is required to keep track of a user. Once a method of state management has been established, you need only to authenticate the user once. Because state management provides a way to identify a Web client, user identification simply requires that you remember which user is associated with which client upon authentication.

