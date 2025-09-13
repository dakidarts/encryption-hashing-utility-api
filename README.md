# 🔐 Encryption & Hashing Utility API

A universal cryptography utility for developers.
Supports **hashing, encryption, and decryption** with multiple algorithms.
Built for easy **GET/POST integration** in apps.

![Encryption & Hashing Utility API](https://res.cloudinary.com/ds64xs2lp/image/upload/v1757729733/encryption_qdzl3f.gif)

---

* [Get Started on RapidAPI](https://rapidapi.com/dakidarts-dakidarts-default/api/encryption-hashing-utility-api)
* [API Docs on Dakidarts](https://dakidarts.com/api/encryption-and-hashing-utility-api/)

## 🌍 Base URL

```
encryption-hashing-utility-api.p.rapidapi.com
```

---

## ⚡ Endpoint

### `GET /crypto`

### `POST /crypto`

Perform hashing, encryption, or decryption using the specified algorithm.

---

## 📥 Query / JSON Body Parameters

| Param    | Type   | Required   | Description                                                                                                                             |
| -------- | ------ | ---------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `text`   | string | ✅ Yes      | Input plain text (for hashing/encryption) or ciphertext (for decryption).                                                               |
| `algo`   | string | ✅ Yes      | Algorithm to use. Supported: `sha256`, `sha512`, `md5`, `bcrypt`, `argon2`, `blake2b`, `blake2s`, `aes`, `rsa`, `blowfish`, `chacha20`. |
| `action` | string | ❌ No       | One of: `hash` (default for hash algos), `encrypt`, `decrypt`. Default: `encrypt`.                                                      |
| `key`    | string | ⚠️ Depends | Required for encryption/decryption (`aes`, `rsa`, `blowfish`, `chacha20`). Not needed for hash.                                         |
| `iv`     | string | ⚠️ Depends | Required (Base64) when decrypting `AES` or `Blowfish`.                                                                                  |
| `nonce`  | string | ⚠️ Depends | Required (Base64) when decrypting `ChaCha20`.                                                                                           |

---

## 🔑 Supported Algorithms

### Hashing

* `sha256`
* `sha512`
* `md5`
* `bcrypt`
* `argon2`
* `blake2b`
* `blake2s`

### Encryption / Decryption

* `AES` (CBC mode, Base64 encoded ciphertext + IV)
* `RSA` (PKCS1\_OAEP, PEM keys required)
* `Blowfish` (CBC mode, Base64 encoded ciphertext + IV)
* `ChaCha20` (Base64 encoded ciphertext + Nonce)

---

## 📊 Example Requests & Responses

### 1. Hashing (SHA256)

**Request**

```http
GET /crypto?text=hello&algo=sha256&action=hash
```

**Response**

```json
{
  "algorithm": "sha256",
  "action": "hash",
  "result": "2cf24dba5fb0a30e26e83b2ac5b9e29e..."
}
```

---

### 2. Hashing (Argon2)

**Request**

```http
POST /crypto
{
  "text": "mypassword",
  "algo": "argon2",
  "action": "hash"
}
```

**Response**

```json
{
  "algorithm": "argon2",
  "action": "hash",
  "result": "$argon2id$v=19$m=65536,t=3,p=4$..."
}
```

---

### 3. AES Encrypt

**Request**

```http
POST /crypto
{
  "text": "secret data",
  "algo": "aes",
  "action": "encrypt",
  "key": "mysecretkey123"
}
```

**Response**

```json
{
  "algorithm": "AES",
  "action": "encrypt",
  "result": {
    "ciphertext": "4tD5a8l1oG+Q6...",
    "iv": "jhs78J9n0sdh=="
  }
}
```

---

### 4. AES Decrypt

**Request**

```http
POST /crypto
{
  "text": "4tD5a8l1oG+Q6...",
  "algo": "aes",
  "action": "decrypt",
  "key": "mysecretkey123",
  "iv": "jhs78J9n0sdh=="
}
```

**Response**

```json
{
  "algorithm": "AES",
  "action": "decrypt",
  "result": "secret data"
}
```

---

### 5. RSA Encrypt

**Request**

```http
POST /crypto
{
  "text": "hidden message",
  "algo": "rsa",
  "action": "encrypt",
  "key": "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkq...\n-----END PUBLIC KEY-----"
}
```

**Response**

```json
{
  "algorithm": "RSA",
  "action": "encrypt",
  "result": "Lk23a9hdJd+Skj2..."
}
```

---

### 6. Blowfish Encrypt

**Request**

```http
POST /crypto
{
  "text": "legacy data",
  "algo": "blowfish",
  "action": "encrypt",
  "key": "legacyKey"
}
```

**Response**

```json
{
  "algorithm": "Blowfish",
  "action": "encrypt",
  "result": {
    "ciphertext": "8shd9sk32==",
    "iv": "9dsh6sdh=="
  }
}
```

---

### 7. ChaCha20 Encrypt

**Request**

```http
POST /crypto
{
  "text": "fast crypto",
  "algo": "chacha20",
  "action": "encrypt",
  "key": "superstreamkey123"
}
```

**Response**

```json
{
  "algorithm": "ChaCha20",
  "action": "encrypt",
  "result": {
    "ciphertext": "ab29dshJDs==",
    "nonce": "2sd8hs72=="
  }
}
```

---

## ⚠️ Error Handling

Examples of possible errors:

**Missing Parameters**

```json
{
  "error": "Missing required params: text, algo"
}
```

**Unsupported Algorithm**

```json
{
  "error": "Unsupported algorithm"
}
```

**Decryption Failure**

```json
{
  "error": "RSA decryption failed. Ensure correct private key & ciphertext"
}
```

---

## 🛡️ Security Notes

* Always keep keys **private & secure**.
* `bcrypt` and `argon2` are best for password storage.
* `RSA` requires proper **PEM-formatted keys**.
* Never log plaintext or keys in production apps.


