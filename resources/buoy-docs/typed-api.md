---
title: "cryptokit typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["CryptoBackendException", "CryptoEntropyException"]
dependencies: []
symbols: ["CryptoBackendException", "CryptoEntropyException"]
related: ["api.md", "getting-started.md", "index.md"]
---

# cryptokit typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## CryptoBackendException

`Public Class CryptoBackendException Inherits Exception`

Operational failure after caller arguments have been validated.

### Constructor

`Public Sub Constructor(message As String)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `message` | `String` | `ByVal` | Required |

Returns: no value.

## CryptoEntropyException

`Public Class CryptoEntropyException Inherits CryptoBackendException`

Secure entropy acquisition failed; never substitutes a fallback source.

### Constructor

`Public Sub Constructor(message As String)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `message` | `String` | `ByVal` | Required |

Returns: no value.

### SecureTokenBytes

`Public Function SecureTokenBytes(count As Integer) As Bytes`

Fresh secure bytes; 0 through 1048576 bytes. Entropy failures propagate.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `count` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Bytes` |

### SecureTokenHex

`Public Function SecureTokenHex(count As Integer) As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `count` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### SecureTokenBase64Url

`Public Function SecureTokenBase64Url(count As Integer) As String`

Unpadded RFC 4648 base64url; count is input bytes, not output characters.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `count` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### SecureRandomInteger

`Public Function SecureRandomInteger(lower As Integer, upperExclusive As Integer) As Integer`

Uniform in [lower, upperExclusive); width must fit positive signed Integer.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `lower` | `Integer` | `ByVal` | Required |
| `upperExclusive` | `Integer` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Integer` |

### SealEnvelope

`Public Function SealEnvelope(key As Bytes, plaintext As Bytes, aad As Bytes) As Bytes`

BCKE v1 AES-256-GCM envelope; owns fresh random 12-byte nonce.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `Bytes` | `ByVal` | Required |
| `plaintext` | `Bytes` | `ByVal` | Required |
| `aad` | `Bytes` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Bytes` |

### OpenEnvelope

`Public Function OpenEnvelope(key As Bytes, envelope As Bytes, aad As Bytes) As Optional Of Bytes`

None for malformed, oversized or unauthentic envelopes; never returns unauthenticated plaintext.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `key` | `Bytes` | `ByVal` | Required |
| `envelope` | `Bytes` | `ByVal` | Required |
| `aad` | `Bytes` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Optional Of Bytes` |
