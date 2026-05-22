# Web Crypto API

[MDN – Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API) | [W3C Specification](https://www.w3.org/TR/WebCryptoAPI/)

The Web Crypto API is a browser-native cryptography interface that allows JavaScript to perform cryptographic operations (key generation, encryption, signing, hashing) without relying on external libraries. Keys generated with it can be marked non-exportable — they never leave the browser.

> **Relevance:** Used in SAKE to generate X.509 key pairs entirely in the browser. The private key stays in memory and is never transmitted anywhere.

---

## Accessing it

```js
// Available globally in all modern browsers
const crypto = window.crypto          // or just crypto
const subtle = window.crypto.subtle   // the main API surface
```

`crypto.subtle` is where all the interesting operations live. It returns Promises.

---

## Generating a Key Pair

```js
const keyPair = await crypto.subtle.generateKey(
  {
    name: "RSASSA-PKCS1-v1_5",
    modulusLength: 2048,
    publicExponent: new Uint8Array([1, 0, 1]),
    hash: "SHA-256",
  },
  true,          // extractable — can the key be exported?
  ["sign", "verify"]
)

// keyPair.privateKey — CryptoKey object
// keyPair.publicKey  — CryptoKey object
```

Setting `extractable: false` means the private key can never be read out — not even by your own JS. It can only be used to sign/decrypt within the browser session.

---

## Exporting a Key

```js
// Export private key as PKCS#8 (binary)
const exported = await crypto.subtle.exportKey("pkcs8", keyPair.privateKey)
// exported is an ArrayBuffer

// Export public key as SPKI
const pubExported = await crypto.subtle.exportKey("spki", keyPair.publicKey)
```

---

## The CryptoKey Object

A `CryptoKey` is an opaque object — you can't read the key material directly. It lives in browser memory (not localStorage, not sessionStorage). When the tab closes, it's gone.

```js
console.log(keyPair.privateKey)
// CryptoKey { type: "private", extractable: true, algorithm: {...}, usages: ["sign"] }
// The actual key bytes are not accessible from JS directly
```

To persist a `CryptoKey` across page reloads, store it in **IndexedDB** — the browser stores it securely and returns the same object back.

---

## Other Common Operations

```js
// Hash (SHA-256)
const hash = await crypto.subtle.digest("SHA-256", data)

// Sign data with private key
const signature = await crypto.subtle.sign("RSASSA-PKCS1-v1_5", privateKey, data)

// Encrypt with public key
const encrypted = await crypto.subtle.encrypt({ name: "RSA-OAEP" }, publicKey, data)

// Random values (for nonces, IDs)
const random = crypto.getRandomValues(new Uint8Array(16))
```

---

## Relation to X.509 Certificates

The Web Crypto API generates the raw key pair. To wrap the public key into an X.509 certificate (required by GCP's service account key upload), you need a library like **pkijs** — the Web Crypto API itself only handles the cryptographic primitives.

```js
// pkijs uses Web Crypto API under the hood
import * as pkijs from 'pkijs'
const cert = new pkijs.Certificate()
await cert.subjectPublicKeyInfo.importKey(keyPair.publicKey)
await cert.sign(keyPair.privateKey, "SHA-256")
```

---

## Security Notes

- Only available in **secure contexts** (HTTPS or localhost)
- `crypto.subtle` operations are non-blocking (Promise-based) — don't block the main thread
- Prefer `extractable: false` for keys that only need to sign/decrypt within the session
- `crypto.getRandomValues()` is cryptographically secure — use it instead of `Math.random()` for anything security-related

---

## Related Topics

- [[JavaScript Promises]] — all Web Crypto API operations are async/Promise-based
- [[Asynchronous Code]] — understanding async/await needed to use this API
- [[Transport Layer Security (TLS)]] — TLS uses the same underlying primitives (RSA, SHA-256)
