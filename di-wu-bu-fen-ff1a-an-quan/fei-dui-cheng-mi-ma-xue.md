#### Asymmetric Cryptography

With asymmetric cryptography, often referred to as public key cryptography, a key pair is employed that consists of both a public key and a private key. A message encrypted with the public key of a pair can only be decrypted with the corresponding private key. Conversely, a message encrypted with the private key can only be decrypted with the corresponding public key. For this reason, it may be clearer to visualize public and private keys as each playing the role of both a key and a lock, such as shown in Figure 18.2.

**Figure 18.2. Asymmetric cryptography employs both a public key and a private key.
**

Because of this relationship, it is now possible to exchange encrypted messages across the Internet. Whereas symmetric cryptography posed the dilemma of distributing the key without it being compromised, asymmetric cryptography employs a key that is purposely made public, and the private key never has to be distributed to anyone (nor should it).

>**Note
**
For more information about public key cryptography, I highly recommend the book, PKI: Implementing and Managing E-Security, by Nash et al., published by RSA Press.

There is one more problem that needs addressing, however, and that is the problem of identification. You can trust that information encrypted with a particular public key can only be decrypted with the corresponding private key. However, you need to be assured of the identity of the possessor of that private key. If an attacker's public key is mistakenly used for the encryption, the information can be compromised, because the attacker can decrypt the communication.