A symmetric-key modern block cipher encrypts an 
n-bit block of plaintext or decrypts an n-bit block of ciphertext. The encryption or decryption algorithm uses a k-bit key. 

Block ciphers
Calculating the padding
If | M |, |Pad|, k are the length of the message, length of the padding, and the block size
| M | +  |Pad| = 0 mod k

A modern block cipher can be designed to act as a substitution cipher or a transposition cipher. 
To be resistant to exhaustive-search attack, 
a modern block cipher needs to be
designed as a substitution cipher.