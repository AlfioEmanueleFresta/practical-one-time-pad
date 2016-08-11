Worksheet
========================================================================

Preparation
___________

Start the Virtual Machine and use your favourite browser to navigate to the
web shell (port 12322) and Usermin (port 12323).



XOR (⊕, Exclusive OR) operator
______________________________

The XOR operator is the basis for OTP encryption. This is a very simple bitwise
operator that essentially means 'either one, but not both'.

=====  =====  ======
   Inputs     Output
------------  ------
  A      B    A ⊕ B
=====  =====  =====
0      0      0
1      0      1
0      1      1
1      1      0
=====  =====  =====


One-Time Pad
____________

The One-Time Pad is an encryption technique that is very easy to understand and
use. It has also been demonstrated that it can't be cracked if used correctly.

The idea behind One-Time Pads is very simple. If you XOR a message with a
completely random key, you will get a ciphertext. If you XOR the ciperthext again
with the same key, you get the original message back.

In the following example, each letter has been encoded to its 8-bits ASCII
equivalent. The XOR operation is applied bitwise to each letter.


**Encryption**

+----------------+------------+-----------------------------------------------------------+
| **Message**    | M          | ``...s........e........c........r........e........t....`` |
|                |            | ``01110011 01100101 01100011 01110010 01100101 01110100`` |
+----------------+------------+-----------------------------------------------------------+
| **Key**        | K          | ``...m........i........-........k........e........y....`` |
|                |            | ``01101101 01101001 00101101 01101011 01100101 01111001`` |
+----------------+------------+-----------------------------------------------------------+
| **Ciphertext** | C = M ⊕ K  | ``00011110 00001100 01001110 00011001 00000000 00001101`` |
+----------------+------------+-----------------------------------------------------------+


**Decryption**

+----------------+------------+-----------------------------------------------------------+
| **Ciphertext** | C          | ``00011110 00001100 01001110 00011001 00000000 00001101`` |
+----------------+------------+-----------------------------------------------------------+
| **Key**        | K          | ``...m........i........-........k........e........y....`` |
|                |            | ``01101101 01101001 00101101 01101011 01100101 01111001`` |
+----------------+------------+-----------------------------------------------------------+
| **Message**    | M = C ⊕ K  | ``...s........e........c........r........e........t....`` |
|                |            | ``01110011 01100101 01100011 01110010 01100101 01110100`` |
+----------------+------------+-----------------------------------------------------------+

The ciphertext has been shown to be impossible to decrypt
without using the original key. No amount of computing power could crack
the cipertext: a n-characters cypertext could decrypt to any
n-characters message, given the right key.

The problem with technique is that safe One-Time Pad encryption therefore
requires a random key which is at least as long as the message to encrypt.
This can be unpractical or very inefficient in some cases where the amount of
bandwith available is a strong constraint.

This key also needs to be communicated safely to the other party, which simply
shifts the information protection problem to the key, instead of the message,
if there is no trusted communication channel.

The "One-Time Pad" name is a consequence of the physical pads that were exchanged
by the parties before splitting up. These were booklets of random numbers, usually
divided into small blocks, which should only be used once.


Key Reuse Attacks
_________________

Even this method is theoretically infallible, simple mistakes in its use can
have very bad consequences. For example, one could be tempted to reuse a key
or part of a key, but as we will demonstrate in this exercise, an attacker
can utilise some clever methods to quickly calculate a very small number of
possible original messages.

Key Reuse attacks are based on the associativity property of the XOR operator:

  A XOR B = (A XOR C) XOR (B XOR C)

In fact, given:

  M1 = Message A
  M2 = Message B
  K  = Key used to encrypt both message A and message B

One could write:

  M1 XOR M2 = (M1 XOR K) XOR (M2 XOR K)

If we assign names C1 and C2 to the ciphertexts,

  C1 = M1 XOR K
  C2 = M2 XOR K

we get:

  M1 XOR M2 = C1 XOR C2


An attacker could easily XOR two encrypted messages, and compare the result
with the XOR result for all pairs of words in the English dictionary. Most
of these pairs will not make sense, but with any luck, a pair will.

