Extension Challenge
===================


Stream Ciphers
______________

One of the main issues with One-Time Pad encryption is the need for a random
key at least as long as the message that is to be transmitted over the channel.
This can often be unfeasible or impractical.

**Stream Ciphers are based on the concept of One-Time Pads, but instead of using**
**a very long key, they use a short initial** *seed* **- such as a key known**
**by both parties.**

**This** *seed* **is fed into a** *keystream generator* **which**
**expands the key to a very long or infinite sequence of keys -- a** *keystream* **.**

**Then, each key in the keystream is used to encrypt a block of the message,**
**using a XOR operation.**

For your convenience, you can generate a sequence of keys from a given seed
using the provided ``keystream_generator``, e.g.:

.. code:: python

  from extension.utils import keystream_generator


  # Create a keystream generator which will generate 8 bytes keys,
  #  given the initial 'seed'.
  keystream = keystream_generator(seed="This is my initial seed",
                                  key_length=8)

  for i, key in zip(range(10), keystream):
    # Note: The keystream is infinite, therefore it needs to be zipped
    #       with a range or another finite sequence -- otherwise it will
    #       generate keys forever!

    print("Key #%d: %s" % (i, key))

This should produce the following output:

::

  Key #0: b'\xe9b\x81\xbe\x06\xc3\xe5-'
  Key #1: b'\xcc\x1c\x90\xf9uun\xca'
  Key #2: b'\x99\xfc\xfb\xe0f\xef\xfe\x9b'
  Key #3: b'\xbb\xed\xd5*\xb73\xb1\xb8'
  Key #4: b'\x08\x1fc"\x8f\xd5@\xa6'
  Key #5: b'\x89\x85\xdd\xcf\xbf\x15\xe6\xac'
  Key #6: b'\xbe #V\xb8\x85\xb9G'
  Key #7: b'\x91\xa4.\xfeb:ks'
  Key #8: b'\xb7\xaezZu-\xb3>'
  Key #9: b'\xa0\xe8V\xa2<\xdc\xa7S'

All of the generated keys are 8 bytes long, as specified in the code,
and executing again the code with the same seed won't change the resulting
keys.


.. topic:: Extension Exercise

  Using Usermin, edit the ``extension.py`` file. This contains a template for
  this exercise.

  Using the keystream generator introduced above, with a keys length of 8
  bytes, write a stream cipher function.

  Note that the length of the message may not be a multiple of the length
  of the keys in the keystream. You should pad your message with NULL bytes
  (``b'\x00'`` in Python) at the end of the message, until it is of an acceptable
  length.

  Use your stream cipher to encrypt the following message, with the key
  provided:

  .. code:: python

    # Plain text to encrypt
    plaintext = b'WOULD YOU KINDLY ENCRYPT ME, PLEASE'

    # Key to use as a seed for the keystream generator
    key = b'I will be very useful as a key for your encryption task, curious stranger'

  Finally, use your encrypted message **as a key** to decrypt the following
  message, using the same stream cipher:

  .. code:: python

    # Secret variable!
    secret = b'\xcf\xab|\xb40\x92y\x03r&\x014\xee\xae\xc92r(U\x98\xdee\xe8\x8fG,\x00B\xdb\xcf\xa8L6F\xa8c\x15\x89\x94>*J\xc8q\xf2"\xd9\xeb\xc5\xb4\x15i\xad\xbc!n\x92I\xee\x8a\x18\x93\x94\xfc\x11#/\x86j\xe1\x91\x14]\xa4.=\x93\x12n\xc6\x05\xedW=\xef\x13Q\xafQ\xc36\xf3A\xf2S\xed\x0f\xca\x18\x87\xf0\xfb\x07\xaepU\xb0\x0fP\x02\x1dRe\x1f\xa3\xa3\xeb\x9f\x13^\x10\xea\x93g\xff\xdc\t\xa0\x96\x90b\xf6sD\x85\x15\xc9\x8d^a\x88\xa7jN\xac\x1c\xb75\xcf\xa7\x9e\xe0\xeb\x06x|\x16\xfd\x8cHS\x95\xaa\x8f\xbe\xde@\x88\xfc\xfb\xaa#\xa1\xa0\xac\x92B\xe0I\x89\\\x05\xc4\xdc\xe9\x1eW\x12\xf6\xa6\x94"\x90g\x9e\xbea\x8d\xd0\xbd\xd2\x85\xd6\x02\x9c\xa4eR\xacv\xdb\x9c\x84\xa8x\x17\xbd\xb9\xe8\xd6\x89\xea\xb6\xfe;U\x87\xfc\xcb\x89\xb30\xcb\x03\xa8b\xa7\xea!\xbe\xdd\xfa\x01+\xc2b\xc3F\x9a\x10t\xdf>\x87\x11\xb4\xa8\xbb`H\xc7\xf4\xab\xe01\x1a\xf7d!\xaaPp\r\x9a[R\xda/{\x91\xd3\xbf F\x91\x11x1\nWPh\xf4\x96s\xa0{\x93*\xf2\xb9\xe6%\xeaI\x8d\xe1\xd6\xba\xad\x003\xfeL\xec\xef@V\xbc\xb5\x8e\x12\x07\x83\xde^\xc5\xbd\xcb\xaa\xb7\\\xfa\xfc\xd7"E\xff! \x1d\x88\xe6P\xe5\x0f+9\xecn-\xc7`\x87\xf1\xa9\x13j\\R\xf4\x16z\xacM\xf4t\xdf\xeb\xd1\n?TI|\xed\xab\xb8\x17:\xf4]m\xd6i\xb9\xbaT\xf2\xf8\xf0D\x9e\x83j\xe5\x80\xb7\x88\x9b\xb4\xb2H\xb7[\x19\xeb-\xd2\xea8\xd2E\xae\x9e\x1f\xd9#=\xdd\x15G\xc8`\xccz@\xfb\x8a\x06\xe2\x80\x18c\xeb\xc0\x0c\\\xf4\xa9\xf3\xef\xa0\xe8\xd7\xe9\xa1\xda\x94\xcf\x12{P\\\xe1&/\x82\xacq\x91\xda\xdc\x8a#\xa5e\xe9\xb2\x0e\x8d\x97#\x08c\x89\xf6,"\x84\xbbsZ\xac\xd7\x8f\t\xe4\xe3\xe3\xce\xc9\xea\xa7\x9d`\x0f~\xc2\x92\x88o\r"'


While this makes stream ciphers very convenient, it breaks the theoretical
security property of OTP ciphers, and its security depends solely on the
properties of the *seed* and the *keystream generator* used for the cipher.

Moreover, Stream Ciphers don't solve the malleability issue of One-Time Pads.



Notes
_____

Please note that the implementation of the keystream may not be cryptographically
secure and is intended for demonstration purposes only. Please don't use it for
other purposes, nor try to build your own. There are plenty of strong
implementations for nearly all existing programming languages.
