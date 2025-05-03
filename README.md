# cgbenigma
Cipher Library for Encryption Methods Found on Chinese Gold Bars

The existence of cryptograms found on Chinese gold bars has been known to the public for about a decade.

https://www.iacr.org/misc/china/

How to solve these cryptograms is explained here.

https://github.com/milton6310/cgbCiphers.git

# Install Library
python3 -m pip install cgbenigma

# Examples
Case 1. Encrypt a plaintext with KEY and RING values. This is the typical use of the Enigma machine.

>>> cgblib.encrypt("HELLOWORLD", "AAA", "BBB")
[{'Key': 'AAA', 'Ring': 'BBB', 'Message': 'HELLOWORLD', 'Cipher': 'SYVEDJVFMT'}]

"AAA" is KEY and "BBB" is RING.

Case 2. CGB (Chinese Gold Bar) cipher has a special method to encrypt a plaintext. It can embed the RING value in the ciphertext.

>>> cgblib.encrypt("HELLOWORLD", "XXX")
Key-Ring cipher tuple(s) found: 2
[{'Key': 'CFY', 'Ring': 'XXX', 'Message': 'HELLOWORLD', 'Cipher': 'RBTUTBUXXX'}, {'Key': 'HDO', 'Ring': 'XXX', 'Message': 'HELLOWORLD', 'Cipher': 'WXMARVWXXX'}]

If a single KEY value is entered, it will be used as the RING value for the ciphertext. In this case, the last 3 letters of the generated cipher will be the same as the RING value. The assignment of the RING value is arbitrary and there may be no generated cipher ending with the RING value. Another RING value can be tried to get the result. If the RING value is set to "XXX", two ciphertexts are generated. Both ciphertexts end with "XXX".

>>> cgblib.encrypt("HELLOWORLD", "AAA")
There is no key-ring cipher tuple with the given ring, AAA
[]

On the contrary, if the RING is set to "AAA", no cipher ending with "AAA" will be found. This special method requires only the KEY value to recover the original text. There is no need to remember both KEY and RING values to decode the ciphertext.

>>> cgblib.decrypt("WXMARVWXXX", "HDO")
'HELLOWORLD
>>> cgblib.decrypt("RBTUTBUXXX", "CFY")
'HELLOWORLD

The cipher "WXMARVWXXX" can only be decrypted with the KEY "HDO".
The cipher "RBTUTBUXXX" can only be decoded with the KEY "CFY".