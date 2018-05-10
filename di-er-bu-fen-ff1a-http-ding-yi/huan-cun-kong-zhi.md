#### Cache-Control

TheCache-Controlheaderindicates the behavior expected for any caching system, usually an intermediate caching proxy that lies between the Web client and the Web server. An example use of the header is as follows:

`Cache-Control: max-age=600`

Multiple directives may be specified with a singleCache-Controlheader as a comma-delimited list. This is illustrated by the following example:

`Cache-Control: max-age=600, no-cache="Set-Cookie"`

Depending on whether theCache-Controlheader is included in the HTTP request or the HTTP response, it can be assigned a different set of values. Each of these values \(usually called directives\) is interpreted in a very specific way so that caching systems behave exactly as expected.

For HTTP requests, the directives covered in the following sections are used.

#### no-cache

Although it may seemmisleading, theno-cachedirective does not prevent a caching system from keeping a cached copy. It simply requires that the caching system revalidate its cache prior to sending it back to the client.

Theno-cachedirective can be given an optional value such as in the following example:

`no-cache="Set-Cookie"`

When a value is given in this manner, it indicates HTTP headers that must be excluded from the cached copy, regardless of whether the copy is used. This can be helpful if you want to allow some caching but want to ensure that HTTP headers that potentially contain sensitive information are not at risk of exposure.

For example, many Web applications will display the same dynamic page to all users who are logged in. Although the response itself may contain some sensitive information in the HTTP headers, this information can be safely excluded by including these headers within theno-cachedirective as just illustrated. Multiple HTTP headers can be specified as a comma-delimited list.

#### no-store

Theno-storedirectiveis what many developers intend when they use theno-cachedirective. This actually specifies that no information pertaining to this transaction should be kept in the cache. This directive is especially helpful for sensitive transactions such as those that contain personal information, credit card numbers, authentication credentials \(logging in on an HTML form\), and so on.

>**Note
**
Internet Explorer is known to completely ignore theno-storedirective, instead treating it asno-cache. Take special note of this behavior in case the storage of a copy cannot be risked. Additionally, you may want to avoid usingno-store, as this can give a perceived disadvantage to standards-compliant Web browsers.

#### max-age

This directiveincludes a value of the form:

`max-age=600 `

The value is given in seconds, so this example indicates amax-agedirective of 10 minutes. This indicates to a caching system that it might send a cached copy of this resource to the Web client, but only if the age of its cached copy is less than or equal to the 10 minutes \(in this example\).

>**Note
**
Related material can be found in the description of theAgeresponse header in[Chapter 6](itss://chm/0672324547_ch06.html#ch06), "HTTP Responses."

#### max-stale

This directive can include a value of the form:

`max-stale=600 `

It can also be givenwithout a value. If a value is given, this indicates the number of seconds past the expiration date of the resource that are allowed to pass before the caching system must revalidate. Thus, this effectively extends the expiration date of the resource for purposes of caching.

If given without a value, themax-staledirective indicates that a caching system is allowed to respond to the Web client with an expired cached copy of the resource.

#### min-fresh

This directive includes a value of the form:

`min-fresh: 600 `

This value is given inseconds, so this indicates that the caching system can only send a cached copy of the resource to the Web client if the resource is not within 10 minutes \(in this example\) of being expired. Thus, this effectively curtails the expiration date of the resource for purposes of caching.

#### no-transform

Theno-transformdirectiveexplicitly requires that the caching system not modify the content part of the HTTP response.

#### only-if-cached

This directive indicatesthat the caching system should not contact the origin server, but it should use its cached copy of the resource in its response to the Web client. This directive is used in cases where problems are expected between the caching system and the origin server.

#### cache-extension

The HTTP definition allowsfor directives not explicitly defined to be used. When used, any proxy that does not understand the directive must ignore it.

For HTTP responses, the directives described in the following sections are used.

#### public

This is the most opendirective for theCache-Controlheader. It allows for any caching by any caching system.

#### private

Theprivatedirectiveallows caching, but not on shared caches. Thus, this is more appropriate when somewhat sensitive information can potentially be included in a response, but you still want to take advantage of caching. An example of a private cache is that of a Web browser. This is obviously less exposed than a regional cache, where numerous Web clients may be communicating directly or indirectly with it.

#### no-cache

Theno-cachedirectivedoes not prevent a caching system from keeping a cached copy. It simply requires that the caching system revalidate its cache prior to sending it back to the client.

#### no-store

Theno-storedirectivespecifies that no information pertaining to this transaction should be kept in the cache. This directive is especially helpful for sensitive transactions such as those that contain personal information, credit card numbers, authentication credentials \(logging in on an HTML form\), and so on.

#### no-transform

Theno-transformdirectiveexplicitly requires that the caching system not modify the content part of the HTTP response.

#### must-revalidate

This directive requires thecache to always revalidate its copy of a cached resource in cases where the resource has expired. This behavior is usually expected even with the absence of themust-revalidatedirective, but like many things in the HTTP definition, it allows for more clarity and an unambiguous requirement.

#### proxy-revalidate

This directive behaves exactlylike themust-revalidatedirective, except that it does not require revalidation for private caches.

#### max-age

This directiveincludes a value of the form:

`max-age=600 `

The value is given in seconds, so this example indicates amax-agedirective of 10 minutes. This indicates to a caching system that it may send a cached copy of this resource to the Web client only if the age of its cached copy is less than or equal to the 10 minutes \(in this example\).

>**Note
**
Related material can be found in the description of theAgeresponse header in[Chapter 6](itss://chm/0672324547_ch06.html#ch06).

#### s-maxage

This directive behavesexactly like themax-agedirective, except that it is ignored for private caches.

#### cache-extension

The HTTP definitionallows for directives not explicitly defined to be used. When used, any proxy that does not understand the directive must ignore it.

