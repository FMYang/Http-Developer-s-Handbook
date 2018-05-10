#### Warning

TheWarningheader,as the name implies, is intended to relay additional information about potential problems related to the response. In practice, it is normally used to alert the Web client to problems in a caching system.

The basic format of aWarningheader is as follows:

`
Warning: 110 proxya.localdomain "Response is stale"`

Optionally, a date can also be included after the error description. If included, the date should be in a proper HTTP format and enclosed with quotation marks, although many implementations fail to include the quotation marks.

The following warning codes are defined in the HTTP specification.

#### 110 Response is stale

This simply indicatesthat the resource being returned is a stale copy from the caching system that issues this warning.

#### 111 Revalidation failed

If for some reason theproxy cannot revalidate the cached copy of a resource with the origin server, it may return the unvalidated response and include this warning. This condition may occur when the origin server cannot be contacted by the proxy that issues this warning.

#### 112 Disconnected operation

This indicates thatthe proxy is intentionally disconnected from the network.

#### 113 Heuristic expiration

If the proxy returns aresource that has an age of more than 24 hours, because it determines the freshness lifetime is greater than that through heuristic measures, it must include this warning.

#### 199 Miscellaneous warning

This warning is reservedfor miscellaneous use. The brief description of the warning will be specific to the situation \(rather than "Miscellaneous warning"\).

#### 214 Transformation applied

This indicates that thecached response being sent has been modified in some way.

#### 299 Miscellaneous persistent warning

This warning is reservedfor miscellaneous use. It is used exactly as199Miscellaneous warning, except that the use of this warning indicates that the miscellaneous problem is recurring. Thus, the use of this warning is a way to indicate an increased need for concern.

