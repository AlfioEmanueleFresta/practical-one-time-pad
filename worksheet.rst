Worksheet
========================================================================

Preparation
___________

Start the Virtual Machine and use your favourite browser to navigate to the
web application (The Terrible Website,
port 12342), Usermin (port 12323) and Request Bin (port 12344).

**TODO**

The One-Time Pad is an encryption technique that is very easy to understand and
use. It has also been demonstrated that it can't be cracked if used correctly.


XOR (⊕, Exclusive OR) operator
------------------------------

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
------------

The idea behind One-Time Pads is very simple. If you XOR a message with a
completely random key, you get a ciphertext. If you XOR the ciperthext with
the same key, you get the original message back.

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

The cippertext has been shown to be impossible to decrypt
without using the original key. No amount of computing power could crack
the cipertext: a n-characters cypertext could decrypt to any
n-characters message, given the right key.
