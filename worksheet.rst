Worksheet
========================================================================

Preparation
___________

Start the Virtual Machine and use your favourite browser to navigate to the
web shell (port 12322) and Usermin (port 12323).



XOR (⊕, Exclusive OR) operator
______________________________

The XOR logical operator is the basis for OTP encryption. This is a very simple bitwise
operator that essentially means 'either one, but not both'.

::

  ============  ========
     Inputs      Output
  ------------  --------
    A      B     A ⊕ B
  =====  =====  ========
    0      0       0
    1      0       1
    0      1       1
    1      1       0
  =====  =====  ========


One-Time Pad
____________

The One-Time Pad is an encryption technique that is very easy to understand and
implement. It has also been demonstrated that it can't be cracked if used correctly.

The idea behind One-Time Pads is very simple. If you XOR a binary message with a
completely random key, you will get a ciphertext. If you XOR the ciphertext again
with the same key, you get the original message back.

In the following example, each letter has been encoded to its 8-bits ASCII
equivalent. The XOR operation is applied bitwise to each letter.


**Encryption**

::

  +------------+------------+-------------------------------------------------------+
  | Message    | M          |    s        e        c        r        e        t     |
  |            |            | 01110011 01100101 01100011 01110010 01100101 01110100 |
  +------------+------------+-------------------------------------------------------+
  | Key        | K          |    m        y        -        k        e        y     |
  |            |            | 01101101 01111001 00101101 01101011 01100101 01111001 |
  +------------+------------+-------------------------------------------------------+
  | Ciphertext | C = M ⊕ K  | 00011110 00011100 01001110 00011001 00000000 00001101 |
  +------------+------------+-------------------------------------------------------+


**Decryption**

::

   +------------+------------+-------------------------------------------------------+
   | Ciphertext | C          | 00011110 00011100 01001110 00011001 00000000 00001101 |
   +------------+------------+-------------------------------------------------------+
   | Key        | K          |    m        y        -        k        e        y     |
   |            |            | 01101101 01111001 00101101 01101011 01100101 01111001 |
   +------------+------------+-------------------------------------------------------+
   | Message    | M = C ⊕ K  |    s        e        c        r        e        t     |
   |            |            | 01110011 01100101 01100011 01110010 01100101 01110100 |
   +------------+------------+-------------------------------------------------------+


The ciphertext has been shown to be impossible to decrypt
without using the original key. No amount of computing power could crack
the ciphertext: a n-characters ciphertext could decrypt to any
n-characters message, given the right key.

For example, the same ciphertext as above could be decrypted to a different, and
completely plausible message, by using a different key:

::

   +------------+------------+-------------------------------------------------------+
   | Ciphertext | C          | 00011110 00001100 01001110 00011001 00000000 00001101 |
   +------------+------------+-------------------------------------------------------+
   | Key        | K          |    j        y        /        z        u        }     |
   |            |            | 01101010 01111001 00101111 01111010 01110101 01111101 |
   +------------+------------+-------------------------------------------------------+
   | Message    | M = C ⊕ K  |    t        e        a        c        u        p     |
   |            |            | 01110100 01100101 01100001 01100011 01110101 01110000 |
   +------------+------------+-------------------------------------------------------+


The main problem with technique is that safe One-Time Pad encryption
requires a key which is completely random and at least as long as the message to encrypt.
This can be unpractical or very inefficient in some cases where the amount of
bandwidth available is a constraint.

This key also needs to be communicated safely to the other party – which simply
shifts the information protection problem to the key instead of the message,
if there isn’t another trusted communication channel.

The "One-Time Pad" name is a consequence of the physical pads that were exchanged
by the parties before separating. These were booklets of random numbers, usually
divided into small blocks, which would only be used once.


Key Reuse Attacks
_________________

Even this method is theoretically infallible, simple mistakes in its use can
have very bad consequences. For example, one could be tempted to reuse a key
or part of a key, but as we will demonstrate in this exercise, an attacker
can exploit a few properties of the system to quickly calculate a very small
subset of possible original messages.

Key Reuse attacks are generally based on the associativity property of the
XOR operator:

  A **XOR** B **=** (A **XOR** C) **XOR** (B **XOR** C)

In fact, given:

  M1 = Message A

  M2 = Message B

  K  = Key used to encrypt both message A and message B

One could write:

  M1 **XOR** M2 = (M1 **XOR** K) **XOR** (M2 XOR K)

If we assign names C1 and C2 to the ciphertexts,

  C1 **=** M1 **XOR** K

  C2 **=** M2 **XOR** K

we get:

  M1 **XOR** M2 **=** C1 **XOR** C2


