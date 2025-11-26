# 🛡️ Wazuh SIEM/EDR Deployment and Endpoint Security Lab

## 🌟 Project Overview

This project documents the hands-on deployment and configuration of Wazuh, an open-source security platform that unifies Security Information and Event Management (SIEM) and Extended Detection and Response (XDR) capabilities.

The lab simulates a small enterprise environment by deploying the Wazuh server to monitor and secure two distinct endpoint operating systems: Windows 10 and Kali Linux. This project demonstrates practical skills in centralized security monitoring, compliance enforcement, File Integrity Monitoring (FIM), and incident detection.

**Key Skills Demonstrated:**
- SIEM/EDR Deployment: Successfully deployed and configured the Wazuh server and dashboard using an OVA image.

- Agent Management: Installed, registered, and managed agents on heterogeneous endpoints (Windows 10 and Kali Linux VMs).

- File Integrity Monitoring (FIM): Customized FIM settings on the Windows endpoint to enable real-time monitoring for critical directories and successfully detected file modifications and additions.

- Security Configuration Assessment (SCA): Utilized Wazuh's SCA feature to perform security audits against industry benchmarks (e.g., CIS Microsoft Windows 10 Enterprise Benchmark).

- Threat Detection & Incident Response: Configured registry monitoring on Windows to detect simulated malware persistence attacks via the Run key and configured log ingestion on Kali Linux for SSH authentication events.

- Compliance Monitoring: Explored and documented compliance standards (PCI DSS, GDPR) and the role of SIEM tools in meeting regulatory requirements.


## 🏗️ Lab Architecture

The environment was built using Virtual Machines (VMs) to isolate the lab network and simulate a real-world infrastructure.

| Component            | Description                                   | Role in the Lab                                                                                   |
|----------------------|-----------------------------------------------|----------------------------------------------------------------------------------------------------|
| Wazuh Server VM      | Wazuh OVA (OpenSearch, Manager, Dashboard)   | Centralized log collection, analysis, alerting, and dashboard visualization.                      |
| Windows 10 VM        | Windows 10 Pro (Endpoint)                     | Monitored endpoint for FIM, SCA, Registry Monitoring, and simulated malware attack.                |
| Kali Linux VM        | Kali GNU/Linux (Endpoint)                     | Monitored endpoint for log analysis (SSH) and general system auditing.                             |

Network Configuration: All VMs were configured in NAT mode for networking to ensure proper communication between agents and the Wazuh server.

## 🛠️ Implementation & Technical Walkthrough

1. Agent Deployment and Centralized Monitoring
   - The Wazuh server was accessed via the web dashboard at https://<WazuhIpAddress>.
   - Agents were deployed on both endpoints (Windows 10 using MSI 32/64 bits and Kali Linux using DEB amd64).
   - Verified successful agent communication and status on the Agents Summary dashboard.
  
2. Custom File Integrity Monitoring (FIM)
   - Modified the Windows agent's configuration file (ossec.conf) at C:\Program Files (x86)\ossec-agent.
   - Added a custom directory for real-time monitoring to the <syscheck> configuration:
     
     `<directories realtime="yes" report_changes="yes" check_all="yes">C:\Users\Win10\Desktop</directories>`
   - Validated the configuration by creating and modifying a text file on the Windows Desktop, resulting in real-time FIM alerts on the Wazuh Dashboard.
  
3. Windows Registry Monitoring and Malware Simulation
   - Demonstrated custom Windows Registry monitoring by adding the key `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\customkey` to the `ossec.conf` file.
   - Accelerated the registry scan frequency from the default 12 hours (43200 seconds) to 30 seconds for testing purposes.
   - Simulated a malware persistence attack by adding an entry for iexplore.exe (Internet Explorer) to the default monitored Run key:
     - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run.`
    - Wazuh successfully captured the addition of the new registry value, demonstrating its ability to detect common persistence techniques.
  
4. Linux Log Collection for SSH Events
   - Configured the Kali Linux agent to transmit SSH logs for analysis by modifying its `ossec.conf` file:
  ```
  <localfile>
    <location>journald</location>
    <log_format>journald</log_format>
    <filter field="_SYSTEMD_UNIT">^ssh.service\$</filter>
</localfile>
```
   -  Validated the setup by performing an SSH login and logout to the Kali VM, which generated real-time alerts in the Wazuh Threat Hunting dashboard, capturing ssh authentication success and PAM: Login session opened/closed events.

## 🧠 Key Takeaways & Cybersecurity Value
This lab provided critical experience in the following areas, which are highly valued in a Security Operations Center (SOC) environment:

EDR Solution Benefit
An EDR solution's primary benefit is providing continuous, real-time monitoring of endpoint activity (such as file changes, process execution, and registry modifications). This allows for immediate threat detection and automated response actions, drastically reducing the dwell time of an attacker and minimizing the risk of a security incident spreading across the network.

Real-World EDR Scenario
An EDR solution is essential for detecting a Ransomware Attack. If an attacker initiates the encryption of files on an endpoint, the EDR system detects the unusual, high-volume file modifications (a strong anomaly). It can then automatically isolate the affected device from the network and alert a security analyst, preventing the ransomware from propagating to file shares or other systems.

Importance of SIEM/EDR Integration
The combination of EDR and SIEM (as demonstrated by Wazuh) is crucial because EDR provides granular, endpoint-specific insights, while the SIEM aggregates this data with logs from other sources (like firewalls and IDS) to provide a comprehensive and contextualized view of the organization's entire security posture. This is the foundation of modern threat hunting and compliance reporting.

## 🔗 Link to Complete Lab Document and Screenshots
  [Wazuh_Lab](wazuh_lab.pdf)
