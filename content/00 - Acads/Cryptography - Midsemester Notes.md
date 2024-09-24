---
time created: 2024-09-24 19:22
tags:
  - academics
  - cryptography
references:
  - "[[Exam Schedules and Syllabus]]"
follows:
---

> [!faq]- What is Symmetric Encryption? 
> It's the form of encryption in which the same key is used on for both encryption and decryption.

---
# Classical Encryption Techniques
## Substitution Ciphers

### Caesar's Cipher
Just shift each alphabet by a set number. Wraps around. Very easy to brute-force as there are only 25 keys to check for readable text.
### Monoalphabetic Cipher
- Requires $26!$ attempts to brute-force it. 
- Randomly map every single letter in the plaintext to any English alphabet. 
- We can easily break it with the knowledge of relative frequency of English alphabets. If in our cipher we see that $P$ is the most common, we cross-reference this with the fact that $E$ is the most common letter. We start with that substitution
### Playfair Cipher
It's a manual symmetric encryption technique. It's the first literal diagram substitution cipher. It's a multiple-letter encryption cipher.

We begin by constructing a $5*5$ matrix using a keyword. Given below is an example matrix construction.
![[playfair-cipher-matrix-construction.png]]
#### Rules for Playfair Encryption
Post creation of the $5*5$ matrix, do the following:
- Create the diagrams. Start making pairs of 2. 
- If there are any repeating letters, replace them with a filler letter and move the second repeat to the next pair. 
- If it's a rectangle -> swap. 
- If it's the same row -> wrap around horizontally. 
- If it's the same column -> wrap around vertically.

***EXAMPLE***:
Consider the word **mosque**. We split it into pairs 'mo', 'sq' and 'ue'. 
- *mo* is the same row, so *m* is swapped with *o* and *o* is swapped with *n*. It becomes *on*. 
- *sq* -> *ts*
- *ue* -> *ml* (vertical wraparound, top to bottom).
Thus **mosque** is encoded into **ontsml**. 
### Hill Cipher

> [!handwritten]- Hill Cipher - Encryption
> ![[hill-cipher-encryption-handwritten.png]]

> [!handwritten]- Hill Cipher - $K^{-1}$  calculation
> ![[hill-cipher-k-inverse-calculation-handwritten.png]]

> [!handwritten]- Hill Cipher - Decryption
> ![[hill-cipher-decryption-handwritten.png]]
### Polyalphabetic Cipher
This was invented to improve on the simle monoalphabetic technique. 
**Common Features**: 
- A set of related monoalphabetic substitution rules is used.
- A key determines which particular rule is chosen for a given transformation. 
#### Vigenere Cipher
> [!handwritten]- Vigenere Cipher
> ![[vigenere-cipher.png]]

**Autokey System**: The periodic nature of the keyword can be eliminated by using a non-repeating keyword that is as long as the message itself. This was proposed by Vigenere himself, in which a keyword is concatenated with the plaintext itself to provide a running key. 

#### Vernam Cipher


### One-Time Pad
 

## Transposition Ciphers
1. **Rail Fence Cipher**: 
2. **Row-Column Transposition**: 

