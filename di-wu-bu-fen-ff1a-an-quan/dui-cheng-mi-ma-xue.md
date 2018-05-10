#### Symmetric Cryptography

When two parties want to send and receive information that only they can interpret, the traditional method is for the two parties to share a secret that explains how to encode and decode the messages. For example, assume that the messages only consist of capital letters of the Latin alphabet (A-Z). The two parties could agree to an encoding system where each letter is exchanged with another letter according to the following chart:

**Decoded   A B C D E F G H I J K L M N O P Q R S T U V W X Y Z 
Encoded   Q W E R T Y U I O P A S D F G H J K L Z X C V B N M **

Thus, a message that is encoded using this format can be decoded as long as the receiving party has this secret chart. The following example illustrates this encoding:

**Decoded   HTTPDEVELOPERSHANDBOOK 
Encoded   IZZHRTCTSGHTKLIQFRWGGA **

The encoded format is very difficult to decode for those who do not know the secret chart. For those who do, however, the message is easily decoded. Although this may seem like an obvious observation, it is a key characteristic of cryptography.

A similar idea is that both parties know only the method used for the encoding. In the previous example, it is assumed that both parties possess the secret chart. However, the format could instead be explained. The previous format exchanges alphabetic characters in ascending order with the letters from a QWERTY keyboard traversing left to right, top to bottom.

Using computers, tasks such as encoding and decoding can be automated, and the methods used to do so are called cryptographic algorithms. For example, you can call the previous method the SHIF-1 algorithm. This algorithm has a serious limitation in that everyone who knows it can decode anyone else's messages that employ it. Thus, a separate algorithm would have to be created for each party you want to exchange encoded messages with, otherwise everyone you exchange messages with would be able to decode all of your messages, regardless of the recipient. One way to make this algorithm more flexible is to introduce a secret key. This key could alter the behavior of the algorithm in some way. For example, the order of the encoded format could be expressed as a key, where the order shown previously is just one possible key, **QWERTYUIOPASDFGHJKLZXCVBNM**. You can call this new algorithm that requires a key the SHIF-2 algorithm. For example, here is the same message encoded with the SHIF-2 algorithm using **THEQUICKBROWNFXJMPSVALZYDG** as the key:

**Decoded   HTTPDEVELOPERSHANDBOOK 
Encoded   KVVJQULUWXJUPSKTFQHXXO **

To make exchanging encoded messages with new parties easier, the SHIF-2 algorithm can be made public, because the key used to encode a message can be the secret. To exchange encoded messages with a new party, you would only need to agree to the secret key to use and the SHIF-2 algorithm. Because there are 26! (**403291461126605635584000000**) possible keys that can be used in the SHIF-2 algorithm, it would be extremely difficult to decode a message without knowing the secret key. Of course, a computer can try every possible key and compare the results with known words from a dictionary to arrive at the decoded message. The time required to complete this process depends on many factors, most importantly the complexity of the algorithm, the number of possible keys, and the speed of the computer.

This imaginary algorithm I have called SHIF-2 is an example of a symmetric algorithm. Figure 18.1 illustrates the symmetrical properties of its use. Keep in mind that any messages that are sent across the Internet should be considered public.

**Figure 18.1. Both parties use identical keys when using a symmetric algorithm.
**

In practice, symmetric algorithms are much more complex than the one described here. Most employ extremely complicated mathematics that are deliberately complex. The result is that these algorithms cannot be defeated with current computer hardware within a realistic amount of time. As computer hardware improves, the strength of these algorithms decreases.

There are a few characteristics of symmetric algorithms that pose serious challenges when used to encrypt HTTP messages. The most difficult challenge is the communication of the secret key. For example, consider a Web server and a Web client that want to employ the SHIF-2 algorithm to encrypt their HTTP messages. How can the secret key be safely exchanged? If the key is compromised, the encryption is useless, because the SHIF-2 algorithm itself is public. However, a message cannot be sent encrypted without the receiving party knowing the secret key, so the key itself cannot be encrypted either. One solution to this dilemma is another type of cryptography called asymmetric cryptography.

>**Note
**
Cryptographic purists will insist that the keys used in symmetric cryptography be referred to as symmetric keys rather than secret keys. However, both uses are accurate; the term symmetric is simply less ambiguous.