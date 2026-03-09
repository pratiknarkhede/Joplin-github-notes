&nbsp;

Basics of Electronic Codebook Mode (ECB), Cipher Block Chaining Mode (CBC), and Counter Mode (CTR).

**ECB Mode:**

ECB is the simplest mode, where the message is divided into blocks and each block is encrypted independently using the same key.  
This is easy to implement, but it is not very secure because identical messages will produce identical ciphertexts, which can reveal information about the message.  
<br/>

`Message: M1 M2 M3`  
`Encryption: E(M1) E(M2) E(M3)`  
`Ciphertext: C1 C2 C3`

**CBC Mode**  
<br/>CBC is more secure than ECB because it XORs the plaintext block with the previous ciphertext block before encrypting it.  
This means that identical messages will produce different ciphertexts, as the ciphertext depends not only on the message but also on the previous ciphertext block.  
<br/>

`Initialization Vector (IV): IV`

`Message: M1 M2 M3`

`Encryption: E(M1 ^ IV) E(M2 ^ C1) E(M3 ^ C2)`

`Ciphertext: C1 C2 C3`

**Counter Mode:**

CTR is the most secure and efficient of the three modes discussed in the video.  
It uses a counter that is incremented for each block and then encrypted to generate a keystream.  
The keystream is then XORed with the plaintext block to produce the ciphertext.  
<br/>

`Nonce: N`  
`Counter: Ctr1 Ctr2 Ctr3`  
`Encryption: E(N + Ctr1) E(N + Ctr2) E(N + Ctr3)`  
`Keystream: K1 K2 K3`  
`Ciphertext: M1 ^ K1 M2 ^ K2 M3 ^ K3 `Source:  
<br/>

[**Computerphiles video explaining modes**](https://youtu.be/Rk0NIQfEXBA?si=LOj8qUryyY5Mw2JL)

https://youtu.be/Rk0NIQfEXBA?si=LOj8qUryyY5Mw2JL