An attacker could easily XOR two encrypted messages, and compare the result
with the XOR result for all pairs of words in the English dictionary. Most
of these pairs will not make sense, but with any luck, one pair will.

To make things easier for the attacker, the XOR operator is commutative:

  M1 **XOR** M2 = M2 **XOR** M1

Therefore, the attacker will only need to try only all possible combinations
of two words in the English language, which is a reasonably small number,
and is computable by a modern computer in a matter of seconds.

Using Usermin, browse to ``/home/students/otp/``. You will find a Python
file named ``exercise1.py``. You can edit this file from the Usermin web
interface.

In the Python file, the variables c1 and c2 contains two secret words
that have been encrypted using OTP. Unfortunately, the sender forgot
to cross the used secret key and ended up reusing the same key for both
the secret words.


.. topic:: Exercise 1

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
  notation (i.e. each byte is in the form ``\x4f``) or as a Python string
  of ASCII characters, e.g.:

  .. code:: python

    >>> strxor(b'secret', b'secure')  # You can use byte literals, ...
    b'\x00\x00\x00\x07\x17\x11'

    >>> strxor("secret", "secure")    # ... or ASCII strings.
    b'\x00\x00\x00\x07\x17\x11'

    >>> strxor(b'\x00\x00\x00\x07\x17\x11', b'secure')
    b'secret'

  The script should not take more than 3 minutes to execute
  on an average computer for any word length.


  Note:
    The script needs Python 3, so you'll need to use
    the ``python3`` command to execute the script from the
    Web shell, e.g.:

    .. code:: bash

      cd /home/student/otp/
      python3 exercise1.py


  Hint:
    You can use Python's built-in ``itertools.combinations`` to get
    possible pairs from a list of words. Learn more about this
    function at https://docs.python.org/3.5/library/itertools.html.



.. topic:: Exercise 2

  Now change your script so that  for each candidate pair of English words,
  it will calculate the key that may have been used.



Malleability (Bit-flipping attack)
__________________________________

The term "malleability" refers to the possibility of the ciphertext being
altered to decrypt to a different plaintext message. This generally is an
undesiderable property, and makes the system inappropriate for use in any
context where man-in-the-middle or similar attacks are possible (e.g.
Internet connections).

In this exercise we will demonstrate that One-Time Pad encryption is malleable and
susceptible to ciphertext alteration, also known as bit-flipping attacks.
In particular an attacker that knows part of the message can also modify the content of the
ciphertext to a different ciphertext, even without being able to decrypt the message.

Using Usermin, browse to ``/home/students/otp/``. You will find a Python
file named ``exercise2.py``.

Suppose you are an attacker and you found a way to intercept an encrypted
message from a sender, change the message and send it to the receiver as if
you were the original sender. This is not unrealistic -- it is in fact very
easy to do in a network, such as an open Wi-Fi network in a public place
which is under your control where you can phish users to connect.

In the Python file, the functions ``intercept_in`` and ``intercept_out`` have
been imported. These can be used respectively to get an intercepted message
as sent by the sender, and to transmit a message to the receiver.

::

  Sender  --( intercept_in )-->  You  --( intercept_out )-->  Receiver

The function ``bytes intercept_in()`` returns a Python byte literal, which is
an encrypted ASCII message. You don't know the encryption key for this message,
and you should not try to find it -- moreover, it will change at every
intercepted message.

The function ``bool intercept_out(bytes)`` can be used to transmit a Python
byte literal to the Receiver. For your convenience, this function returns
True when the practical has been completed successfully, and False otherwise.
If the received message is invalid or if the receiver can't decrypt the
message using their secret key, a ValueError exception will be thrown.

Even if you don't know the secret key, suppose you find out the content of
the plaintext which is encrypted. For the purpose of this practical, you can
do so by simply running the Python script unaltered -- which simply wires
the input to the output, allowing for normal communication between the
parties:

.. code:: bash
  cd /home/student/otp/
  python3 exercise2.py


.. topic:: Exercise 3

  Change the Python script to activate a super massive black hole.

  Hint:
    You can use the ``strxor`` method, which has already been imported
    into the Python script, from the previous Exercise.

  Hint:
    You want to generate B XOR K, but you don't know K.

    Remember the associativity property of the XOR operator:

      X **XOR** Y = (X **XOR** Z) **XOR** (Y **XOR** Z)


Ciphertext malleability could also be exploited in replay attacks: these differ from
man-in-the-middle attacks in the fact that the latter intercept and immediately
replace the original message with an altered message, while replay attacks
are executed by using replaying the same message or the altered message at
a different time.
