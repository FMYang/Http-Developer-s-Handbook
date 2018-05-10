#### Always Protect Sensitive Data

There have been numerous stories in the past of databases compromised in which sensitive data such as credit card numbers and passwords were stored unprotected. This type of practice is unacceptable, especially now that the repercussions are well known.

It is often unnecessary that data be recoverable. In these cases, following with the principle of least privilege, you should make this data unrecoverable. The most common example of this technique is a password challenge. You are generally not concerned with a user's password, but rather whether it was valid. By using a message-digest algorithm such as MD5, you can achieve this result without recovering the original password. When you initially store the user's password, store the MD5 digest of the user's password instead. When the user attempts to log in, compare the MD5 digest of what the user enters with the digest stored in the database. If they match, authentication is successful.

In some cases, such as with credit card numbers, you might need to recover the data. In these cases, a symmetric algorithm usually works best. It is very important that the symmetric key be protected, of course, but storing encrypted data makes recovery more difficult for the potential intruder.

#### Summary

You should consider the techniques described here and combine these with your own experience to create good security habits. Contrary to popular belief, creating secure applications requires very little extra effort. It simply requires a broader perspective and a bit of experience.

The following chapter discusses a few common attacks that Web developers face. These should help to broaden your perspective so that you are better prepared to anticipate the types of attacks to which your applications may be subjected.