# NETWORKWALKS-SAMUEL-B084N-WEEK3-PM1-2-CYBERSECURITY-PASSWORD-CRACKING
Cybersecurity &amp; Ethical Hacking internship project (Week 3) focusing on cryptographic hash extraction, encryption vs. hashing concepts, and dictionary-based password auditing using John the Ripper (JTR), Johnny GUI, and NetworkWalks web tools.

# Cybersecurity & Ethical Hacking Internship – Week 3 Report
**Cryptographic Hash Extraction & Password Auditing (`W3-PM1` & `W3-PM2`)**

**Author:** Samuel Ayomide Adekoya (Batch B084)  
**Organization:** NetworkWalks Training Academy  
**Instructor:** Waqas Karim (CCIE)  
**Date:** November 2026  

---

## 1. Authorization & Ethical Compliance Disclaimer
> **Disclaimer:** All password auditing, hash extraction, and dictionary attack exercises documented in this report were performed strictly within an authorized educational lab environment using sample encrypted files (`My Locked PDF1.pdf`) provided by NetworkWalks Training Academy. The methodologies demonstrated herein are intended solely for defensive security auditing, password policy evaluation, and educational purposes.

---

## 2. Executive Summary
During Week 3 of the Cybersecurity & Ethical Hacking internship, two hands-on project modules were completed focusing on cryptographic hashing, encryption concepts, and password strength auditing:

* **Project Module 1 (`W3-PM1`):** Local hash extraction and dictionary-based password recovery using **John the Ripper (JTR)** CLI and the **Johnny** graphical user interface.
* **Project Module 2 (`W3-PM2`):** Browser-based local hash parsing and dictionary attack execution using the **NetworkWalks Hash Calculator** and **NetworkWalks Password Cracker** utilities.

Both methodologies successfully extracted the cryptographic hash of the target document (`My Locked PDF2.pdf`), recovered the plaintext password (`password1`), and decrypted the file to verify authorization and capture the lab flag.

---

## 3. Core Concepts: Encryption vs. Hashing
Understanding how protected files store credentials is essential for security auditing:

| Property | Encryption | Hashing |
| :--- | :--- | :--- |
| **Directionality** | **Two-way function** (reversible with the decryption key) | **One-way function** (irreversible message digest) |
| **Primary Purpose** | Confidentiality and protecting sensitive data | Integrity validation and credential verification |
| **Auditing Approach** | Decryption using authorized keys or exploiting weak ciphers | Extracting the hash and running dictionary/brute-force comparisons |

---

## 4. Module 1 (`W3-PM1`): Password Auditing with John the Ripper & Johnny

### 4.1 Tools & Environment
* **Operating System:** Windows 11 (x64)
* **Core Engine:** John the Ripper `1.9.0-jumbo-1` (`john.exe`)
* **GUI Frontend:** Johnny `v2.2`
* **Hash Extraction:** `pdf2john` format extractor (`$pdf$4*4*128*...`)

### 4.2 Execution Workflow
1. **Environment Configuration:**
   * Installed Johnny GUI and configured the backend binary path under **Settings** to point directly to `...\john-1.9.0-jumbo-1-win64\run\john.exe`.
   * Verified engine detection (`Detected John the Ripper 1.9.0-jumbo-1 OMP`).
    ![](Screenshot%20(648).png)

2. **Cryptographic Hash Extraction:**
   * Processed `My Locked PDF1.pdf` to extract its `pdf2john`-compatible hash string starting with `$pdf$4*4*128*...`.
   * Saved the clean hash string into a plain-text file (`hash1.txt`).
   ![](Screenshot%20(660).png)

3. **Dictionary Attack Execution:**
   * Loaded `hash1.txt` into Johnny via **Open password file (PASSWD format)**.
   * Initiated the audit via **Start new attack**.
   * John the Ripper matched the hash against its built-in wordlist rules and recovered the plaintext password: **`good-luck`**.
   ![](Screenshot%20(652).png)

4. **Verification:**
   * Opened `My Locked PDF1.pdf` in Google Chrome, entered `good-luck`, and confirmed access to the target flag document.
   ![](Screenshot%20(656).png)

---

## 5. Module 2 (`W3-PM2`): Password Auditing with NetworkWalks Web Tools

### 5.1 Tools & Environment
* **Hash Extraction:** [NetworkWalks Hash Calculator](https://networkwalks.com/hash-calculator/) (client-side local PDF parsing)
* **Dictionary Attack:** [NetworkWalks Password Cracker](https://networkwalks.com/password-cracker/) (browser-based dictionary engine)
### 5.2 Execution Workflow
1. **Client-Side Hash Extraction:**
   * Loaded `My Locked PDF2.pdf` (65.2 KB) into the NetworkWalks Hash Calculator.
   * Verified encryption metadata (**Revision: R4 | Version: V4 | Key length: 128-bit**) and copied the generated `$pdf$` hash string.
   ![](Screenshot%20(661).png)

2. **Browser-Based Dictionary Attack:**
   * Pasted the full `$pdf$` hash string into the NetworkWalks Password Cracker.
   * Executed the dictionary attack using the active built-in wordlist (100 common passwords).
   * At entry `91 / 100` (`9 pw/s`), the tool identified a cryptographic match: **`password1`**.
   ![](Screenshot%20(659).png)

3. **Verification:**
   * Verified the recovered credential against the encrypted PDF to unlock the document and confirm flag capture.
   ![](Screenshot%20(654).png)

---

## 6. Findings & Defensive Recommendations

### Risk Assessment Summary
| Finding | Severity | Observation |
| :--- | :--- | :--- |
| **Weak / Predictable Password (`password1`)** | **High** | Recovered almost instantaneously by both JTR and a 100-word dictionary list because it relies on a dictionary root word plus a single trailing digit. |
| **Legacy PDF Encryption Revision (R4 / 128-bit)** | **Medium** | Older PDF encryption revisions allow high-speed offline hash extraction and rapid candidate testing. |

### Remediation & Hardening Guidelines
1. **Enforce High-Entropy Passwords:** Replace dictionary words and predictable suffixes (`1`, `123`, `!`) with passphrases of **14+ characters** combining unrelated words, numbers, and symbols.
2. **Upgrade Document Encryption Standards:** Use modern **AES-256 (PDF 2.0 / Revision 6)** encryption when protecting sensitive documents to increase computational resistance against offline cracking.
3. **Implement Multi-Factor & Access Controls:** Avoid relying solely on static file passwords for sensitive distribution; use identity-based access controls, DRM, or encrypted containers.
