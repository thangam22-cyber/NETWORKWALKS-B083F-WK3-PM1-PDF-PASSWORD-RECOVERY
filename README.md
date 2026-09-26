# 🔐 Networkwalks B083F — Week 3

## PDF Password Recovery & Hash Cracking Lab

![Networkwalks](https://img.shields.io/badge/Networkwalks-Cybersecurity%20Internship-blue?style=for-the-badge\&logo=shield)
![Week](https://img.shields.io/badge/Week-03-purple?style=for-the-badge)
![Modules](https://img.shields.io/badge/W3--PM1%20%7C%20W3--PM2-brightgreen?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-black?style=for-the-badge\&logo=kalilinux)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

> **Cybersecurity Internship — Networkwalks | Batch B083F**
> **Intern:** Thangamani M
> **Module:** W3-PM1 & W3-PM2
> **Focus:** PDF Password Recovery, Hash Extraction & Dictionary Attacks

---

## 📌 Overview

This repository documents my **Week 3 cybersecurity lab** completed as part of the Networkwalks internship.

The objective of this exercise was to understand how password-protected PDF documents can be assessed through two different recovery workflows:

* **W3-PM1 — Offline Password Recovery:** PDF hash extraction with `pdf2john.pl` followed by dictionary-based password recovery using **John the Ripper (JTR)** in Kali Linux.
* **W3-PM2 — Online Lab Workflow:** `$pdf$` hash extraction using the **Networkwalks Hash Calculator**, followed by dictionary-based recovery using the **Networkwalks Password Cracker**.

The recovered passwords were then used to unlock the lab PDFs and verify the embedded challenge flags.

All testing was performed exclusively against **self-owned / internship-provided lab files with authorization**.

---

## 🎯 Learning Objectives

This lab focused on understanding:

* PDF password protection and `$pdf$` hashes
* PDF hash extraction using `pdf2john.pl`
* Dictionary-based password recovery
* John the Ripper workflow in Kali Linux
* Use of the `rockyou.txt` wordlist
* Browser-based password recovery workflows
* Password-strength assessment
* Verification through recovered document contents
* Basic cybersecurity evidence collection and troubleshooting

---

# 🧩 Lab Architecture

```text
                    ┌─────────────────────┐
                    │  Password-Protected │
                    │       PDF Files     │
                    └──────────┬──────────┘
                               │
                 ┌─────────────┴─────────────┐
                 │                           │
                 ▼                           ▼
        ┌─────────────────┐         ┌────────────────────┐
        │    W3-PM1       │         │      W3-PM2        │
        │  Offline JTR    │         │ Networkwalks Web   │
        └────────┬────────┘         └─────────┬──────────┘
                 │                            │
                 ▼                            ▼
        ┌─────────────────┐         ┌────────────────────┐
        │  pdf2john.pl    │         │  Hash Calculator   │
        └────────┬────────┘         └─────────┬──────────┘
                 │                            │
                 └─────────────┬──────────────┘
                               ▼
                         ┌─────────────┐
                         │   $pdf$     │
                         │    Hash     │
                         └──────┬──────┘
                                │
                    ┌───────────┴───────────┐
                    │                       │
                    ▼                       ▼
             ┌─────────────┐       ┌─────────────────┐
             │ John the    │       │ Networkwalks    │
             │ Ripper      │       │ Password Cracker│
             └──────┬──────┘       └────────┬────────┘
                    │                       │
                    └───────────┬───────────┘
                                ▼
                         ┌─────────────┐
                         │  Recovered  │
                         │  Password   │
                         └──────┬──────┘
                                ▼
                         ┌─────────────┐
                         │ PDF Unlock  │
                         │ + Flag      │
                         │ Verification│
                         └─────────────┘
```

---

# 🛠️ Tools & Technologies

| Tool / Component                    | Purpose                                    |
| ----------------------------------- | ------------------------------------------ |
| 🐉 **Kali Linux**                   | Offline security testing environment       |
| 🔓 **John the Ripper**              | Offline password recovery                  |
| 🧩 **pdf2john.pl**                  | Extract `$pdf$` hashes from encrypted PDFs |
| 📖 **rockyou.txt**                  | Dictionary wordlist                        |
| 🌐 **Networkwalks Hash Calculator** | Browser-based PDF hash extraction          |
| ⚡ **Networkwalks Password Cracker** | Browser-based dictionary attack            |
| 📄 **PDF Viewer**                   | Password verification and flag capture     |
| 🖥️ **VirtualBox Shared Folder**    | Windows ↔ Kali file transfer               |

---

# 🔴 W3-PM1 — Offline Password Recovery

## Workflow

```text
Encrypted PDF
     ↓
pdf2john.pl
     ↓
$pdf$ Hash
     ↓
rockyou.txt
     ↓
John the Ripper
     ↓
Recovered Password
     ↓
PDF Verification
     ↓
Embedded Flag
```

### Hash Extraction

Example workflow:

```bash
pdf2john.pl "My Locked PDF1.pdf" > hashes/pdf1.hash
```

Verify the generated hash:

```bash
cat hashes/pdf1.hash
```

The extracted value contains the crackable:

```text
$pdf$
```

format.

### Dictionary Attack

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hashes/pdf1.hash
```

Display the recovered credential:

```bash
john --show hashes/pdf1.hash
```

The same workflow was repeated for all three internship-provided PDFs.

---

# 🔵 W3-PM2 — Networkwalks Online Workflow

The second workflow used the Networkwalks browser-based tools.

### Workflow

```text
PDF
 ↓
Networkwalks Hash Calculator
 ↓
$pdf$ Hash
 ↓
Networkwalks Password Cracker
 ↓
Recovered Password
 ↓
PDF Verification
 ↓
Flag
```

### Tools

**Hash Calculator**

`https://networkwalks.com/hash-calculator/`

Used to extract the `$pdf$` hash from each lab PDF.

**Password Cracker**

`https://networkwalks.com/password-cracker/`

Used to perform the dictionary attack against the extracted hash.

The report records that the online Password Cracker used its built-in **100-word dictionary** for the lab exercise.

---

# 📊 Recovery Results

## W3-PM1 — JTR

| Target               | Recovered Password | Verification Flag                         |
| -------------------- | ------------------ | ----------------------------------------- |
| `My Locked PDF1.pdf` | `good-luck`        | `nw{cybersecurity_flag_captured_2608}`    |
| `My Locked PDF2.pdf` | `password1`        | `nw{networkwalks_persistence_jtr_270521}` |
| `My Locked PDF3.pdf` | `1qaz2wsx`         | `nw{networkwalks_flag_260821_1}`          |

The report records PDF1 as taking approximately **10 seconds**, while PDF2 and PDF3 were recovered instantly in the lab environment.

---

## W3-PM2 — Networkwalks

| Target               | Recovered Password | Dictionary Progress | Verification Flag                         |
| -------------------- | ------------------ | ------------------: | ----------------------------------------- |
| `My Locked PDF1.pdf` | `password1`        |            91 / 100 | `nw{networkwalks_flag1_jtr_270521_1}`     |
| `My Locked PDF2.pdf` | `password1`        |            91 / 100 | `nw{networkwalks_persistence_jtr_270521}` |
| `My Locked PDF3.pdf` | `1qaz2wsx`         |            35 / 100 | `nw{networkwalks_flag_260821_1}`          |

These results are documented in the internship report's online-cracking section.

---

# 🧪 Verification

Password recovery was not treated as complete until each recovered credential was tested against its corresponding PDF.

```text
Password Recovered
       ↓
Open PDF
       ↓
Enter Password
       ↓
PDF Successfully Unlocked
       ↓
Capture Embedded Flag
```

This provided a second layer of verification for the cracking results.

---

# 🛠️ Troubleshooting & Real Lab Challenges

One of the useful parts of this exercise was dealing with actual Kali/VirtualBox environment issues rather than only executing the cracking commands.

### 1. Missing PDF files

The PDFs were initially available on the Windows host but were not accessible inside Kali.

**Solution:** configured and mounted the VirtualBox shared folder.

```bash
sudo mount -t vboxsf week3 /mnt/week3
```

---

### 2. Empty shared-folder mount

The mounted directory initially contained no target files.

**Solution:** corrected the VirtualBox shared-folder configuration and enabled auto-mount/permanent settings.

---

### 3. Permission denied

The working directory was created with root ownership.

**Solution:**

```bash
sudo chown -R kali:kali ~/W3-PM1
```

This restored normal user access.

---

### 4. PDF Viewer Issue

`evince` was unavailable in the Kali environment.

**Solution:** used the available PDF viewer / XFCE file manager to verify the recovered passwords and capture the flags.

---

# 🔎 Security Findings

### 01 — Weak passwords are highly vulnerable

The lab demonstrated that dictionary-based passwords such as:

```text
good-luck
password1
1qaz2wsx
```

can be recovered quickly when the attacker has access to the encrypted PDF and the password is present in the attack dictionary.

### 02 — Password-protected PDFs expose crackable metadata

The exercise demonstrated that a `$pdf$` hash can be extracted from the protected document without first knowing the password.

This means the strength of the password remains a critical part of the security model.

### 03 — Third-party online tools introduce data-handling risk

The Networkwalks browser workflow was useful for the controlled lab, but uploading genuine confidential documents to unknown third-party services could expose sensitive information.

For real confidential data, an isolated offline workflow is preferable.

---

# 🔐 Security Recommendations

* Use long, random, non-dictionary passphrases.
* Avoid common passwords and keyboard patterns.
* Do not reuse lab/demo passwords for real documents.
* Combine document passwords with appropriate access controls and encryption.
* Avoid uploading confidential files to untrusted online cracking services.
* Prefer isolated offline tooling for authorised security assessments.
* Maintain evidence of hashes, commands, results and verification steps.
* Perform password-recovery testing only with explicit authorization.

The internship report recommends **12+ character random, non-dictionary passphrases** for confidential PDFs.

---

# 📁 Repository Structure

```text
NETWORKWALKS-B083F-WK3-PM1-PDF-PASSWORD-RECOVERY/
│
├── README.md
│
├── THANGAMANI_M_B083F_W3-PM-FINAL_Report.pdf
│
├── W3-PM1/
│   ├── hashes/
│   │   ├── pdf1.hash
│   │   ├── pdf2.hash
│   │   └── pdf3.hash
│   │
│   └── screenshots/
│       ├── jtr_setup/
│       ├── hash_extraction/
│       ├── password_recovery/
│       └── pdf_verification/
│
└── W3-PM2/
    └── screenshots/
        ├── hash_calculator/
        ├── password_cracker/
        └── verification/
```

> **Privacy note:** Do not upload the original password-protected PDFs, sensitive documents, or unnecessary recovered credentials to a public repository. Keep only the evidence and lab documentation required for the internship submission.

---

# 📸 Evidence

The complete internship report contains evidence covering:

* Kali Linux/JTR environment setup
* `pdf2john.pl` hash extraction
* `$pdf$` hash generation
* JTR password recovery
* PDF1/PDF2/PDF3 verification
* Networkwalks Hash Calculator
* Networkwalks Password Cracker
* Online verification and flag capture
* Troubleshooting and environment configuration

The final report contains **18 evidence screenshots** covering both modules.

---

# 🎓 Key Takeaways

This lab provided practical exposure to the complete password-recovery lifecycle:

```text
Identify
   ↓
Extract
   ↓
Hash
   ↓
Attack
   ↓
Recover
   ↓
Verify
   ↓
Document
   ↓
Assess Risk
```

The main lesson was that **document encryption does not compensate for weak password selection**. A password that is common, dictionary-based, or based on predictable keyboard patterns can significantly reduce the effective protection of an encrypted document.

The exercise also demonstrated the difference between a **local, offline security workflow** and a **browser-based lab workflow**, while reinforcing the importance of authorization, evidence handling and responsible security testing.

---

## 👤 Author

**Thangamani M**

Cybersecurity Intern
**Networkwalks — Batch B083F**

**Week 03 | PDF Password Recovery & Hash Cracking**

---

### ⚠️ Responsible Security Notice

This repository documents an **authorized educational cybersecurity lab** performed using internship-provided / self-owned files.

The techniques demonstrated here must only be used against systems, files and credentials for which you have explicit authorization.

**Learn → Test → Document → Defend 🔐**
