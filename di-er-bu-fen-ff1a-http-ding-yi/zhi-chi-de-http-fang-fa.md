#### Allow

The Allow header can be used in one of two ways:

* In a PUT request to indicate the request methods that the Web server should allow for the given resource

* In an HTTP response with a status code of 405 Method Not Allowed to indicate which request methods are allowed for the resource being requested

The value of the Allow header is a comma-delimited list of request methods. For example:

**Allow: GET, HEAD, POST **

This would indicate (using either method) that only the methods GET, HEAD, and POST are allowed as request methods for the specified resource.