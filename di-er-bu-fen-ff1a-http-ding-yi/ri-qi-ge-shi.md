#### Date Formats

Dates are used as values in several HTTP headers. The proper format for a date is as follows:

`Tue, 21 May 2002 12:34:56 GMT `

Consider this format in parts:

* First three characters of the day of the week, followed by a comma and a space

* Two-digit day of the month (01-31), followed by a space

* First three characters of the month, followed by a space

* Four-digit year, followed by a space

* Two-digit hour (0-23), followed by a colon

* Two-digit minute (0-59), followed by a colon

* Two-digit second (0-59), followed by a space and then GMT

This format allows for a date that is universally understood (GMT), unambiguous (no confusion between month and day or which century a year refers to), and of a fixed-length for easier dissection.

Most programming languages allow dates to be formatted according to a mask. For example, PHP code to generate an HTTP-compliant Date header is as follows:

`$http_date = "Date: " . gmdate("D, d M Y H:i:s T", time()); `

>**Note
**
Some criticisms of the HTTP date format state that the day of the week and the indication that the date is in GMT format are superfluous pieces of information. This is because all dates are supposed to be in GMT format, and the day of week can be calculated if needed. Thus, no ambiguity would be added, yet several bytes of space could be saved.


There are two other formats for dates that appear in some HTTP messages.

* Tuesday, 21-May-02 12:34:56 GMT

* Tue May 21 12:34:56 2002

These are not preferred and should not be used, although you may encounter them.