To make things easier for the attacker, the XOR operator is commutative:

  M1 XOR M2 = M2 XOR M1

Therefore, the attacker will only need to try only all possible combinations
of two words in the English language, which is reasonably small.


.. topic:: Exercise 1

  Using Usermin, browse to `/home/students/otp/`. You will find a Python
  file named `exercise1.py`.

  You can edit this file from the Usermin web interface.

  In the Python file, the variables c1 and c2 contains two secret words
  that have been encrypted using OTP. Unfortunately, the sender forgot
  to cross the used secret key and ended up reusing the same key for both
  the secret words.

  Calculate the possible words pairs that have been encrypted. Try and
  determine which of the pairs correspond to the secret message.

  You should NOT try to crack the secret key.

  For your convenience, an English dictionary has been provided and imported
  into the script. You can read all words of length *n* using:

  .. code:: python

    list_of_words(of_length=n)

  This will return a list of all English words of length *n*.

  Moreover, a function has been provided and imported to XOR two byte literals.
  You can express parameters as either a sequence of bytes in hexadecimal
  notation (ie. each byte is in the form ``\x4f``) or as a Python string
  of ASCII characters, e.g.:

  .. code:: python

    >>> strxor(b'secret', b'secure')
    b'\x00\x00\x00\x07\x17\x11'

    >>> strxor(b'\x00\x00\x00\x07\x17\x11', b'secure')
    b'secret'


  Hint:
    You can use Python's built-in `itertools.combinations` to get
    possible pairs from a list of words. Learn more about this
    function at https://docs.python.org/3.5/library/itertools.html.


.. topic:: Exercise 2

  Now, for each possible pair of English words, calculate the key.



Malleability
____________

The term "malleability" refers to the possibility of the ciphertext being
altered to decrypt to a different plaintext message. This generally is an
undesiderable property, and makes the system inappropriate for use in any
context where man-in-the-middle or similar attacks are porssible (e.g.
Internet connections).

In this exercise we will demonstrate that OTP encryption is malleable and
susceptible to ciphertext alteration. In particular an attacker, even without
being able to decrypt the message, given they knows part of the message,
they can also modify the content of the ciphertext to a different ciphertext.

Using Usermin, browse to `/home/students/otp/`. You will find a Python
file named `exercise2.py`.

Suppose you are an attacker and you found a way of intercepting an encrypted
message from a sender, change the message and send it to the receiver as if
you were the original sender. This is not unrealistic -- it is in fact very
easy to do in a network, or phish some users to connect to an open Wi-Fi
network -under your control- in a public place.

In the Python file, the functions `intercept_in` and `intercept_out` have
been imported. These can be used respectively to get an intercepted message
as sent by the sender, and to transmit a message to the receiver.

The function `bytes intercept_in()` returns a Python byte literal, which is
an encrypted ASCII message. You don't know the encryption key for this message,
and you should not try to find it -- moreover, it will change at every
intercepted message.

The function `bool intercept_out(bytes)` can be used to transmit a Python
byte literal to the listening party. For convenience, this function returns
True when the practical has been completed successfully, and False otherwise.
If the received message is invalid or if the receiver can't decrypt the
message using their secret key, a ValueError exception will be thrown.


.. topic:: Exercise 3

  Even if you don't know the secret key, suppose you find out the content of
  the plaintext which is encrypted. For the purpose of this pracical, you can
  do so by simply running the Python script unaltered -- which simply wires
  the input to the output, allowing for normal communication between the
  parties:

  .. code:: bash
    cd /home/student/otp/
    python3 exercise1.py

  Change the Python script to activate a super massive black hole.

  Hint:
    You can use the `strxor` method, which has already been imported
    into the Python script, from the previous Exercise.

  Hint:
    You want to generate B XOR K, but you don't know K.

    Remember the associativity property of the XOR operator:

      X XOR Y = (X XOR Z) XOR (Y XOR Z)



# TODO Replay attacks?
# TODO Complete.
# TODO Spell checking.
