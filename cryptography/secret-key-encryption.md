# Lab Report: Secret-Key Encryption
## **🎯 Objective**
To gain hands-on experience with various encryption algorithms, modes of operation, and common cryptographic attacks. This included manual frequency analysis on a monoalphabetic substitution cipher and performing a brute-force attack to recover an AES key.

## **🛠️ Environment & Tools**
- Tools Used: openssl (CLI), gcc (C compiler), Python (for frequency analysis scripting).
- Algorithms: Monoalphabetic Substitution, AES-128-CBC.
- Concepts: Frequency analysis (bigrams/trigrams), Initialization Vectors (IV), Padding, Brute-force attacks.

## **💡 Key Learning & Tips**
- Frequency Analysis Strategy: English language patterns are predictable. I found that identifying the most common trigram ("THE") and bigram ("TH") provides the strongest starting point. Once the structure of "the" is found, common words like "and" or "ing" usually follow.
- The Importance of IVs: In CBC (Cipher Block Chaining) mode, the Initialization Vector (IV) ensures that the same plaintext doesn't result in the same ciphertext. I learned that an IV must be unique and unpredictable to maintain security.
- Padding Verification: I explored how EVP_DecryptFinal_ex performs padding verification. If the padding is incorrect, the decryption fails, which is the basis for "Padding Oracle" attacks (though we used it here for key verification).
- Programming for Crypto: When writing the brute-force C code using the OpenSSL library, the "gotcha" was ensuring the key was exactly 16 bytes. Since my wordlist had words shorter than 16 characters, I had to pad them with the # character (0x23 in hex) as per the lab requirements.

## **🔍 Troubleshooting Log**
- | Issue Encountered | Root Cause | Solution/Observation |
- | --- | --- | --- |
- | Frequency analysis results messy | Newline characters and spaces were being counted. | Refined the script to focus only on alphabetic characters and convert everything to lowercase. |
- | Brute-force script failing | Wordlist words had trailing \n characters. | Used a stripping function in C to remove the newline before calculating the hash and padding with #. |
- | AES decryption error | Input ciphertext was not a multiple of 16 bytes. | Verified that the ciphertext was correctly parsed from hex to binary and that it aligned with the block size. |

## **🏁 Summary of Defense**
1. Entropy Matters: Brute-force attacks are only possible when the "keyspace" is small (e.g., using a common dictionary word as a key). Using truly random, high-entropy keys is non-negotiable.
2. Never Reuse IVs: Reusing an IV with the same key in CBC mode can leak information about the plaintext.
3. Library Usage: Always use established libraries like OpenSSL (evp.h) rather than trying to "roll your own" crypto functions, as the library handles complex padding and memory cleanup safely.
