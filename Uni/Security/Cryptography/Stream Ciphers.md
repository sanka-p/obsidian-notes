In a modern stream cipher, encryption and decryption
are done r bits at a time. We have a plaintext bit stream P = pn…p2 p1, a ciphertext bit stream 
C = cn…c2 c1, and a key bit stream K = kn…k2 k1, in which pi , ci , and ki are r-bit words. 

Pattern in the ciphertext of a one-time pad cipher
1. The plaintext is made of n 0’s.: Because $0 \oplus k_i = k_i$ , the ciphertext stream is the same as the key stream. If the key stream is random, the ciphertext is also random. The patterns in the plaintext are not preserved in the ciphertext.
2. The plaintext is made of n 1’s. Because $1 \oplus k_i = \overline{k_i}$, the ciphertext stream is the complement of the key stream. If the key stream is random, the ciphertext is also random. Again the patterns in the plaintext are not preserved in the ciphertext.
3. The plaintext is made of alternating 0’s and 1’s.  In this case, each bit in the ciphertext stream is either the same as the corresponding bit in the key stream or the complement of it. Therefore, the result is also a random string if the key stream is random.
4.  The plaintext is a random string of bits. In this case, the ciphertext is definitely random because the exclusive-or of two random bits results in a random bit.

Feedback Shift Register (FSR)
The maximum period of an LFSR is to 2m − 1.
Linear Shift Feedback Register
![[Pasted image 20231222053047.png]]
In a nonsynchronous stream cipher, the key depends on either the plaintext or ciphertext.