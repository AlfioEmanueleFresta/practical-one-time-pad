Answer Sheet
============


Key Reuse
---------

.. code:: python

  from cp_otp import strxor
  from cp_dictionary import list_of_words
  from itertools import combinations


  # Intercepted unintelligible ciphertexts
  c1 = b'\x0b\x0e\x1e\x0b\x17'
  c2 = b'\x15\x0a\x1b\x1d\x01'

  # Get a list of English words from a dictionary
  words = list_of_words(of_length=len(c1))

  # Get all combinations of two English words (pairs)
  pairs = combinations(words, r=2)

  # Get C1 XOR C2
  c1_xor_c2 = strxor(c1, c2)

  for m1, m2 in pairs:

      # Calculate the difference between the words
      m1_xor_m2 = strxor(m1, m2)

      # If (M1 XOR M2) == (C1 XOR C2), this is a candidate.
      # This is because (C1 XOR C2) == (M1 XOR K1) XOR (M2 XOR K2)
      if m1_xor_m2 == c1_xor_c2:

          # Calculate the key
          key = strxor(m1, c1)

          # Announce our exciting discovery.
          print("Candidate found: m1='%s', m2='%s', key='%s'"
                % (m1, m2, key))


Malleability
------------

A = Original plaintext message

B = Modified plaintext message

K = Secret key

You want to transmit B XOR K, by expanding it to:

  B **XOR** K **=** (B **XOR** A) **XOR** (K **XOR** A)

Or, in Python code:

.. code:: python

  from cp_otp import strxor, intercept_in, intercept_out


  x = intercept_in()  # Intercept the message.

  a = "Online=1; UserIsPresident=0; GenerateSupermassiveBlackHole=0;"  # Message that will be transmitted
  b = "Online=1; UserIsPresident=1; GenerateSupermassiveBlackHole=1;"  # Message that I want to transmit

  diff = strxor(a, b)  # Calculate the difference between the messages
  y = strxor(x, diff)  # Apply the difference to the intercepted message

  intercept_out(y)  # Forward to the other party.




Extension Challenge
___________________



.. code:: python

  from cp_otp import strxor
  from extension.utils import keystream_generator


  def stream_cipher(key, message, key_length=8):
      # Pad the message up to a multiple of key_length
      while len(message) % key_length != 0:
          message += b'\x00'  # Add a NULL byte.

      # Get a keystream from the key
      keystream = keystream_generator(seed=key, key_length=key_length)

      output = b''

      # Get an iterator from 0 to the length of the message, in 'key_length' steps
      iterator = range(0, len(message), key_length)

      for i, key in zip(iterator, keystream):
          # Get a block of the message
          block = message[i:i + key_length]

          # XOR it with the key
          output += strxor(block, key)

      # Optional -- remove trailing NULL bytes, useful when this cipher
      #             is being used to decipher some text!
      output = output.rstrip(b'\x00')

      return output


  plaintext = b'WOULD YOU KINDLY ENCRYPT ME, PLEASE'
  key = b'I will be very useful as a key for your encryption task, curious stranger'

  # Encrypt the plaintext with the key.
  encrypted = stream_cipher(key=key, message=plaintext)

  # This is the secret string we want to decrypt.
  secret = b'\xcf\xab|\xb40\x92y\x03r&\x014\xee\xae\xc92r(U\x98\xdee\xe8\x8fG,\x00B\xdb\xcf\xa8L6F\xa8c\x15\x89\x94>*J\xc8q\xf2"\xd9\xeb\xc5\xb4\x15i\xad\xbc!n\x92I\xee\x8a\x18\x93\x94\xfc\x11#/\x86j\xe1\x91\x14]\xa4.=\x93\x12n\xc6\x05\xedW=\xef\x13Q\xafQ\xc36\xf3A\xf2S\xed\x0f\xca\x18\x87\xf0\xfb\x07\xaepU\xb0\x0fP\x02\x1dRe\x1f\xa3\xa3\xeb\x9f\x13^\x10\xea\x93g\xff\xdc\t\xa0\x96\x90b\xf6sD\x85\x15\xc9\x8d^a\x88\xa7jN\xac\x1c\xb75\xcf\xa7\x9e\xe0\xeb\x06x|\x16\xfd\x8cHS\x95\xaa\x8f\xbe\xde@\x88\xfc\xfb\xaa#\xa1\xa0\xac\x92B\xe0I\x89\\\x05\xc4\xdc\xe9\x1eW\x12\xf6\xa6\x94"\x90g\x9e\xbea\x8d\xd0\xbd\xd2\x85\xd6\x02\x9c\xa4eR\xacv\xdb\x9c\x84\xa8x\x17\xbd\xb9\xe8\xd6\x89\xea\xb6\xfe;U\x87\xfc\xcb\x89\xb30\xcb\x03\xa8b\xa7\xea!\xbe\xdd\xfa\x01+\xc2b\xc3F\x9a\x10t\xdf>\x87\x11\xb4\xa8\xbb`H\xc7\xf4\xab\xe01\x1a\xf7d!\xaaPp\r\x9a[R\xda/{\x91\xd3\xbf F\x91\x11x1\nWPh\xf4\x96s\xa0{\x93*\xf2\xb9\xe6%\xeaI\x8d\xe1\xd6\xba\xad\x003\xfeL\xec\xef@V\xbc\xb5\x8e\x12\x07\x83\xde^\xc5\xbd\xcb\xaa\xb7\\\xfa\xfc\xd7"E\xff! \x1d\x88\xe6P\xe5\x0f+9\xecn-\xc7`\x87\xf1\xa9\x13j\\R\xf4\x16z\xacM\xf4t\xdf\xeb\xd1\n?TI|\xed\xab\xb8\x17:\xf4]m\xd6i\xb9\xbaT\xf2\xf8\xf0D\x9e\x83j\xe5\x80\xb7\x88\x9b\xb4\xb2H\xb7[\x19\xeb-\xd2\xea8\xd2E\xae\x9e\x1f\xd9#=\xdd\x15G\xc8`\xccz@\xfb\x8a\x06\xe2\x80\x18c\xeb\xc0\x0c\\\xf4\xa9\xf3\xef\xa0\xe8\xd7\xe9\xa1\xda\x94\xcf\x12{P\\\xe1&/\x82\xacq\x91\xda\xdc\x8a#\xa5e\xe9\xb2\x0e\x8d\x97#\x08c\x89\xf6,"\x84\xbbsZ\xac\xd7\x8f\t\xe4\xe3\xe3\xce\xc9\xea\xa7\x9d`\x0f~\xc2\x92\x88o\r"'

  # Decrypt the secret message using the previous encrypted message as a key
  message = stream_cipher(key=encrypted, message=secret)

  print(message)
