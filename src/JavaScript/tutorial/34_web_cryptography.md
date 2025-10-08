# Chapter 34: Web Cryptography

Cryptography is the foundation of secure web applications. It ensures confidentiality, integrity, and authentication in a world where sensitive data constantly moves across untrusted networks. Web cryptography refers to the application of cryptographic techniques within browsers, APIs, and servers to secure communication, protect user data, and preserve privacy.

This chapter introduces the essential concepts, algorithms, APIs, and best practices every web developer should know when working with cryptography.

---

## 1. Core Concepts in Web Cryptography

### 1.1 Encryption and Decryption

- **Encryption:** The process of converting plaintext into ciphertext to prevent unauthorized access.
- **Decryption:** The process of converting ciphertext back into plaintext using a key.

### 1.2 Symmetric Cryptography

Uses the same key for encryption and decryption.

Examples:-

- **AES (Advanced Encryption Standard)(modern):** A widely used standard for secure encryption.
- **DES/3DES:** Older encryption algorithms, now considered less secure than AES.

**Use Case:** Fast bulk encryption (e.g., securing files or messages).

### 1.3 Asymmetric Cryptography

Uses a public key for encryption and a private key for decryption.

- **RSA:** Commonly used for secure data transmission.
- **Elliptic Curve Cryptography (ECC):** Offers equivalent security to RSA with smaller key sizes.

**Use Case:** Secure key exchange, digital signatures.

#### Symmetric vs. Asymmetric Cryptography

- **Symmetric Cryptography:** Uses the same key for both encryption and decryption (e.g., AES).
- **Asymmetric Cryptography:** Uses a pair of keys (public and private) for encryption and decryption (e.g., RSA, ECC).

### 1.4 Hash Functions

Generate fixed-length digests of data.

- **SHA-2 (Secure Hash Algorithm 2):** A family of cryptographic hash functions, including SHA-256 and SHA-512.
- **SHA-3:** The latest member of the SHA family.

**Use Case:** Password storage, file integrity checks.

#### Hashing

Hashing generates a fixed-length representation of data, often used for data integrity checks and password storage (e.g., SHA-256).

### 1.5 Digital Signatures

Digital signatures verify the authenticity and integrity of a message, document, or software using asymmetric cryptography.

OR

Verify authenticity and integrity of data using private/public key pairs.

Example: ECDSA, RSASSA.

### 1.6 Key Exchange

Key exchange protocols (e.g., Diffie-Hellman) securely establish a shared secret key between parties over an untrusted network.

Protocols like Diffie-Hellman establish shared secrets over untrusted networks.

Forms the basis of TLS and end-to-end encryption.

---

## 2. Secure Authentication Mechanisms

### 2.1 OAuth

OAuth is an open standard for access delegation, commonly used for token-based authentication and authorization.

Delegated authorization using third-party identity providers (e.g., Google, Facebook).

Best Practice: Use short-lived access tokens and refresh tokens.

- **Example:** Allowing users to log in with third-party accounts like Google or Facebook.
- **Best Practices:** Use short-lived tokens and refresh tokens for enhanced security.

### 2.2 JSON Web Tokens (JWT)

Self-contained tokens with header, payload, and signature.

Best Practice: Use strong signing algorithms (e.g., RS256).

JWTs are compact, URL-safe tokens used to transmit information between parties securely.

- **Structure:** Consists of a header, payload, and signature.
- **Best Practices:** Sign tokens using a strong algorithm (e.g., RS256) and validate the signature on the server.

### 2.3 Passport.js

Passport.js is a popular Node.js authentication middleware supporting various strategies, including OAuth and JWT.

- **Features:** Simplifies authentication logic with pluggable strategies.
- **Best Practices:** Use HTTPS and secure session management.

Node.js authentication middleware supporting OAuth, JWT, sessions, and more.

Best Practice: Always use HTTPS to protect tokens in transit.

---

## 3 Web Cryptography API

