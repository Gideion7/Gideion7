# 🔒 Secure Active Directory Hardening with Nessus and GPOs
## 🌟 Project Overview

This project documents the process of securing and hardening a baseline Windows Server 2022 Domain Controller (DC) using industry-standard tools and practices. The core objective was to perform a vulnerability assessment using Nessus Essentials and then systematically mitigate identified risks by implementing strict Group Policy Objects (GPOs) and adhering to secure configuration best practices.



**Key Skills Demonstrated**
- Vulnerability Assessment: Deployed and configured Nessus Essentials on a separate Linux VM to perform authenticated and non-authenticated vulnerability scans against the Windows DC.
- Active Directory Management (GPOs): Implemented and enforced security policies via Group Policy Objects (GPOs), including a strong password policy.
- Account and Access Control: Applied the Principle of Least Privilege (PoLP) by creating a non-admin user with restricted logon hours and enforcing access limits for remote administration tools (RDP and PowerShell Remoting).
- System Hardening: Performed crucial baseline hardening steps, including applying Windows Updates and changing default administrative passwords.
- Conceptual Security Controls: Defined and provided examples of Administrative, Technical/Logical, and Physical security controls, and analyzed the security implications of tools like User Account Control (UAC) and the Windows Registry.


## 🏗️ Lab Environment
| Component               | Description                                           | Role in the Lab                                                                                         |
|-------------------------|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Domain Controller (DC) VM | Windows Server 2022 (Default: Administrator/Password#2!) | Target system for hardening and vulnerability scanning. Configured Active Directory Domain Services (AD DS). |
| Nessus Scanner VM       | Linux VM (e.g., Kali/Ubuntu)                          | Hosted Nessus Essentials for unauthenticated and authenticated scans against the Domain Controller.      |

## 🛠️ Implementation & Technical Hardening Steps

The Domain Controller was secured through the following sequence:

1. Initial Vulnerability Scan (Unauthenticated & Authenticated)
   - Deployment: Nessus Essentials was installed on the Linux VM.
   - Scan Policy: A "Basic Network Scan" was configured against the DC's IP address.
   - Authenticated Scan: The scan included credentials (Administrator/Password#2!) to ensure an in-depth, accurate vulnerability assessment, which is necessary to detect critical issues.
   - Initial Findings: The scan identified a large number of vulnerabilities (e.g., 57 vulnerabilities in the initial scan screenshot ), confirming the default insecure state of the Domain Controller.

2. Baseline Security Measures & Mitigation
   - Windows Updates: Performed crucial Windows Updates on the DC to patch known security vulnerabilities, addressing immediate, high-risk flaws.
   - Password Policy via GPO: A strong password policy was enforced using the Default Domain Policy Group Policy Object (GPO).
   - Minimum Password Length: Set to 14 characters.
   - Password History: Set to 24 passwords remembered.
   - Complexity Requirements: Set to Enabled
   - User Account Creation and PoLP:
      - Created a standard, non-administrative Active Directory user (Joe Smith).
      - Access Control: Restricted the user's logon hours to a specific window (Sunday through Friday from 6:00 AM to 8:00 PM) to limit the attack surface (access control)
