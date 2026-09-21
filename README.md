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

| Feature | HTTP | HTTPS |
|---|---|---|
| Protocol | HTTP | HTTP over TLS |
| Default Port | 80 | 443 |
| Encryption | No | Yes |
| Confidentiality | Not provided | Provided through TLS |
| Wireshark Application Data | Readable HTTP traffic | Encrypted TLS Application Data |
| Certificate | Not required | Required for TLS |
| Network Protection | Limited | Provides confidentiality through encryption |

The Wireshark captures demonstrated the difference between the two protocols. HTTP traffic exposed identifiable HTTP requests and responses, while HTTPS traffic appeared as TLS 1.3 encrypted application data.

## Security Findings

- **Symmetric encryption:** AES-256-CBC successfully protected the plaintext data and allowed the original content to be recovered through the correct decryption process.
- **Asymmetric encryption:** RSA-2048 demonstrated how a public key can be used for encryption while the corresponding private key is required for decryption.
- **HTTP exposure:** Wireshark showed that HTTP requests and responses could be identified directly in captured traffic.
- **HTTPS protection:** TLS 1.3 encrypted the application data exchanged during the HTTPS session, preventing the HTTP content from being directly viewed in the packet capture.
- **Certificate trust:** The self-signed certificate provided encryption for the controlled laboratory environment but was not publicly trusted by the browser.
- **Key protection:** Private cryptographic keys must remain confidential and should never be exposed or committed to a public repository.

## Lessons Learned

- Symmetric encryption is efficient for protecting data when the required secret can be securely established.
- Asymmetric encryption separates encryption and decryption roles through a public/private key pair.
- RSA is appropriate for relatively small amounts of data; larger data sets are commonly protected using hybrid cryptographic approaches.
- HTTPS uses TLS to protect HTTP communication rather than simply replacing HTTP with a different application protocol.
- Wireshark can provide useful visibility into network protocols and demonstrate the difference between readable HTTP traffic and encrypted TLS traffic.
- Certificate trust is important in secure web communication. Self-signed certificates are useful in controlled laboratories but are not automatically trusted by browsers.
- Cryptographic private keys must be protected and should never be committed to a public GitHub repository.

## Evidence

Selected screenshots from the practical laboratory exercises are available in the [`evidence/`](./evidence/) directory.

### Symmetric Encryption

- [AES-256-CBC encryption](./evidence/01-aes-encryption.png)
- [Successful AES decryption and verification](./evidence/02-aes-successful-decryption.png)

### Asymmetric Encryption

- [RSA-2048 key generation](./evidence/03-rsa-private-key-generation.png)
- [Successful RSA decryption and verification](./evidence/06-rsa-successful-decryption.png)

### HTTP

- [Apache web server over HTTP](./evidence/07-http-web-page.png)
- [HTTP traffic captured in Wireshark](./evidence/08-wireshark-http-traffic.png)

### HTTPS and TLS

- [HTTPS browser certificate warning](./evidence/10-https-browser-warning.png)
- [Encrypted TLS 1.3 application data](./evidence/12-wireshark-encrypted-data.png)

## Conclusion

This project demonstrated practical applications of symmetric and asymmetric cryptography and showed how TLS can protect web communication.

AES-256-CBC was used for symmetric encryption, while RSA-2048 with OAEP and SHA-256 was used for asymmetric encryption. Apache was then configured for HTTP and HTTPS, allowing Wireshark to demonstrate the difference between readable HTTP traffic and encrypted TLS 1.3 application data.

The project provided practical experience with encryption, key management, SSL/TLS configuration, web-server security, and network traffic analysis in a controlled cybersecurity laboratory.