The Web Cryptography API is a standard interface provided by modern browsers for performing cryptographic operations securely. It enables developers to:

- Generate cryptographic keys.
- Encrypt and decrypt data.
- Sign and verify data.
- Perform hashing operations.

### Example Usage:

```javascript
// Generate a (AES-GCM) cryptographic key
const key = await crypto.subtle.generateKey(
  {
    name: "AES-GCM",
    length: 256,
  },
  true, // Whether the key is extractable
  ["encrypt", "decrypt"]
);

// Encrypt data
const encryptedData = await crypto.subtle.encrypt(
  {
    name: "AES-GCM",
    // Never reuse IV with same key in GCM. Use 12-byte random IV.
    iv: crypto.getRandomValues(new Uint8Array(12)), // unique per encryption
  },
  key,
  new TextEncoder().encode("Sensitive Data")
);
```

### Supported Operations

- **Encryption:** AES-GCM, RSA-OAEP.
- **Hashing:** SHA-256, SHA-384, SHA-512.
- **Digital Signatures:** ECDSA, RSASSA-PKCS1-v1_5.
- **Data Integrity:** Using HMAC with SHA algorithms.

---

## 4 Key Import/Export, JWK, and Storage

- Keys must be handled securely across their lifecycle.
- Export keys when needed as JWK; store only if strictly necessary.
- Import/Export: Use JWK (JSON Web Key) when necessary.

```javascript
// Export AES key as JWK
const jwk = await crypto.subtle.exportKey("jwk", key);
// Import later
const imported = await crypto.subtle.importKey(
  "jwk",
  jwk,
  { name: "AES-GCM" },
  true,
  ["encrypt", "decrypt"]
);
```

---

## Deriving Keys: PBKDF2 and HKDF

- Derive encryption keys from passwords or shared secrets.

```javascript
// PBKDF2 from password
const pwKey = await crypto.subtle.importKey(
  "raw",
  new TextEncoder().encode(password),
  "PBKDF2",
  false,
  ["deriveKey"]
);
const salt = crypto.getRandomValues(new Uint8Array(16));
const encKey = await crypto.subtle.deriveKey(
  { name: "PBKDF2", salt, iterations: 310000, hash: "SHA-256" },
  pwKey,
  { name: "AES-GCM", length: 256 },
  true,
  ["encrypt", "decrypt"]
);

// HKDF from input keying material (IKM)
const ikm = crypto.getRandomValues(new Uint8Array(32));
const ikmKey = await crypto.subtle.importKey("raw", ikm, "HKDF", false, [
  "deriveKey",
]);
const prk = await crypto.subtle.deriveKey(
  {
    name: "HKDF",
    hash: "SHA-256",
    salt: new Uint8Array(32),
    info: new Uint8Array(),
  },
  ikmKey,
  { name: "AES-GCM", length: 256 },
  false,
  ["encrypt", "decrypt"]
);
```

---

## Digital Signatures and Verification (ECDSA/EdDSA, RSASSA)

```javascript
// ECDSA P-256
const { publicKey, privateKey } = await crypto.subtle.generateKey(
  { name: "ECDSA", namedCurve: "P-256" },
  true,
  ["sign", "verify"]
);
const data = new TextEncoder().encode("message");
const sig = await crypto.subtle.sign(
  { name: "ECDSA", hash: "SHA-256" },
  privateKey,
  data
);
const ok = await crypto.subtle.verify(
  { name: "ECDSA", hash: "SHA-256" },
  publicKey,
  sig,
  data
);
```

---

## RSA-OAEP and Key Wrapping

Encrypt symmetric keys with RSA for secure sharing.

