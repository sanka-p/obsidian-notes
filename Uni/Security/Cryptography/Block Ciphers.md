Full-Size Key Substitution Block Ciphers
In a  full-size key transposition cipher We need to have n! possible keys, so the key should have $\lceil log_2 n! \rceil}$  bits.

![[Pasted image 20231222013403.png]]


### Permutation Boxes
- Transposes Bits
	- Straight P-box
		- Only the straight P-box is invertible![[Pasted image 20231222014147.png]]
	- Compression P-box - A compression P-box is a P-box with n inputs and m outputs where m < n
	- Expansion P-box - An expansion P-box is a P-box with n inputs and m outputs where m > n. 
	![[Pasted image 20231222013610.png]]
Circular shift
![[Pasted image 20231222015225.png]]
Swap = Circular shift halfway
![[Pasted image 20231222015216.png]]
Split and combine
![[Pasted image 20231222015255.png]]

### Substitution Box
- Substitutes bits
- It is an m × n substitution unit, where m and n are not necessarily the same.
- An S-box may or may not be invertible. In an invertible S-box, the number of input bits should be the same as the number of output bits.![[Pasted image 20231222014913.png]]


### Product Cipher

A product cipher is a complex cipher combining substitution, permutation, and other components.

Rounds
Diffusion and confusion can be achieved using iterated product ciphers where each iteration is a combination of S-boxes, P-boxes, and other components. 

All modern block ciphers are product ciphers but they are divided into two
 - Feistel Ciphers
 - Non-Feistel Ciphers

### Feistel Ciphers
Attacks
 - Differential Cryptanalysis
	 - $C_1 = P_1 \oplus K$
	 - $C_2 = P_2\oplus K$
	 - $C_1 \oplus C_2 = P_1 \oplus K \oplus P_1 \oplus K$
	 - $C_1 \oplus C_2 = P_1 \oplus P_1$

