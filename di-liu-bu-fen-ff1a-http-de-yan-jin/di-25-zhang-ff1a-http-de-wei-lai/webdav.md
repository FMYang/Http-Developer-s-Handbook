#### WebDAV

WebDAV, Web-based Distributed Authoring and Versioning (often simply referred to as DAV), is an extensive framework that seeks to provide a medium for Web-based collaboration. The WebDAV specification identifies three fundamental characteristics:

* Locking

* Properties

* Namespace management

Locking is an essential topic whenever more than one entity can access the same resource. With collaboration between multiple people, the resource can be a file that can be edited. In order to allow for synchronization so that multiple people can edit the same file without overwriting each other's changes, a resource can be locked. This means only one person at a time can edit the resource. This technique avoids the necessity of merging changes, whereby multiple people have a changed resource and need to merge all changes into one.

Properties refer to the properties of a resource. WebDAV utilizes XML for this purpose, and the discovery process for the properties of a resource relies on DASL, the DAV Searching and Locating protocol.

Namespace management can be described as common filesystem commands extended to the resources available on the Web. WebDAV provides methods for key commands such as copying and moving.

There are many HTTP headers, request methods, and response codes defined by WebDAV. In fact, WebDAV is considered a separate protocol that is basically an extension of HTTP. Thus, it requires a WebDAV client much like HTTP requires a Web client. This is likely the major factor that has thus far prevented WebDAV from being adopted more readily. The future of WebDAV is still solidifying, however, and many people expect to see it play a very important role in the future of Web-based collaboration.

>**Note
**
One use of WebDAV that has been gaining attention is the Subversion project, a version control system that leverages the strengths of WebDAV for collaborative development. This project seeks to become a "compelling replacement for CVS," the current industry standard version control system. For more information on Subversion, visit http://subversion.tigris.org/.

Another popular use of WebDAV is Microsoft's Web folders. Windows (98/2000/NT/ME/XP) provides a WebDAV client that allows users to connect to folders located on remote Web servers.

>**Note
**
For more information about WebDAV, see RFC 2518, RFC 3253, and http://webdav.org/.

