#### Restricting Access with Cookie Attributes

When a Web server adds a Set-Cookie response header to the HTTP response, it includes additional information about the access restrictions for the cookie. To further discuss this point, this section introduces two example uses of the Set-Cookie response header:

```
Set-Cookie: first_name=chris; domain=.httphandbook.org; 
            expires=Tue, 21 May 2002 12:34:56 GMT; path=/; secure 

Set-Cookie: first_name=chris 
```

In the first example, the server is asking the Web client to store a cookie called first_name with a value of chris. The rest of the attributes provide additional information about the access restrictions to be imposed on this cookie. Breaking down each additional attribute (the list of attributes is delimited by semicolons), you have:

* domain=.httphanbook.org— This cookie should only be included for requests to subdomains of httphandbook.org.

* expires=Tue, 21 May 2002 12:34:56— This cookie should be stored only until this expiration date.

* path=/— This cookie should be sent in requests for documents within document root (logically, this is equivalent to granting access to all resources within the specified domains).

* secure— This cookie should be returned only when the request is being made over a secure connection such as SSL or TLS.

Note

The path attribute specifies the directory that a resource must be within in order for the cookie to apply. For example, in the case where path=/foo/, a request for a resource located in /foo/bar/ is still considered to be within the /foo/ directory. Thus, this restriction can be thought of as the uppermost directory a resource is allowed to be within.


>**Note
**
The domain attribute should be omitted for cases where you may require access to the cookie for URLs without a sub-domain. For example, a cookie set with domain=.httphandbook.org will be sent in requests to www.httphandbook.org, but some Web browsers will omit the cookie from requests for httphandbook.org.


In the second example, the following default attributes are used:

* domain— The domain of the current resource.

expires— Resides only in memory, thus expiring when the browser process exits (all browser instances are closed).

* path— Document root (no path restrictions).

* secure— Without the secure attribute, the cookie can be returned in insecure ("plain") HTTP requests.

For attributes that designate a restriction of some sort, an absence of the attribute simply implies an absence of the particular restriction.

There are other attributes that are identified in the specification, but most Web agents do not provide support for these. They are as follows:

* comment— A description of the cookie's purpose or function.

* max-age— Maximum age of the cookie in seconds.

* version— The version of HTTP state management that the cookie complies with.

Regardless of the attributes used in a Set-Cookie response header, the Cookie request header will only include the name and value of the cookie. The cookie attributes are only used by the browser to determine whether the cookie should be included.

