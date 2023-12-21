### Traditional Symmetric Key Ciphers

Two Types
- [[Substitution Ciphers]]
- [[Transposition Ciphers]]

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