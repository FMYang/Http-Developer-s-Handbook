#### URL Encoding

All HTTP messages, with the possible exception of the content section of the message, use the ISO-8859-1 \(ISO-Latin\) character set. An HTTP request may include an Accept-Encoding request header that identifies alternative character encodings that are acceptable for the content in the HTTP response.

URLs pose a special challenge, because their syntax does not allow the following groups of characters from the ISO-Latin character set:

* Non-ASCII characters— The ASCII characters are a subset of the entire ISO-Latin character set, consisting of the bottom half \(characters 0-127\) of the entire 256 characters. All non-ASCII characters \(128-255\) are invalid for use in URLs.

* Non-printable ASCII characters \(control characters\)— Several ASCII characters cannot be printed and are thus invalid for use in URLs. These are the ISO-Latin characters 0-31 and 127.

* Reserved characters— Many other characters in the ISO-Latin character set have a special meaning in URLs or otherwise cause some ambiguity in URLs or in common uses of them. \(For example, quotation marks often surround a URL in HTML syntax.\) These are outlined in Table 9.1.

**Table 9.1. Reserved Characters Cannot Be Used in URLs**

Character Number|Definition
:--- | :--- |
Character 32 | Space \( \)
Character 33 | Exclamation \(!\) 
Character 34 | Quotation marks \("\)
Character 35 | Pound sign \(\#\)
Character 36 | Dollar sign \($\)
Character 37 | Percent sign \(%\)
Character 38 | Ampersand \(&\)
Character 39 | Apostrophe \('\)
Character 40 | Left parenthesis \(\(\)
Character 41 | Right parenthesis \(\)\)
Character 43 | Plus sign \(+\)
Character 44 | Comma \(,\)
Character 47 | Slash \(/\)
Character 58 | Colon \(:\)
Character 59 | Semicolon \(;\)
Character 60 | Less than sign \(&lt;\)
Character 61 | Equals sign \(=\)
Character 62 | Greater than sign \(&gt;\)
Character 63 | Question mark \(?\)
Character 64 | At symbol \(@\)
Character 91 | Left bracket \(\[\)
Character 92 | Backslash \(\)
Character 93 | Right bracket \(\]\)
Character 94 | Caret \(^\)
Character 96 | Backtick \(\`\)
Character 123 | Left brace \({\)

> **Note  
> **  
> A good reference for the ASCII character set is [http://www.asciitable.com/](http://www.asciitable.com/).

In order to represent these invalid characters, one uses a special format called URL encoding. Each character is encoded as a percent sign \(%\), followed by the hexadecimal value of the character. So, for example, the following conversion example takes a URL and encodes it into URL encoded format:

Original string: [http://www.httphandbook.org/](http://www.httphandbook.org/)

Encoded string: http%3a%2f%2fwww.httphandbook.org%2f

The following shows the specific characters that were encoded:

* The colon \(:\) is encoded as %3a.

* The slash \(/\) is encoded as %2f.

You will likely encounter many situations where it is necessary to URL encode a string. Luckily, most Web scripting languages provide a simple function to do this encoding for you. For example, the previous transformation can be achieved in PHP as follows:

`$orig_string="http://www.httphandbook.org/";   
$enc_string=rawurlencode($orig_string);`

If you imagine passing an entire URL as a variable within another URL, the necessity of URL encoding might become clearer, especially when the URL being passed contains its own URL variables. Take the following example:

`http://www.example.org/?return_url=http%3a%2f%2fhttphandbook.org%2findex.php%3fvar% graphics/ccc.gif3dvalue`

In this example, the following name/value pair is being passed \(decoded value shown for clarity\):

`return_url=http://httphandbook.org/index.php?var=value`

If you imagine additional variables being passed on the URL delimited by an ampersand \(&\), rather than the question mark shown here, it becomes clear that it would be impossible to determine whether the variable belonged to the query string of the URL or was part of the value of return\_url.

> **Note  
> **  
> HTML entities use a similar format for representing many of these special characters in the HTML markup language. Characters from the ISO-Latin character set can be represented as &\#xxx ; where xxx represents the decimal value of the character. For example, & is an ampersand. Not all characters are supported, and consistent support is provided only for characters 32-127.



