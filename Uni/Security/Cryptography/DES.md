It is a symmetric key block cipher
The encryption process is made of two permutations (P-boxes), which we call initial and final permutations, and sixteen Feistel rounds. 
The initial and final permutations are straight P-boxes that are inverses 
of each other.
They have no cryptography significance in DES.
The heart of DES is the DES function. The DES function applies a 48-bit key to the rightmost 32 bits to produce a 32-bit output. 
![[Pasted image 20231222061023.png]]![[Pasted image 20231222061052.png]]
![[Pasted image 20231222061142.png]]

To make all 16 rounds unique
 - last round has no swapper (S-box)

![[Pasted image 20231222094037.png]]![[Pasted image 20231222094053.png]]![[Pasted image 20231222094105.png]]![[Pasted image 20231222094112.png]]

![[Pasted image 20231222094202.png]]![[Pasted image 20231222094213.png]]![[Pasted image 20231222094219.png]]

S box provides confusion + difffusion
P-box provides diffusion

Weaknesses
 - After two encryptions
with the same key the original plaintext block is created.
![[Pasted image 20231222094600.png]]
DES has a key domain of 256. The total number of the above keys are 64 (4 + 12 + 48). The probability of choosing one of these keys is 8.8 × 10−16, almost impossible.

Key Complement - converts 1s to 0s and vice versa
Eve only has to try half of the combinations because if we encrypt the complement of plaintext with the complement of the key we get the complement of the cipher text. So you need to brute force only half of the keys
$C = E(K, P) \;\; \overline{C} = E(\overline{K}, \overline{P})$

It has been revealed that the designers of DES already knew about this type of attack and designed S-boxes and chose 16 as the number of rounds to make DES specifically resistant to this type of attack. 

It has been shown that DES can be broken using 243 pairs of known plaintexts. However, from the practical point of view, finding so many pairs is very unlikely.