[Architecture Diagram](architecture-diagram.png)

# AWS-Fortress-Architecture
AWS CloudFortress: A Zero-Trust IaC "Bank Vault" architecture. Features WAF protection against OWASP Top 10 (SQLi/XSS), private subnet isolation for EC2, and Client VPN for encrypted management. Replaces risky Bastion Hosts with automated, multi-layered defense to secure sensitive FinTech or Healthcare data.
Got it. Here is the clean, professional, and copy-paste-ready `README.md` content. I’ve balanced the technical "buzzwords" that recruiters look for with the "Super Fortress" story we built.

---
#  AWS CloudFortress: Secure Zero-Trust Infrastructure

**AWS CloudFortress** is a production-grade Infrastructure-as-Code (IaC) project. It deploys a "Digital Bank Vault" architecture designed to host sensitive applications (FinTech/Healthcare) by isolating them from the public internet while maintaining a robust, multi-layered defense.

##  Key Security Implementations

### 1. Application-Layer Defense (WAF)
The infrastructure includes an **AWS WAF (Web Application Firewall)** integrated with the Application Load Balancer. This is configured to **mitigate OWASP Top 10 threats**, providing deep packet inspection to block:
*   **SQL Injection (SQLi):** Prevents malicious database queries from reaching your backend.
*   **Cross-Site Scripting (XSS):** Filters out scripts intended to hijack user sessions.
*   **Layer 7 Rate Limiting:** Protects against automated bots and brute-force attempts.

### 2. Network Isolation (The "Invisible Server")
The EC2 application server is housed in a **Private Subnet** with:
*   **No Public IP:** The server is entirely invisible to external port scanners.
*   **NAT Gateway:** Allows the server to initiate outbound traffic (for security patches) while blocking all unsolicited inbound requests from the open web.

### 3. Secure Management (Client VPN)
This project **eliminates the need for Bastion Hosts**. System administration is handled via:
*   **AWS Client VPN:** An encrypted tunnel providing direct access to the private network.
*   **Mutual Authentication:** Access is granted only via cryptographic client certificates, ensuring only authorized admins can enter.

---

##  Tech Stack
*   **AWS CloudFormation:** Automated, repeatable infrastructure deployment.
*   **Networking:** VPC, Public/Private Subnets, NAT Gateway, Internet Gateway.
*   **Security:** AWS WAFv2, ACM (Certificate Manager), Security Groups.
*   **Compute:** Amazon EC2 (Amazon Linux 2023).

---

## How to Deploy

### Prerequisites
*   An AWS Account.
*   VPN Certificates generated via OpenVPN Easy-RSA and imported into **AWS Certificate Manager (ACM)**.

### Steps
1.  **Fork** this repository.
2.  Navigate to the **CloudFormation Console** in your AWS account.
3.  Select **Create Stack** > **With new resources** and upload `fortress-template.yaml`.
4.  **Parameters:** When prompted, paste your **Client** and **Server** ACM ARNs.
5.  Wait for the `CREATE_COMPLETE` status (approx. 10–15 mins).

---

##  Project Logic: The Armored Bank
To explain this in layterms:
*   **The WAF** is the security guard at the front door checking IDs and scanning bags.
*   **The Private Subnet** is the vault in the back with no windows or doors to the street.
*   **The VPN** is the secret underground tunnel used only by the bank manager to get inside safely.

---
