---
title: "CryptoKit API"
summary: "Public signatures and failure contracts."
category: "Reference"
order: 20
---

# CryptoKit

Focused wrappers over Buoy's maintained OpenSSL-backed `crypto` package. No new cryptographic primitive implementation. Version 0.1.0 requires Buoy 0.72.0 and `crypto-1.0.0.buipkg` alongside the sealed CryptoKit archive in the consumer's `packages/` directory. `Import cryptokit` exposes the functions below. The dependency must be supplied separately; bpm.json follows this repository's UUIDKit convention rather than claiming an automatically resolved dependency.

| API | Contract |
| --- | --- |
| `SecureTokenBytes(count As Integer) As Bytes` | Fresh secure random bytes, count 0..1048576 inclusive. |
| `SecureTokenHex(count As Integer) As String` | Lowercase hex, exactly 2*count characters. |
| `SecureTokenBase64Url(count As Integer) As String` | Unpadded base64url, ceil(8*count/6) characters; count means entropy bytes. |
| `SecureRandomInteger(lower As Integer, upperExclusive As Integer) As Integer` | Uniform half-open range; positive width must fit signed 64-bit Integer. |
| `SealEnvelope(key As Bytes, plaintext As Bytes, aad As Bytes) As Bytes` | AES-256-GCM; caller supplies exactly 32 key bytes; wrapper generates nonce. |
| `OpenEnvelope(key As Bytes, envelope As Bytes, aad As Bytes) As Optional Of Bytes` | Authenticated plaintext only; None for malformed, oversized, truncated, altered, wrong-key or wrong-AAD input; invalid key length/Nil key throws. |

Keys are raw caller-managed cryptographic keys. No password or passphrase shortcut is offered. Generate keys from `SecureTokenBytes(32)` or use an appropriate external key management/derivation system. Byte buffers are mutable references; functions retain no inputs after return. Do not mutate inputs concurrently. No secret zeroization guarantee is made.

## Envelope wire format

Binary concatenation, no text encoding or length prefixes:

| Offset | Bytes | Meaning |
| --- | --- | --- |
| 0 | 4 | ASCII `BCKE` (42 43 4b 45 hex) |
| 4 | 1 | Version 1 |
| 5 | 1 | Algorithm 1: AES-256-GCM |
| 6 | 2 | Reserved, must both be zero |
| 8 | 12 | Fresh cryptographically random 96-bit nonce |
| 20 | variable | Ciphertext followed by 16-byte GCM tag |

GCM AAD is exactly the eight-byte header concatenated with the caller's AAD. Its fixed header width makes the split unambiguous. The nonce is supplied to GCM as its nonce parameter. Every byte affects parsing or authentication. No algorithm negotiation, fallback, padding, key ID, hidden metadata, or trailing bytes are accepted. Applications must frame separate envelopes themselves and supply matching AAD externally. Header/AAD are authenticated, not encrypted. Plaintext length is exposed.

Plaintext and caller AAD each permit at most 1048576 bytes. Seal checks both before constructing copies or invoking Crypto. Open checks total wire length 36..1048612 and AAD <=1048576 before allocating header/nonce/ciphertext/auth-data copies or invoking Crypto. Input buffers already exist and are outside this budget; this is a per-field byte limit, not a total process-memory ceiling. Multiple bounded copies coexist. Empty plaintext/AAD are valid. Nil plaintext/AAD on seal, invalid key length/Nil key, invalid token sizes, empty random ranges and widths exceeding Integer throw InvalidArgumentException. Nil envelope/AAD on open returns None. Entropy acquisition errors, including missing or incorrectly sized output, throw CryptoEntropyException, a subclass of CryptoBackendException. Exceptions raised by backend encryption or decryption calls are wrapped as CryptoBackendException. Caller validation errors remain InvalidArgumentException. The backend can also return None for internal decryption failures, so None does not distinguish authentication failure from backend failure.

## Security and limits

Nonce generation uses Crypto.RandomBytes, as UUIDKit does for UUIDv4 entropy. Random nonces cannot guarantee uniqueness: after q encryptions under one key, collision probability is approximately q(q-1)/2^97 while small. Rotate keys and impose application-specific message limits; this package provides no durable counter, per-key usage tracking, replay protection, key storage, streaming, or misuse-resistant encryption. Never replace the generator with a deterministic fixture in production. Failed decryptions never expose plaintext through this API, but no memory-erasure claim is made about backend buffers.

Integer sampling draws eight random bytes, masks the first to seven bits and accumulates without overflow. It accepts candidate < L where L = MAX_INT - (MAX_INT Mod width), then returns lower + candidate Mod width. Thus every residue has exactly L/width accepted preimages. L <= MAX_INT deliberately rejects a few extra candidates (including MAX_INT for width 1). Each retry uses fresh entropy; no retry cap or fallback introduces bias. The full signed interval and widths >=2^63 are deliberately unsupported. Negative-only and crossing-zero ranges with representable widths are supported, including a singleton at MIN_INT.

Incremental hashing is deferred: the installed Crypto API exposes whole-buffer hashing, not an incremental state. Buffering entire input would not provide streaming semantics.

## Verification


Maintainers run the private validation suite before release. See the repository README for public build instructions.

