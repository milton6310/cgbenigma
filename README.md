# cgbenigma
This package contains Python files that utilize encryption methods reverse-engineered from cryptograms found on Chinese Gold Bars. In the 1930s, some Chinese individuals deposited three hundred million US dollars into a branch of the National City Bank in Shanghai. In return, the bank issued several gold bars as certificates of the transaction. These gold bars were inscribed with plain Chinese characters and cryptograms. The existence of the Chinese Gold Bars gained attention on the Internet through a blog published on the [IACR web site](https://www.iacr.org/misc/china/) around 2009. The presumed owner of these gold bars, known as Bin-Jiang Tao filed a lawsuit against Citibank to recover the deposited funds; however, the court ruled in favor of Citibank, and the claim was denied. The actual audio of the oral argument between Bin-Jiang Tao and Citibank can be accessed at [this link](https://www.courtlistener.com/audio/43830/bin-jiang-tao-v-citibank-n-a/). Recently, the cryptograms have been decrypted, and the methods used to generate the cryptograms have also been reverse-engineered and documented [here](https://github.com/milton6310/cgbCiphers.git).<br /><br />
CGB (Chinese Gold Bar) ciphers were presumably created with a Swiss-K Enigma machine. None of the CGB cryptograms exceed 26 alphabet characters. CGB ciphers are also multiply encrypted. In other words, after a piece of plain text was encrypted, another piece of plain text is added to form a new message for the next encryption. The decrypted results show that some CGB ciphers have been encrypted three times in a row by adding extra words to produce the final cryptogram. The more a message is encrypted, the more important information it carries.<br /><br />
<p align="center">
  <img src="https://www.cryptomuseum.com/crypto/enigma/img/300879/043/full.jpg" width="350" title="Swiss-K Enigma Machine">
</p>
To decrypt a CGB cipher, two parameters need to be chosen: the KEY and the RING values. CGB ciphers introduced a new method to further reduce the number of parameters down to just one. In other words, a CGB cipher can be decrypted using only the KEY value, without needing the RING value. Some CGB ciphers are generated in such a way that the last three letters of the cryptogram are used as the RING value for decryption.<br /><br />

# Install Library
```
py -m pip install cgbenigma
```
The original class definition of Enigma and Rotor was cloned from [pyEnigma](https://github.com/cedricbonhomme/pyEnigma) and used for the implementation of CGB ciphers.<br /><br />
# Reconstructed Decrypted Text
```
A GOVT CIV BOU CO
REPAY US OGD WHEN LATE TOLD

OUST GOV P BANK CT GOL EES VOG
PAY KEY IN [USS, KAN] UP IF DUE ASK
USED [JUD, BOR] IS GOV ID P KEY [SEA]

US VET SIG CO
US BERGE GOV KEY [JDS, IAA]
QUIZ BUYER(/ASH OK/A CLUE/BY NUM) TO BYE ANY OPP

ACC NO [ASH KEEP I]
```
"A government civillian group bought company. The bank repay us of gold when late told. Ousted government purchased CT bank and the government see log of transaction. Pay up with keys [USS, KAN] in if due ask. Who used the keys [JUD, BOR] is the government with the identification P-KEY [SEA]. Buyer is challenged to bye any opposition. Account number [ASH KEEP I]."<br /><br />

# Examples
## Case 1. Encrypt a plaintext "HELLOWORLD" with KEY ("AAA") and RING ("BBB") values.
```
cgblib.encrypt("HELLOWORLD", "AAA", "BBB")
[{'Key': 'AAA', 'Ring': 'BBB', 'Message': 'HELLOWORLD', 'Cipher': 'SYVEDJVFMT'}]
```
```
cgblib.decrypt("SYVEDJVFMT", "AAA", "BBB")
HELLOWORLD
```
The cipher `SYVEDJVFMT` can be deciphered with the KEY ("AAA") and RING ("BBB") values to recover the text "HELLOWORLD".<br /><br />

## Case 2. Encrypt a plaintext "HELLOWORLD" with the RING value.
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
The cipher "RBTUTBUXXX" is decoded with the KEY "CFY".<br /><br />

## Case 3. Find ciphers that contain the provided string in its decrypted text.
```
cgblib.find(["FEWGDRHDDEEUMFFTEEMJXZR"], ["BANK"])
[{'Key': 'CGX', 'Ring': 'XZR', 'Message': 'DDVTBZTZOADREBANKQTZQMD', 'Cipher': 'FEWGDRHDDEEUMFFTEEMJXZR'}, {'Key': 'GPV', 'Ring': 'XZR', 'Message': 'OUSTGOVPBANKIZMUBMHUVOG', 'Cipher': 'FEWGDRHDDEEUMFFTEEMJXZR'}, {'Key': 'HWS', 'Ring': 'XZR', 'Message': 'BANKUPCWXJKZEZPFSVJRLDH', 'Cipher': 'FEWGDRHDDEEUMFFTEEMJXZR'}]
```
It checks if the decrypted text contains `BANK` while changing the KEY value from `AAA` to `ZZZ`. The last three letters of the cipher `XZR` is used as the RING value.
```
cgblib.find(["FEWGDRHDDEEUMFFTEEMJXZR"], ["BANK"], "AAA")
[{'Key': 'CVU', 'Ring': 'AAA', 'Message': 'MKXBFBANKODXXRTMOQTDHQD', 'Cipher': 'FEWGDRHDDEEUMFFTEEMJXZR'}, {'Key': 'FHG', 'Ring': 'AAA', 'Message': 'DWQRBZTZOADREBANKQTZQMD', 'Cipher': 'FEWGDRHDDEEUMFFTEEMJXZR'}, {'Key': 'JQE', 'Ring': 'AAA', 'Message': 'OUSTGOVPBANKIZMUBMHUVOG', 'Cipher': 'FEWGDRHDDEEUMFFTEEMJXZR'}, {'Key': 'KXB', 'Ring': 'AAA', 'Message': 'BANKUPCWXJKZEZPFSVJRLDI', 'Cipher': 'FEWGDRHDDEEUMFFTEEMJXZR'}]
```
When a specific value was provided, it will be used as the RING value.<br /><br />

## Case 4. Print decrypted text
```
decrypted = cgblib.getDecryptedCiphers("A")
print(decrypted)
[{'cipher': 'ABRYCTUGVZXIWA', 'key': 'ADD', 'ring': 'IWA', 'decoded': [{'order': 1, 'cipher': '', 'key': '', 'ring': '', 'message': 'REPAY'}, {'order': 2, 'cipher': 'SPIBLDZCQ', 'key': 'VMA', 'ring': 'ZCQ', 'message': 'USOGDWHEN'}], 'text': 'REPAY US OGD WHEN', 'interpretation': 'Repay us of gold when'}]
```
```
print(decrypted[0]['cipher'])
ABRYCTUGVZXIWA
```
```
print(decrypted[0]['text'])
REPAY US OGD WHEN
```
```
print(decrypted[0]['interpretation'])
Repay us of gold when
```
