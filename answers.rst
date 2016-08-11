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

B XOR K = (B XOR A) XOR (K XOR A)

Or, in Python code:

.. code:: python

  from cp_otp import strxor, intercept_in, intercept_out


  x = intercept_in()  # Intercept the message.

  a = "'Online=1; UserIsPresident=0; ActivateSuperMassiveBlackHole=0;'"  # Message that will be transmitted
  b = "'Online=1; UserIsPresident=1; ActivateSuperMassiveBlackHole=1;'"  # Message that I want to transmit

  diff = strxor(a, b)  # Calculate the difference between the messages
  y = strxor(x, diff)  # Apply the difference to the intercepted message

  intercept_out(y)  # Forward to the other party.




Extension Challenge
----------------------------------------
