# tls-1.2-vs-tls-1.3
Packet-level comparison of TLS 1.2 and TLS 1.3 using real HTTPS traffic captured in Wireshark
#TLS 1.2 vs TLS 1.3 — Packet Level Analysis
Objective

The goal of this project is to analyze the real differences between TLS 1.2 and TLS 1.3 by capturing and inspecting live HTTPS traffic using Wireshark.
Instead of relying on documentation, this lab verifies how modern browsers and servers negotiate encryption in real network packets.

## Tools Used
- Wireshark
- Google Chrome / Firefox
- Public HTTPS websites
## What was analyzed
Two different HTTPS connections were captured:
- A server negotiating TLS 1.2

- A server negotiating TLS 1.3

From the TLS handshake packets, the following were extracted:
- TLS version used
- Cipher suite
- Encryption algorithm
- Key exchange method

## Core cryptographic design
TLS uses three main components:

- Function	Algorithm
- Key exchange	ECDHE
- Encryption	AES-GCM or ChaCha20
- Authentication	Certificates (RSA / ECDSA)

The key exchange creates a shared secret.
From this secret, encryption keys are derived and used by AES-GCM to protect data.
This allowed a direct comparison between both protocol versions at the network level.


## TLS 1.2 Design
TLS 1.2 supports multiple key exchange and encryption modes:
- RSA key exchange
- ECDHE key exchange
- CBC and GCM encryption modes
## Several legacy ciphers

This creates two problems:
- Servers could be configured without forward secrecy (RSA key exchange)
- Weak and legacy ciphers could still be negotiated
- If a server used RSA key exchange and its private key was ever compromised, previously recorded traffic could be decrypted.

## Why TLS 1.3 was needed

TLS 1.2 tried to remain compatible with old systems, but this left dangerous options enabled.
TLS 1.3 was designed to remove everything that had caused real-world vulnerabilities.
It enforces modern cryptography by default and significantly reduces the attack surface of HTTPS connections.

## Why AEAD matters

AEAD (Authenticated Encryption with Associated Data) provides:
- Confidentiality (encryption)
- Integrity (tampering detection)
- Authentication
AES-GCM and ChaCha20-Poly1305 are AEAD ciphers.
They replace older constructions like AES-CBC + HMAC, which were vulnerable to padding and timing attacks.
