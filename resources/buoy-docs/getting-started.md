---
title: "CryptoKit getting started"
summary: "Install Crypto and CryptoKit sealed archives."
category: "Reference"
order: 10
---

Place `crypto-1.0.0.buipkg` and `cryptokit-0.1.0.buipkg` in `packages/`. `Import cryptokit`. The standalone example is `examples/cryptokit/main.bui`. Keys must contain exactly 32 bytes; pass identical AAD to sealing and opening. Inspect `HasValue` before reading an opened `Optional`.

Save the following as `main.bui` in your consumer project. Package archives and any dependencies must be installed as described above.

```basic
Import cryptokit
Sub Main()
    Dim key As Bytes = SecureTokenBytes(32)
    Dim aad As Bytes = Bytes.FromString("example/context", Text.Encodings.UTF8)
    Dim sealed As Bytes = SealEnvelope(key, Bytes.FromString("hello", Text.Encodings.UTF8), aad)
    Dim opened As Optional Of Bytes = OpenEnvelope(key, sealed, aad)
    If Not opened.HasValue Then Throw New Exception("Authentication failed")
    Print opened.Value.ToString(Text.Encodings.UTF8)
    Print SecureTokenBase64Url(24)
    Print SecureRandomInteger(-10, 10)
End Sub
```
