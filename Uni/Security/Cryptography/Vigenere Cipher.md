$P = P_1P_2P_3...$
$C = C_1C_2C_3...$
$k = (k_1,k_2,k_3,...,km), (k_1,k_2,k_3,...,km),...,(k_1,k_2,k_3,...,km)$

$C_i = P_i + k_i$
$P_i = C_i - k_i$
https://www.youtube.com/watch?v=ZFdsJ4a_3k8
Attacks
- Kasiski test
	- Happens when the same set of letters are encrypted using the same set of keys in the key stream
	- If the length of the keyword divides the distance between the repeated plaintext, the ciphertext must also repeat
	- If the distance between the repeated plaintext / ciphertext is large, many key lengths could divide the gap leaving a lot of options for the true key size
	- If you have multiple repeats, then you know n must divide all of them