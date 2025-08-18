# **HTTPS, TLS, Keys and Certificates**

HTTPS is the **secure version of HTTP**, using **TLS (Transport Layer Security)** to encrypt communication between the client and server. It protects against eavesdropping, tampering, and impersonation.

---

## **What It Does**

* **Encrypts** data in transit so third parties can’t read or alter it.

* **Authenticates** that the server you’re talking to is the real one (via certificates).

* **Prevents MITM attacks** (Man-in-the-Middle).

---

## **Where It’s Used**

* **Websites** with login forms, e-commerce, or personal data

* **APIs** (public/private)

* **Mobile and desktop apps** communicating with remote servers

* **Search engines** prioritize HTTPS over HTTP

---

## **How It Works**

### **1. TLS Handshake (Simplified)**

```
Client                 Server
  |  ---- ClientHello ---> |
  |  <--- ServerHello ---- |
  |  <--- Certificate -----|
  |  ---- Key Exchange --> |
  |  ---- Finished ------> |
  |  <--- Finished ------- |
```

* **ClientHello**: proposes TLS version, cipher suites, and sends a random number

* **ServerHello**: picks a cipher suite and sends its certificate (includes its public key)

* **Key Exchange**: securely exchange a **shared session key**

* **Finished**: both sides confirm the key was successfully exchanged

Then, all HTTP data is encrypted using the **shared session key**.

---

## **TLS vs SSL**

|  **Protocol**  |  **Status**  |  **Notes**  | 
|---|---|---|
|  SSL  |  Deprecated  |  Do **not** use it  | 
|  TLS 1.2  |  Common  |  Still widely supported  |
|  TLS 1.3  |  Modern  |  Faster, more secure handshake  |
> TLS replaced SSL. The term **SSL** is still used informally, but technically **TLS** is what powers HTTPS today.

---

## **Keys and Certificates**

### **🔐 Asymmetric Encryption (TLS Setup)**

* **Public Key**: shared with everyone

* **Private Key**: kept secret on the server

* Used to securely exchange a **symmetric key** (faster encryption)

### **🔑 Symmetric Encryption (TLS Data Transfer)**

* After handshake, a **shared session key** encrypts all traffic

* Faster than asymmetric, used for performance

---

## **What is an SSL/TLS Certificate?**

A **digital file** containing:

* Server’s **public key**

* Server identity (domain name, organization, etc.)

* **Digital signature** by a Certificate Authority (CA)

### **Example:**

```
Subject: CN=ahmedikram.org
Issuer: Let's Encrypt
Public Key: [encoded]
Signature: [signed hash]
```

---

## **Certificate Authorities (CAs)**

* Trusted third parties like **Let’s Encrypt**, **DigiCert**, etc.

* Their job: **verify your domain identity** and **sign your certificate**

* Browsers trust a set of built-in CAs

---

## **Chain of Trust**

```
Browser trusts → Root CA
                → Intermediate CA
                  → Your Site Certificate
```

> All certs must form a **valid chain** up to a trusted root.

---

## **HTTPS URL Flow**

1. You type: https://ahmedikram.org

2. Browser connects via TCP (usually port 443)

3. TLS handshake begins

4. Certificate verified

5. Encrypted connection established

6. Encrypted HTTP data exchanged

⠀
---

## **TLS Debugging Tips**

* Use openssl s_client -connect yoursite.com:443 to view certificate

* Use [https://www.ssllabs.com/ssltest](https://www.ssllabs.com/ssltest) for public audit

* Watch for:

  * **Expired certs**

  * **Self-signed** (untrusted) certs

  * **Wrong domain name** in cert

  * **Mixed content** (HTTP on HTTPS page)

---

## **Real-World Analogy**

> Think of the public key as a **locked mailbox**—anyone can drop a message inside (encrypt), but only the owner with the **private key** can open it.

---

## **TL;DR**

> HTTPS = HTTP + TLS

> TLS uses certificates, public/private keys, and a session key to encrypt communication.

> Always use HTTPS for any sensitive or production traffic.

---

#backend