#### Session-Only Data

For data that needs to persist only as long as the user is actively interacting with your application, the only major consideration you need to make is whether the data is sensitive. For data that can risk exposure over the Internet, you can choose between storing the data on the client or storing the data on the server. An example of storing the data on the client is the use of a cookie, and an example of storing the data on the server is the use of a database as a session data store. For sensitive data, you want to expose it over the Internet as little as possible, so you want to store this information on the server rather than have the client keep up with it.

When I refer to storing the data on the client, I am referring to methods where the client will send the data within each HTTP request. Although cookies are an intuitive example of this technique, even URL and form variables can be considered methods of storing data on the client, even though the data may never be physically saved. This is because the client is providing the data rather than it being kept on the server.

In the previous chapter, you learned that cookies, URL variables, and form variables are the three methods the client can use to communicate a unique identifier with each request. Although the unique identifier is sensitive, it is the one piece of sensitive data that must risk exposure over the Internet, because it is required for the identification of the client. This caveat poses the greatest challenge to Web developers trying to implement secure state or session management.

With the exception of the unique identifier, sensitive data should be stored on the Web server. The most common method of server-side storage is the use of a database, although you can also store such data in files. A database is generally the most flexible and can handle synchronization and maintain data integrity, among other things. Using files can be convenient for situations where a database server is not available for some reason or for cases where you might be a budding Web developer who has yet to study databases.

>**Note
**
If your Web serving environment is (or may potentially be) a cluster of more than one node, do not store session data in files, because the session data will be lost if the user's subsequent requests are sent to a different node. There are ways around this problem, such as ensuring that a user will be directed to the same node for each request, but none are as elegant as a proper application design.

The method of directing a user to the same node for each request is accomplished with load balancing. This technique is usually referred to as sticky sessions. This approach creates additional overhead that is unnecessary if a central database is used to store session data, so that each node has access to the same session data store. The sticky session technique is also less reliable because it does not utilize the unique identifier to identify clients, relying instead on unreliable metrics such as the client's IP address.

Another method that can be used is NFS, Network File System, which is a protocol that allows file systems to be shared among network peers. Thus, a shared NFS partition can be used to store data common to all machines in a cluster. Due to security, integrity, and performance concerns, NFS has a poor reputation among network and systems administrators and might not be an option for you. For more information about NFS, the latest specification is RFC 3010.

Neither sticky sessions nor NFS are considered an elegant solution, and it is recommended that you instead design your application to accommodate a clustered environment by not relying on the session data being stored on the local filesystem.


Whether using files or a database to store the session information, there are three basic elements you will want to store for each session's record:

* Unique identifier

* Timestamp of last access

* Client data

An example session data store in MySQL (a popular open source database) can be created with the following SQL statement:

```
create table session_data_store 
( 
   unique_id varchar(32) primary key, 
   last_access varchar(10), 
   session_data text 
); 
```

A visual representation of this session data store as provided by MySQL is as follows:

```
+--------------+-------------+------+-----+---------+-------+ 
| Field        | Type        | Null | Key | Default | Extra | 
+--------------+-------------+------+-----+---------+-------+ 
| unique_id    | varchar(32) |      | PRI |         |       | 
| last_access  | varchar(10) | YES  |     | NULL    |       | 
| session_data | text        | YES  |     | NULL    |       | 
+--------------+-------------+------+-----+---------+-------+ 
```

The unique identifier, as I have explained, is the one mandatory element that the client must send with each HTTP request in order to maintain state. This is what distinguishes the current client from all other clients.

>**Note
**
Although some information, such as a username, might be unique per client, it is best to choose a unique identifier that is not easy to guess and also does not require the user to be logged in. This will allow you to enable some features of session management prior to authenticating a user.


The timestamp for the user's last access can be used to allow some sort of timeout mechanism for the session. This timestamp is reset upon the receipt of each HTTP request immediately after being checked to ensure that it is within the tolerated threshold of inactivity.

The data store for the session data should be quite volatile, containing only the current sessions that are active, so the timestamp should also be used with some sort of maintenance process that removes stale sessions. This activity is handled automatically by some built-in session management mechanisms found in modern Web scripting languages, but you will need to implement this yourself if you build your own.

>**Note
**
One key consideration is that the timeout element can help you determine whether a session is currently active, so it is not necessary to keep only active sessions in the data store. In fact, you should keep session data stored for longer than the tolerated threshold of inactivity, because this allows users to resume timed out sessions by re-authenticating, perhaps by providing their password to continue. This is much friendlier than treating a user who only recently timed out like a complete stranger.


The session data is easiest to store as a single element, such as a serialized string, so that the mechanism itself is as flexible as possible. A serialized string of data is basically each variable and its associated value combined into a single string that can be parsed into the individual elements when necessary. As an example, consider the following variables and their corresponding values:

```
first_name=Chris 
last_name=Shiflett 
fav_color=red 
```

Examples of serialized strings using this data as implemented by PHP and ColdFusion, respectively, are as follows:

```
first_name|s:5:"Chris";last_name|s:8:"Shiflett";fav_color|s:3:"red"; 
first_name=Chris#last_name=Shiflett#fav_color=red# 
```

More complicated elements such as arrays and objects are less intuitive than these examples, but most Web scripting languages handle the serialization for you when you take advantage of their integrated session management mechanism. In fact, the actual format used for storing session data is hidden from Web developers when using the built-in session management of PHP or ColdFusion.

With these three elements (unique identifier, timestamp of last access, and session data) well defined in a file structure or in a database table, your basic session management procedure, once it has already been initiated, will follow the steps outlined in Figure 13.1.

**Figure 13.1. The procedure for continuing a returning user's session.**