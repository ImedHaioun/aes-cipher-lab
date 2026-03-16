# AES Cipher Lab 🔐

A web-based AES Encryption & Decryption application supporting AES-128, AES-192, AES-256 with ECB, CBC, and CFB modes.

---

## 📖 AES Explanation

### What is AES?

**AES (Advanced Encryption Standard)** is a symmetric block cipher standardized by NIST in 2001. It replaced the older DES algorithm and is now the most widely used encryption standard in the world (used in TLS, file encryption, VPNs, etc.).

Key properties:
- **Symmetric**: The same key is used for both encryption and decryption
- **Block cipher**: Operates on fixed 128-bit (16-byte) data blocks
- **Substitution-Permutation Network**: Each round applies 4 operations: SubBytes, ShiftRows, MixColumns, AddRoundKey

---

### AES Key Lengths

| Variant   | Key Size   | Number of Rounds | Security Level       |
|-----------|------------|------------------|----------------------|
| AES-128   | 128 bits (16 bytes) | 10 rounds | Good — considered secure for most purposes |
| AES-192   | 192 bits (24 bytes) | 12 rounds | Better — higher security margin |
| AES-256   | 256 bits (32 bytes) | 14 rounds | Best — military-grade, used for top-secret data |

**How brute-force scales**: AES-128 has 2^128 possible keys (~3.4 × 10^38). AES-256 has 2^256 — incomprehensibly larger.

---

### Modes of Operation

#### ECB — Electronic Codebook
- Each 128-bit block is encrypted **independently** with the same key
- **No IV required**
- ⚠️ **Weakness**: Identical plaintext blocks → identical ciphertext blocks. Patterns can be visible (famous ECB penguin example)
- Not recommended for sensitive data

#### CBC — Cipher Block Chaining
- Each plaintext block is **XOR'd with the previous ciphertext block** before encryption
- First block uses a random **IV (Initialization Vector)**
- **Requires IV** — prepended to output in `IV:Ciphertext` format
- ✅ Much stronger than ECB — identical plaintexts produce different ciphertexts
- Most widely used mode for file/message encryption

#### CFB — Cipher Feedback
- Converts AES block cipher into a **stream cipher**
- Encrypts the IV (or previous ciphertext) first, then XORs result with plaintext
- **Requires IV**
- ✅ Can handle data that isn't a multiple of 16 bytes (no padding needed)
- Good for streaming/real-time encryption

---

### Key Derivation

AES requires exact key sizes (16, 24, or 32 bytes). User passwords are variable length, so this app derives keys using:

| Target | Method |
|--------|--------|
| AES-128 | SHA-256 hash → take first 16 bytes |
| AES-192 | SHA-256 hash → take first 24 bytes |
| AES-256 | SHA-256 hash → full 32 bytes |

This ensures the key always matches the required size regardless of password length.

---

## 🚀 Features

- ✅ Encrypt and decrypt text messages
- ✅ AES-128, AES-192, AES-256 key lengths
- ✅ ECB, CBC, CFB modes of operation
- ✅ Automatic IV generation for CBC/CFB (stored with ciphertext)
- ✅ Save encrypted output to `.txt` file
- ✅ Load ciphertext from `.txt` file for decryption
- ✅ Key strength indicator
- ✅ Built-in AES educational section

---

## 🛠 Tech Stack

- Pure HTML5 + CSS3 + JavaScript
- [CryptoJS 4.2.0](https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.2.0/crypto-js.min.js) — AES implementation
- No backend required — runs entirely in the browser

---

## 📁 File Format (.txt)

When saving encrypted output, the `.txt` file contains:

```
# AES Cipher Lab - Encrypted Output
# Mode: CBC
# Key Length: AES-256
# Generated: 2025-03-10T12:00:00.000Z
# Note: For CBC/CFB modes, format is IV:Ciphertext (Base64)

<base64_iv>:<base64_ciphertext>
```

Lines starting with `#` are metadata comments and are automatically ignored on file load.

---

## 📦 How to Run

1. Open `index.html` in any modern browser
2. No installation or server required

---

## 🔒 Security Notes

- This app is for educational purposes
- In production, use PBKDF2 or bcrypt for key derivation (not raw SHA-256)
- Never reuse IVs with the same key
- ECB mode should be avoided for real sensitive data
