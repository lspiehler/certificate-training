## Prerequisites

- [Install OpenSSL](https://slproweb.com/products/Win32OpenSSL.html) - Preferably "Win64 OpenSSL v3.5.4 Light"
- Update PATH `[Environment]::SetEnvironmentVariable("Path", ([Environment]::GetEnvironmentVariable("Path", "User") + ";C:\Program Files\OpenSSL-Win64\bin"), "User")`
- Restart terminal

## Keys types and sizes

* Asymmetric cryptography vs symmetric cryptography
* Two encodings DER (binary) and PEM (Base64-encoded text)
* Two formats: PKCS#8 (modern) and PKCS#1 (traditional)
* Can be encrypted or unencrypted
* Generating a private key also generates a public key
* Key size matters
* Hashing algorithm has nothing to do with key generation

### RSA
#### Legacy
```
openssl genrsa 2048
```
#### Modern
```
openssl genpkey -outform PEM -algorithm RSA -pkeyopt rsa_keygen_bits:2048
```
### ECC
#### Legacy
```
openssl ecparam -name prime256v1 -genkey -noout
```
#### Modern
```
openssl genpkey -algorithm EC -pkeyopt ec_paramgen_curve:P-256
```
### Post-Quantum Cryptography ([NIST Recently Released Post Quantum Encryption Standards](https://www.nist.gov/news-events/news/2024/08/nist-releases-first-3-finalized-post-quantum-encryption-standards))
#### ML-DSA
```
openssl genpkey -algorithm mldsa65
```
#### SLH-DSA
```
openssl genpkey -algorithm slh-dsa-sha2-128s
```

https://slproweb.com/products/Win32OpenSSL.html
CSRs and generation methods
	- always starts with a key
	- OpenSSL
	- Windows
Best way to examine certs is with ASN1, demonstrate UTF8 vs PRINTABLESTRING
SHA1 deprecated, but doesn't matter for CSR
Workflows
	- Customer wants a CSR from us, could be public or private
	- Customer gives us access to request/sign CSRs, usually only private
	- Customer provides us a certificate
Private vs public - Public has strict rules (attributes, expiration), private can do mostly what it wants except with key size and signature algorithm
Caddy lab
IIS lab

Open floor for questions and 