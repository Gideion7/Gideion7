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
   1. Windows Updates: Performed crucial Windows Updates on the DC to patch known security vulnerabilities, addressing immediate, high-risk flaws.
   2. Password Policy via GPO: A strong password policy was enforced using the Default Domain Policy Group Policy Object (GPO).
   3. Minimum Password Length: Set to 14 characters.
   4. Password History: Set to 24 passwords remembered.
   5. Complexity Requirements: Set to Enabled
   6. User Account Creation and PoLP:
      - Created a standard, non-administrative Active Directory user (Joe Smith).
      - Access Control: Restricted the user's logon hours to a specific window (Sunday through Friday from 6:00 AM to 8:00 PM) to limit the attack surface (access control)
   7. Administrative Password Change: The default insecure administrator password (Password#2!) was changed to a secure password.
   8. Restricting Remote Access (RDP/PowerShell):
       - RDP Restriction: Used a GPO (Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > User Rights Assignment > Allow log on through Remote Desktop Services) to limit RDP access exclusively to the Administrators group.
       - PowerShell Remoting Restriction: Used the Set-PSSessionConfiguration PowerShell cmdlet to restrict who can use PowerShell Remoting, enforcing a Technical/Logical Control.
3. Post-Hardening Vulnerability Scan
   - The "Basic Network Scan" was run a second time.
   - Result: The vulnerability count was significantly reduced (e.g., 57 vulnerabilities to 17 vulnerabilities ), validating the effectiveness of the applied security controls and hardening steps

## 🧠 Key Takeaways
Role-Based Access Control (RBAC) and Least Privilege (PoLP)
RBAC is a mechanism that restricts system access based on an individual's defined role. This directly enforces the Principle of Least Privilege (PoLP), which states that users must have the absolute minimum level of access necessary to complete their job functions. By limiting non-administrative users (like Joe Smith) to only what is needed, the risk of data compromise or accidental system configuration changes is minimized.


Importance of Regular Updates (Q5.1)
Regular updates are critical because they fix security holes (vulnerabilities) that hackers could exploit to gain unauthorized access. For new OS installs, this is especially important as the base installation often lacks the latest patches, making it immediately susceptible to known, easily exploitable weaknesses.

The Windows Registry and Security (Q4.c.i)
The Windows Registry is a central database storing settings for the operating system and programs. Security-related settings it controls include user permissions and startup programs. A hacker may compromise the HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run key to achieve persistence, ensuring their malicious program runs every time the computer starts, staying active even after a reboot

## Link to Complete Lab Document and Screenshots
> **note:** The doc also includes a try hack me challenge that I did before starting the lab

[Nessus and GPO](nessus.pdf)
