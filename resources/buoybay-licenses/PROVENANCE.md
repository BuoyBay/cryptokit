# Provenance

Original focused Buoy wrappers written for this workspace. Crypto primitives and entropy are delegated to the installed Buoy crypto 1.0.0 package, whose source `modules/Crypto.bui` and installed `/usr/share/buoy/docs/documentation/crypto.md` were inspected before implementation. AES-GCM AAD overloads and Optional authentication-failure behavior come from that maintained API. UUIDKit supplied local packaging and entropy-error conventions. No external implementation code was copied.

Envelope BCKE v1 is a local serialization contract; it is not a standardized protocol. Base64url uses the standard RFC 4648 URL alphabet without padding. The fixed AES-256-GCM zero-key/nonce/block reference is additionally challenged with independently generated Python cryptography AESGCM vectors, nonempty AAD, and Buoy-to-Python decryption. Such tests do not establish full cryptographic conformance. The test environment's exact Crypto archive SHA-256 is recorded in evidence.json.

The package does not add raw FFI declarations or handwritten cipher, hash, MAC, KDF, or random generator primitives. No secret zeroization, production cryptographic audit, cross-platform native execution, or novel cryptographic construction guarantee is claimed.
