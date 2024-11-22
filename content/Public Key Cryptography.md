---
time created: 2024-11-22 14:22
tags:
  - academics
  - cryptography
  - endterm
references: 
follows:
---
---
# Principles of Public Key Cryptosystems. 
Consider Bob and Alice, two friends who want to communicate securely with one another. 

Conventionally, with symmetric encryption, both Bob and Alice would have to have the same key and store it securely for secure end-to-end encryption and decryption. 

With Assymetric Encryption however, Both Bob and Alice would have Public Keys in addition to their secret Private Keys. So in total, there would be 4 keys. 
1. Bob's Public Key
2. Bob's Private Key
3. Alice's Public Key
4. Alice's Private Key

---
# The RSA Algorithm
Given below are the steps taken to generate public and private keys for users using The RSA Algorithm. 
1. Select 2 different, preferably large prime numbers, $p$ and $q$. 
2. Calculate $n$ as their product -> $n = p*q$. 
3. Calculate $Φ(n)$ which is -> $$Φ(n) = (p-1)(q-1)$$
4. Select an integer $e$, such that it is relatively prime with $Φ(n)$. 
5. Calculate a number $d$ such that $$\frac{e*d}{Φ(n)} = 1$$
6. **Public Key (PU)** will thus be $\{e, n\}$. 
7. **Private Key (PK)** will thus be $\{d, n\}$. 

## How will these public and private keys be used? 
Let's assume that Alice has run the above algorithm and now has her public and private keys. Bob wants to send her a message, and has her public key. 
### Encryption - Done on Bob's Side
Bob has Alice's public key. 
$$\text{Plaintext} : M < n$$
$M$ is the message, and it's length has to be less than $n$. That's why we try to choose large prime numbers for $p$ and $q$, since $n$ is their product.
$$\text{Ciphertext} : C = M^e mod(n)$$
$C$ is now the generate ciphertext that will travel through the Internet to Alice. 

### Decryption - Done on Alice's Side
Alice has her own private key that she hasn't shared with anyone. She receives the ciphertext $C$. 
$$\text{Plaintext}: M = C^dmod(n)$$
> We remember that $d$ and $e$ are multiplicative inverses of one another. 

---
# Diffie-Hellman Key Exchange
## Analogical Explanation

![[neso-diffie-hellman-key-exchange-mapped-analogy.png]]
## The Algorithm
Given below is the algorithm for Diffie-Hellman Exchange. 
1. We begin with **2 Global Public Elements** which are
   - $q$ -> any prime number, 
   - $α$ -> primitive root of $q$, and $α<q$
2. Now, we come to Alice for now. Alice selects **her private key** $X_A$ with the condition that $$X_A < q$$
3. Alice then chooses **her public key** $Y_a$ which is calculated by: $$Y_A = α^{X_A}\text{   }mod(q)$$
4. Bob does the same, he generates **his private key** $X_B$ with the same condition as Alice that, $$X_B < q$$
5. And he also calculates **his private key** $Y_B$ by: $$Y_B = α^{X_B}\text{ }mod(q)$$
6. Then Alice and Bob calculate their shared keys, which are $$K_A \text{ or } K_B = α^{X_B*X_A}\text{ }mod(q)$$
---
