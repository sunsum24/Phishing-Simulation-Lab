# 🎣 Phishing Simulation & Credential Harvesting Lab

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Gophish](https://img.shields.io/badge/Gophish-FF5722?style=for-the-badge&logo=gophish&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE_ATT%26CK-T1566-red?style=for-the-badge)

## 📌 Executive Summary
Phishing remains the primary initial access vector for modern cyberattacks (MITRE ATT&CK **T1566**). This repository provides an isolated, containerized environment for Red Teamers and IT Administrators to simulate real-world phishing campaigns safely.

This lab utilizes **Gophish** (an industry-standard open-source phishing framework) and **MailHog** (a local SMTP testing server), orchestrated via **Docker**. It allows security professionals to craft emails, clone login portals, capture credentials, and analyze user behavior without the risk of sending emails to the real internet.

## 🏗️ Lab Architecture
*   **Attacker Environment (Docker):** Hosts the Gophish web server for campaign management and landing pages.
*   **SMTP Interceptor (MailHog):** Catches all outgoing phishing emails locally so they can be inspected safely.
*   **Target (Simulated):** The user interacting with the email and landing page.

## 🚀 Deployment & Setup

### Prerequisites
*   [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) installed on your host machine.

### Step 1: Clone and Start the Lab
```bash
git clone https://github.com/sunsum24/Phishing-Simulation-Lab.git
cd Phishing-Simulation-Lab
docker-compose up -d


---

### 🔴 Part 2: The Execution & Defense

---

## 🎯 Executing a Campaign (Walkthrough)

### 1. Configure Sending Profile
1. In Gophish, go to **Sending Profiles** -> **New Profile**.
2. Set the **Host** to `mailhog:1025`.
3. Set the **From** address to `it-support@company.local`.
4. Leave authentication blank (MailHog does not require auth for local testing).

### 2. Create an Email Template
1. Go to **Email Templates** -> **New Template**.
2. Craft a compelling email. Example:
   *   *Subject:* URGENT: Password Expiration Notice
   *   *Body:* "Your corporate password expires in 2 hours. Please click the link below to reset it immediately to avoid account lockout."
3. Add a tracking link `{{.URL}}` in the body.

### 3. Create a Landing Page
1. Go to **Landing Pages** -> **New Page**.
2. Check **"Capture Submitted Data"** and **"Capture Passwords"**.
3. Set the **Redirect to** URL as `https://www.cybersecurityawareness.com` (to simulate a training page after the "failure").

### 4. Launch the Campaign
1. Go to **Campaigns** -> **New Campaign**.
2. Select your Template, Landing Page, and Sending Profile.
3. Add a group of test emails (e.g., `victim@company.local`).
4. Launch! 
5. Open `http://localhost:8025` to view the email, click the link, and enter fake credentials on the landing page. Watch Gophish capture the data in real-time.

---

## 🛡️ Mitigation & Blue Team Defense

As a VAPT Practitioner, exploiting the vulnerability is only half the job; remediation is critical. To defend against this attack vector, organizations must implement:

1.  **Email Authentication:** Enforce **SPF, DKIM, and DMARC** policies to prevent domain spoofing.
2.  **Secure Web Gateways:** Deploy proxies that block newly registered domains and known malicious IPs.
3.  **Multi-Factor Authentication (MFA):** Even if credentials are harvested, MFA prevents immediate account takeover.
4.  **User Awareness Training:** Run recurring phishing simulations (like this lab) to train employees on spotting suspicious links and verifying sender addresses.

## ⚠️ Disclaimer
This environment is designed strictly for **educational purposes, authorized security awareness training, and approved Red Team engagements**. The author is not responsible for any misuse or damage caused by this program. Do not use these techniques against targets without explicit written permission.
