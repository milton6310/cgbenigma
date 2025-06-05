# cgbenigma
This package contains Python file(s) to use encryption methods reverse-engineered from cryptograms found on Chinese Gold Bars. In 1930s, some Chinese people deposited three hundred million US dollars into a branch of National City Bank in Shanghai. The bank issued several gold bars as a certificate of the transaction. On these gold bars, some plain Chinese characters and cryptograms were inscribed. The existence of Chinese Gold Bars has been brought out on the Internet through a blog published on [IACR web site](https://www.iacr.org/misc/china/) around 2009. The presumed owner of these gold bars known as Bin-Jiang Tao filed a law suit against the Citibank to pay the deposited fund back, but the court ruled out in favor of the Citibank and the claim was denied. The actual audio of oral argument between Bin-Jiang Tao and the Citibank can be accessed at [this link](https://www.courtlistener.com/audio/43830/bin-jiang-tao-v-citibank-n-a/). Recently, the cryptograms have been decrypted and the methods used in the generation of cryptograms also have been reverse-engineered and documented [here](https://github.com/milton6310/cgbCiphers.git).<br /><br />
CGB (Chinese Gold Bar) ciphers was presumably created with Swiss-K Enigma machine. None of CGB cryptograms exceeds 26 alphabet characetrs. CGB ciphers also are multiply encrypted kind. In other words, after a piece of plain message was encrypted, another piece of plain message is added to form a new message for the next encryption. The decrypted results show that a few CGB ciphers have been encrypted messages three times in a row by adding extra words to get the final cryptogram. The more a message encrypted, the more important information it carries. In order to decrypt CGB cipher, two parameters need to be chosen and they are KEY and RING values. CGB ciphers devised a new method to further reduce the number of parameters down to one. In other words, a CGB cipher can be decrypted by using KEY value without the RING value. Some CGB ciphers was generated in a way to use the last three letters of the cryptogram as the RING value for its decryption.

# Install Library
```
python3 -m pip install cgbenigma
```

# Examples
Case 1. Encrypt a plaintext "HELLOWORLD" with KEY ("AAA") and RING ("BBB") values.
```
cgblib.encrypt("HELLOWORLD", "AAA", "BBB")
[{'Key': 'AAA', 'Ring': 'BBB', 'Message': 'HELLOWORLD', 'Cipher': 'SYVEDJVFMT'}]
```
The cipher `SYVEDJVFMT` can be deciphered with the KEY ("AAA") and RING ("BBB") values to recover the text "HELLOWORLD".<br /><br />

Case 2. Encrypt a plaintext "HELLOWORLD" with the RING value.
```
cgblib.encrypt("HELLOWORLD", "XXX")
Key-Ring cipher tuple(s) found: 2
[{'Key': 'CFY', 'Ring': 'XXX', 'Message': 'HELLOWORLD', 'Cipher': 'RBTUTBUXXX'}, {'Key': 'HDO', 'Ring': 'XXX', 'Message': 'HELLOWORLD', 'Cipher': 'WXMARVWXXX'}]
```
If a single key value is entered, it will be used as not the KEY, but the RING value for the ciphertext. In this case, the last 3 letters of the generated cipher will be the same as the provided key value. The assignment of the key value is arbitrary and there may be no resultant cipher ending with the provided key value.
```
cgblib.encrypt("HELLOWORLD", "AAA")
There is no key-ring cipher tuple with the given ring, AAA
[]
```
When the provided key value was "AAA", there is no cipher ending with "AAA". If the provided key value was "XXX", there are two ciphers ending with "XXX" as presented above. In that case, the original plaintext can be recovered with the KEY value and the last 3 letters of the cipher is used as the RING value.
```
cgblib.decrypt("WXMARVWXXX", "HDO")
'HELLOWORLD'
cgblib.decrypt("RBTUTBUXXX", "CFY")
'HELLOWORLD'
```
The cipher "WXMARVWXXX" is decrypted with the KEY "HDO".
The cipher "RBTUTBUXXX" is decoded with the KEY "CFY".
