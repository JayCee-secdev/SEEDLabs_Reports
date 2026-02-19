# 🛡️ SEED Labs: Cybersecurity Methodology & Insights

Welcome! This repository is a collection of **technical insights, troubleshooting tips, and methodology** developed while working through the [SEED Labs](https://seedsecuritylabs.org/) project. 

Instead of just providing solutions, this repo focuses on the **underlying security principles** and the "Aha!" moments encountered during the exploitation and defense phases.

---

## 🧭 Repository Structure

Each laboratory has its own dedicated documentation file containing specific environment setups, challenges, and key takeaways.

* **/docs/software-security/**: Buffer Overflow, Shellshock, Set-UID.
* **/docs/network-security/**: TCP/IP Attacks, DNS, Firewalls.
* **/docs/crypto/**: RSA, PKI, Hash Collisions.

---

## 🚀 The "Golden Rules" for SEED Labs
Before starting any lab, I found these three habits saved me hours of debugging:

1. **Snapshots are Life:** Always take a VM snapshot before running a kernel exploit (like Dirty COW).
2. **Environment Isolation:** Use the provided Docker containers to ensure network configurations don't bleed into your host machine.
3. **Hex is Truth:** When a payload doesn't work, don't guess—use `xxd` or `hexdump` to see what is actually happening in memory.

---

## ⚠️ Academic Integrity
To respect the spirit of the SEED Project and university policies, this repository **does not** contain:
* Completed source code for the lab tasks.
* Direct answers to "Questions" found in the lab manuals.
* Ready-to-run exploit scripts.

*The goal here is to document the **learning process**, not to bypass it.*
