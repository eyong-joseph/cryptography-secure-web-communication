# Cryptography Fundamentals & Secure Web Cmmunication

## Project Overview

This project demonstrates practical applications of cryptography and secure web communication in a controlled Kali Linux laboratory environment.

The project covers symmetric encryption using AES-256-CBC, asymmetric encryption using RSA-2048, deployment of an Apache web server, configuration of HTTPS using a self-signed X.509 certificate, and network traffic analysis using Wireshark.

The project compares HTTP and HTTPS traffic to demonstrate how TLS protects web communication from being transmitted as readable application-layer data.

## Project Objectives

- Demonstrate symmetric encryption and decryption using AES-256-CBC and PBKDF2.
- Generate and use an RSA-2048 public/private key pair for asymmetric encryption and decryption.
- Deploy an Apache web server and demonstrate HTTP communication.
- Capture and analyze HTTP traffic using Wireshark.
- Configure HTTPS using a self-signed X.509 SSL/TLS certificate.
- Capture and analyze TLS 1.3 traffic and encrypted application data.
- Compare HTTP and HTTPS to demonstrate the security benefits of encrypted web communication.

## Lab Environment
| Component | Configuration |
|---|---|
| Virtualization | Oracle VirtualBox |
| Operating System | Kali Linux 2026.1 |
| Web Server | Apache2 |
| Cryptography Tool | OpenSSL |
| Symmetric Algorithm | AES-256-CBC |
| Key Derivation | PBKDF2 |
| Asymmetric Algorithm | RSA-2048 |
| Web Protocols | HTTP and HTTPS |
| Certificate | Self-signed X.509 certificate |
| Traffic Analysis | Wireshark |

## Architecture

Plaintext
   |
   +--> AES-256-CBC Encryption
   |          |
   |          v
   |     Encrypted Data
   |
   +--> RSA-2048 Public-Key Encryption
              |
              v
         Encrypted Data

Apache Web Server
       |
       +--> HTTP ---------> Wireshark
       |
       +--> HTTPS
              |
              v
          TLS 1.3
              |
              v
          Wireshark

The project demonstrates cryptographic protection of data and secure web communication. AES-256-CBC is used for symmetric encryption, while RSA-2048 with OAEP is used for asymmetric encryption. Apache is then accessed through HTTP and HTTPS, with Wireshark used to compare the resulting network traffic.

## Scenario 1 - Symmetric Encryption

AES-256-CBC was used to demonstrate symmetric encryption and decryption.

A plaintext file was encrypted using OpenSSL with AES-256-CBC, a unique salt, and PBKDF2 for password-based key derivation.

```bash
openssl enc -aes-256-cbc -salt -pbkdf2 -in plaintext.txt -out encrypted.txt
```

The encrypted file was then decrypted using the same cryptographic parameters:

openssl enc -d -aes-256-cbc -pbkdf2 -in encrypted.txt -out decrypted.txt

The recovered plaintext was compared with the original using cmp. The files were identical, confirming successful encryption and decryption.

Key Security Concept

AES provides symmetric encryption, meaning the same secret-derived key is used for encryption and decryption. PBKDF2 strengthens password-based key derivation by deriving cryptographic key material from the password using a salt and repeated computation.

## Scenario 2 - Asymmetric Encryption

RSA-2048 was used to demonstrate asymmetric encryption using a public/private key pair.

The private key was generated first, and the corresponding public key was derived from it:

```bash
openssl genrsa -out private_key.pem 2048
openssl rsa -in private_key.pem -pubout -out public_key.pem
```

A short plaintext message was encrypted using the public key with RSA-OAEP and SHA-256:

```bash
openssl pkeyutl -encrypt -pubin -inkey public_key.pem \
-in rsa_plaintext.txt \
-out rsa_encrypted.bin \
-pkeyopt rsa_padding_mode:oaep \
-pkeyopt rsa_oaep_md:sha256 \
-pkeyopt rsa_mgf1_md:sha256
```

The encrypted message was then decrypted using the corresponding private key. The decrypted output was compared with the original plaintext, confirming successful RSA encryption and decryption.

Key Security Concept

The public key can be shared for encryption, while the private key must remain confidential. RSA is generally used for relatively small amounts of data or as part of a hybrid cryptographic system rather than for encrypting large files directly.

## Scenarion 3 - HTTP Web Server

Apache2 was deployed on the Kali Linux virtual machine to demonstrate basic HTTP communication.

The web server was installed and started using:

```bash
sudo apt update
sudo apt install -y apache2
sudo systemctl enable --now apache2
```

A custom HTML page was placed in Apache's default web directory and accessed through the server's IP address using HTTP.
Wireshark was used to capture the traffic. The http display filter revealed an HTTP GET / request followed by an HTTP/1.1 200 OK response.

Security Observation

HTTP does not provide encryption at the protocol level. The captured request and response could therefore be identified directly in the network traffic.

## Scenario 4 - HTTPS and TLS

HTTPS was configured on Apache using a self-signed X.509 SSL/TLS certificate generated with OpenSSL.

The Apache SSL configuration was updated to use the generated certificate and private key. The configuration was validated successfully and Apache was restarted.

The HTTPS connection was then accessed through the browser. Because the certificate was self-signed and not issued by a trusted Certificate Authority, the browser displayed a security warning. The certificate was accepted for this controlled laboratory environment.

Wireshark was used to analyze the HTTPS connection. The TLS handshake was identified using:

```text
tls.handshake
```
The capture showed TLS 1.3 Client Hello and Server Hello messages

The TLS application data was then isolated using:

```bash
tls.app_data && ip.src == 10.0.2.15 && ip.dst == 10.0.2.15
```
The resulting packets appeared as TLS 1.3 Application Data rather than readable HTTP requests or responses.

Security Observation

HTTPS uses TLS to provide confidentiality for HTTP communication. The Wireshark capture demonstrated that, unlike the HTTP traffic, the application data exchanged over HTTPS was not directly readable as HTTP content.

## HTTP vs HTTPS

## Security Findings

## Lessons Learned

## Evidence

## Conclusion
