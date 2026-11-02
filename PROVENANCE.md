# Provenance

Independent focused Buoy wrappers. Cryptographic primitives and entropy are delegated to the installed Buoy crypto package. AES-GCM AAD handling and authentication-failure behavior follow that API; no external implementation code was copied.

Envelope BCKE v1 is a local serialization contract, not a standardized protocol. Base64url uses the RFC 4648 URL alphabet without padding. Cryptographic conformance and production security review are outside this wrapper's guarantees.

The package does not add raw FFI declarations or handwritten cipher, hash, MAC, KDF, or random generator primitives. No secret zeroization, production cryptographic audit, cross-platform native execution, or novel cryptographic construction guarantee is claimed.
