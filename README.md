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

## Scenario 2 - Asymmetric Encryption

## Scenarion 3 - HTTP Web Server

## Scenario 4 - HTTPS and TLS

## HTTP vs HTTPS

## Security Findings

## Lessons Learned

## Evidence

## Conclusion
