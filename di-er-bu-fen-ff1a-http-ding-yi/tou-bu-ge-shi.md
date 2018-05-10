#### Header Formatting

All HTTP headers follow a general format:

`Header-Name: value `

Of special note is that separate words in the header name are separated by a single hyphen, and the first letter of each word is capitalized. Many implementations, however, will only capitalize the first character of the name, although this strays from the definition. For example:

`Content-type: text/html `

The value of the header (text/html in the previous example) can be in various formats, and these formats are explained in the descriptions of the HTTP headers themselves.