![[Pasted image 20231222011507.png]]

Properties of Block Ciphers
 - Confusion - Each plaintext symbol has a obscure relation with a symbol in the ciphertext. Confusion hides the relationship between the ciphertext and the key.
 - Diffusion - One symbol change in the plaintext changes more than one symbol in the ciphertext. Diffusion hides the relationship between the ciphertext and the plaintext.

### Traditional Symmetric Key Ciphers

Two Types
- [[Substitution Ciphers]]
- [[Transposition Ciphers]]

Stream Ciphers
 - Additive
 - Multiplicative
 - Affine
 - Vigenere
Block ciphers
![[Pasted image 20231222004928.png]]
- playfair
- hill
- 

If $P$ is the plaintext, $C$ is the ciphertext, and $K$ is the key
Encryption: $C = E_k(P)$
Decryption: $P = D_k(C)$
$P = D_k(E_k(P))$

Kerchkoff's principle - The algorithm is general knowledge.  The resistance of the cipher to attack must be based only on the secrecy of the key. 

Cryptanalysis attacks
- Ciphertext-only - Attacker has access to only the cipher text ![[Pasted image 20231221221501.png]]
- Known-plaintext - Attacker has both the plaintext and its corresponding cipher text ![[Pasted image 20231221221514.png]]
- Chosen-plaintext - Attacker can obtain the ciphertexts for arbitrary plaintexts. ![[Pasted image 20231221221547.png]]
- Chosen-ciphertext - Attacker can obtain the plaintext for arbitrary ciphertexts. ![[Pasted image 20231221221554.png]]