# lab-week-10

# Vulnerability Analysis and Risk-Based Prioritization Report

**Course Lab Documentation | Vulnerability Assessment Lab** **Target Environment:** Metasploitable2 (`192.168.56.106`)  
**Attacker Platform:** Kali Linux  
<img width="467" height="251" alt="image" src="https://github.com/user-attachments/assets/f7d04914-e408-45d2-811b-08dc1e6f3100" />

---

## LAB 1: Foundation (Host-Based Vulnerability Assessment)

### 1. Assessment Scope & Objective
* **Target Host:** Metasploitable2 (`192.168.56.106`)
* **Purpose:** To perform a credential-less or credentialed host-based vulnerability assessment to identify, classify, and prioritize potential security risks without active exploitation.
* **Scan Metrics Overview:** Total Identified Vulnerabilities = 111

### 2. High-Severity Scan Findings
The automated scanner identified multiple high-risk flaws across various infrastructure layers:

<img width="562" height="317" alt="image" src="https://github.com/user-attachments/assets/a7802084-c5d9-4f4a-a235-7614503e1194" />


### 3. Top 5 Prioritized Vulnerabilities
From the raw results, the top 5 vulnerabilities extracted for advanced correlation are:

| Vulnerabilities | CVE ID | CVSS Score | Severity |
| :--- | :---: | :---: | :---: |
| Apache Tomcat AJP Connector Request Injection (Ghostcat) | CVE-2020-1938 | 9.8 | Critical |
| SSL V2 and 3 Protocol Detection | CVE-2014-3566 | 9.8 | Critical |
| VNC Server Default Password (`password`) Detection | CVE-1999-0506 | 10.0 | Critical |
| NFS Exported Share Information Disclosure | CVE-1999-0554 | 10.0 | Critical |
| UnrealIRCd Backdoor Detection | CVE-2010-2075 | 10.0 | Critical |

---

## LAB 2: Core Analyst Skill (CVE, CVSS & CWE Correlation)

Detailed manual lookup and structural correlation of findings to determine environmental exploitability:

### Vulnerability 1: UnrealIRCd Backdoor Detection
* **CVE Lookup:** `CVE-2010-2075`
* **CVSS Base Score:** 10.0 (Critical)
* **Attack Vector (AV):** Network (N) — The exploit can be executed remotely over the network without prior local system access.
* **Privileges Required (PR):** None (N) — The attacker does not need an account or any authenticated session to trigger the backdoor.
* **CWE Mapping:** * `CWE-78`: Improper Neutralization of Special Elements used in an OS Command (OS Command Injection)
  * `CWE-506`: Embedded Malicious Code

### Vulnerability 2: Apache Tomcat AJP Connector (Ghostcat)
* **CVE Lookup:** `CVE-2020-1938`
* **CVSS Base Score:** 9.8 (Critical)
* **Attack Vector (AV):** Network (N) — Accessible remotely over the network via port 8009.
* **Privileges Required (PR):** None (N) — Completely unauthenticated request.
* **CWE Mapping:** * `CWE-20`: Improper Input Validation
  * `CWE-94`: Improper Control of Generation of Code (Code Injection)

### Vulnerability 3: VNC Server Default Password Detection
* **CVE Lookup:** `CVE-1999-0506`
* **CVSS Base Score:** 10.0 (Critical)
* **Attack Vector (AV):** Network (N) — The authentication prompt is exposed over default port 5900.
* **Privileges Required (PR):** None (N) — No initial authentication credentials are required to attempt a login connection.
* **CWE Mapping:** * `CWE-1392`: Use of Default Credentials
  * `CWE-521`: Weak Password Requirements

---

## LAB 3: Real-World Scenario (False Positive Validation Exercise)

### Manual Verification Output
To ensure findings were true operational risks rather than scanner misinterpretations, an active Nmap verification scan was conducted against the target environment:

```text
5432/tcp open postgresql PostgreSQL DB 8.3.0 - 8.3.7
| ssl-enum-ciphers:
|   SSLv3:
|     ciphers:
|       TLS_DHE_RSA_WITH_3DES_EDE_CBC_SHA (dh 1024) - F
|       TLS_DHE_RSA_WITH_AES_128_CBC_SHA (dh 1024) - F
|       TLS_DHE_RSA_WITH_AES_256_CBC_SHA (dh 1024) - F
|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 1024) - F
|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 1024) - F
|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 1024) - F
|       TLS_RSA_WITH_RC4_128_SHA (rsa 1024) - F
|     compressors:
|       DEFLATE
|       NULL
|     cipher preference: client
|     warnings:
|       64-bit block cipher 3DES vulnerable to SWEET32 attack
|       Broken cipher RC4 is deprecated by RFC 7465
|       CBC-mode cipher in SSLv3 (CVE-2014-3566)
|       Insecure certificate signature (SHA1), score capped at F
|   TLSv1.0:
|     ciphers:
|       TLS_DHE_RSA_WITH_3DES_EDE_CBC_SHA (dh 1024) - F
|       TLS_DHE_RSA_WITH_AES_128_CBC_SHA (dh 1024) - F
|       TLS_DHE_RSA_WITH_AES_256_CBC_SHA (dh 1024) - F
|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 1024) - F
|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 1024) - F
|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 1024) - F
|       TLS_RSA_WITH_RC4_128_SHA (rsa 1024) - F
|     compressors:
|       DEFLATE
|       NULL
|     cipher preference: client
|     warnings:
|       64-bit block cipher 3DES vulnerable to SWEET32 attack
|       Broken cipher RC4 is deprecated by RFC 7465
|       Insecure certificate signature (SHA1), score capped at F
|_  least strength: F
