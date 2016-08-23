Conclusion
==========

Summary: Important points
_________________________

Strong cryptographic primitives exist and the One-Time Pad cipher is one of these.
Nonetheless, one should not be fooled into believing that strong primitives
are enough to make strong systems. Strong primitives may also not
have all of those properties that are nowadays expected from cryptosystems such as
information integrity and provenance guarantee.

Moreover, cryptosystems, for each of their properties, are as strong as their weakest
component and therefore need exhaustive analysis before being put to use.

In fact, you should generally never try to build your own cryptosystem,
nor implement or endorse software that utilises a certain cryptosystem if you
don't believe the software is using the cryptosystem correctly.


Stream Ciphers
______________

One of the main issues with One-Time Pad encryption is the need for a random
key at least as long as the message that is to be transmitted over the channel.
This can often be unfeasible or impractical.

Stream Ciphers are based on the concept of One-Time Pads, but instead of using
a very long key, they use a short initial *seed* - such as a password known
by both parties. This *seed* is fed into a *keystream generator* which
expands the key to a very long or infinite sequence -- a *keystream*. Then,
the keystream is used to encrypt the message.

While this makes stream ciphers very convenient, it breaks the theoretical
security property of OTP ciphers, and its security depends solely on the
properties of the *seed* and the *keystream generator* used for the cipher.

Moreover, Stream Ciphers don't solve the malleability issue of One-Time Pads.


Ethical Issues
______________

As is common in security teaching, the techniques described here could be
used to attack systems but you must behave responsibly and ethically toward
other people, their data and systems. The writing or use of tools to gain
unauthorised access to systems is a criminal offence.

The Computer Misuse Act (1990) clearly states:

  [...]

  A person is guilty of an offence if—
    (a) he causes a computer to perform any function with intent to secure access to any program or data held in any computer, or to enable any such access to be secured;
    (b) the access he intends to secure, or to enable to be secured, is unauthorised;

  [...]

You can learn more about the Act at http://www.legislation.gov.uk/ukpga/1990/18/contents.
