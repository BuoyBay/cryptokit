# Secure tokens and authenticated AES-GCM envelopes

Focused wrappers over the maintained OpenSSL-backed `crypto` package. No new cryptographic primitive implementation. Version 0.1.0 requires Buoy 0.72.0 and `crypto-1.0.0.buipkg` alongside the sealed CryptoKit archive in the consumer's `packages/` directory. `Import cryptokit` exposes the functions below. The dependency must be supplied separately; bpm.json follows this repository's UUIDKit convention rather than claiming an automatically resolved dependency.

Keys are raw caller-managed cryptographic keys. No password or passphrase shortcut is offered. Generate keys from `SecureTokenBytes(32)` or use an appropriate external key management/derivation system. Byte buffers are mutable references; functions retain no inputs after return. Do not mutate inputs concurrently. No secret zeroization guarantee is made.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add cryptokit --git https://github.com/BuoyBay/cryptokit.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## Example

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

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer cryptokit
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```


## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
