# CryptoKit standalone consumer

Copy main.bui into an otherwise empty directory with packages/ containing crypto-1.0.0.buipkg and cryptokit-0.1.0.buipkg. From that directory:

```
buoy main.bui --check --diagnostic-format json
buoy main.bui -o app
./app
```

The example uses a fresh caller-owned key and explicit AAD, seals and authenticates `hello`, then generates a token and a bounded integer. Do not print real application keys. The example's temporary key is not persistent storage. The repository verifier builds and runs this example as a fresh sealed consumer.