```javascript
// Generate RSA-OAEP
const rsa = await crypto.subtle.generateKey(
  {
    name: "RSA-OAEP",
    modulusLength: 2048,
    publicExponent: new Uint8Array([1, 0, 1]),
    hash: "SHA-256",
  },
  true,
  ["encrypt", "decrypt"]
);
// Wrap AES key with RSA public key
const wrapped = await crypto.subtle.wrapKey("raw", key, rsa.publicKey, {
  name: "RSA-OAEP",
});
const unwrapped = await crypto.subtle.unwrapKey(
  "raw",
  wrapped,
  rsa.privateKey,
  { name: "RSA-OAEP" },
  { name: "AES-GCM", length: 256 },
  true,
  ["encrypt", "decrypt"]
);
```

---

## HMAC and Constant-Time Comparison

```javascript
// HMAC signing
const hmacKey = await crypto.subtle.generateKey(
  { name: "HMAC", hash: "SHA-256" },
  true,
  ["sign", "verify"]
);
const tag = await crypto.subtle.sign("HMAC", hmacKey, data);
// Constant-time compare
function equalCT(a, b) {
  if (a.byteLength !== b.byteLength) return false;
  const av = new Uint8Array(a),
    bv = new Uint8Array(b);
  let diff = 0;
  for (let i = 0; i < av.length; i++) diff |= av[i] ^ bv[i];
  return diff === 0;
}
```

---

## WebAuthn (Passkeys) – Brief Overview

- Use platform authenticators for phishing-resistant auth; keys never leave the device.
- Modern authentication standard leveraging hardware security.

```javascript
const cred = await navigator.credentials.create({
  publicKey: {
    /* challenge, rp, user, pubKeyCredParams, etc. */
  },
});
```

---

## Generating Secure Random Values

Secure randomness is critical for generating keys, initialization vectors, and tokens.

### Methods:

- **Browser:** Use `crypto.getRandomValues()` to generate cryptographically secure random values.
- **Node.js:** Use `crypto.randomBytes()` for secure random byte generation.

### Example:

```javascript
// Generate a random initialization vector
const iv = crypto.getRandomValues(new Uint8Array(16));
console.log(iv);
```

---

## Best Practices in Web Cryptography

### 1. Use Strong Algorithms

- Choose modern and secure algorithms like AES-GCM, RSA with 2048+ bits, and SHA-256.
- Avoid outdated algorithms like MD5 and SHA-1.
- Never roll your own crypto – always use vetted libraries/APIs.

### 2. Key Management

- Store keys securely using hardware security modules (HSM) or secure enclaves.
- Rotate keys regularly to minimize exposure.

### 3. Secure Randomness

- Use cryptographically secure random number generators (e.g., `crypto.getRandomValues()` in JavaScript).

### 4. HTTPS Everywhere

- Always use HTTPS to ensure secure communication over the web.

### 5. Protect Against Side-Channel Attacks

- Implement constant-time algorithms to prevent timing attacks.
- Reduce observable differences in cryptographic operations.

---

## Tools and Libraries

### 1. Node.js Crypto Module

The `crypto` module in Node.js provides cryptographic functionality for server-side applications.

### 2. Third-Party Libraries

- **CryptoJS:** A popular library for client-side cryptography.
- **OpenSSL:** A robust toolkit for implementing cryptographic protocols.
- **WebCrypto:** The native browser API for web cryptography.

---

## Real-World Applications

### 1. Secure Communication

- Encrypting HTTP traffic using TLS.
- Implementing end-to-end encryption in messaging apps.

### 2. Authentication

- Using hashed passwords with salts for secure user authentication.
- Employing digital signatures for identity verification.
- Password Storage: Hash + salt using PBKDF2, bcrypt, or Argon2.

### 3. Data Integrity

- Validating the integrity of files using hash functions.
- Ensuring tamper-proof logs using HMAC (Hash-based Message Authentication Code).
- File checksums and tamper-proof logs.

---

## Conclusion

Web cryptography plays a vital role in ensuring the security and privacy of modern web applications. By understanding core concepts, leveraging robust algorithms, and adhering to best practices, developers can build applications that protect sensitive data and maintain user trust.
