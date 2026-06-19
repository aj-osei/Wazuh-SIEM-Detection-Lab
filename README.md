# Wazuh SIEM Detection Lab

## Objective

The goal of this lab was to build a hands-on Wazuh SIEM environment and practice basic SOC-style detection. I connected both Windows and Linux endpoints to Wazuh, generated failed authentication activity, and reviewed the alerts in the Wazuh dashboard.

This lab helped me practice log collection, endpoint monitoring, Windows Event ID analysis, Linux SSH log analysis, and alert investigation.

### Skills Learned

- SIEM deployment and basic configuration
- Windows endpoint monitoring
- Linux endpoint monitoring
- Windows Event ID 4625 analysis
- SSH authentication failure detection
- Log ingestion and alert review
- Basic SOC-style investigation workflow
- Using Wazuh Threat Hunting to search for security events

### Tools Used

- Wazuh SIEM
- Wazuh OVA Server
- VirtualBox
- Windows 11 Endpoint
- Ubuntu Server Endpoint
- PowerShell
- SSH
- Windows Event Logs
- Linux Authentication Logs

## Lab Environment

| System | Purpose | Details |
|------|---------|---------|
| Wazuh Server | SIEM server | Hosted in VirtualBox |
| Windows Endpoint | Windows log source | Windows 11 system connected as a Wazuh agent |
| Linux Endpoint | Linux log source | Ubuntu Server connected as a Wazuh agent |

## Steps

### 1. Accessed the Wazuh OVA Server

I installed and launched the Wazuh OVA server in VirtualBox. After logging in, I used the terminal to get admin/root access and identify the Wazuh server IP address.

```bash
sudo bash
ip addr
```

The Wazuh server IP was then used to access the Wazuh dashboard from a browser.

### 2. Logged Into the Wazuh Dashboard

I accessed the Wazuh dashboard from my browser using the Wazuh server IP address.

```text
https://<WAZUH-SERVER-IP>
```

From there, I confirmed the dashboard was working and prepared to add endpoints as Wazuh agents.

### 3. Added the Windows Endpoint

I added my Windows 11 system as a Wazuh agent.

Steps completed:

1. Opened the Wazuh dashboard.
2. Went to **Agents Management**.
3. Selected **Deploy new agent**.
4. Chose Windows as the operating system.
5. Entered the Wazuh server IP address.
6. Copied the generated agent installation command.
7. Opened PowerShell as Administrator.
8. Ran the Wazuh agent installation command.
9. Started the Wazuh agent service.
10. Confirmed the Windows endpoint appeared as active in Wazuh.

### 4. Detected Windows Failed Login Attempts

To test Windows authentication monitoring, I generated failed login attempts on the Windows endpoint.

Steps completed:

1. Locked the Windows system.
2. Entered the wrong password multiple times.
3. Logged back in normally.
4. Opened the Wazuh dashboard.
5. Went to **Threat Hunting**.
6. Searched for:

```text
4625
```

Windows Event ID **4625** represents a failed logon attempt.

Wazuh generated alerts for failed login activity, showing that Windows authentication logs were being collected and analyzed correctly.

Example alert details:

```text
Rule Description: Logon Failure - Unknown user or bad password
Rule ID: 60122
Rule Level: 5
```

### 5. Added the Linux Endpoint

I added an Ubuntu Server system as a Wazuh agent.

Steps completed:

1. Installed Ubuntu Server.
2. Confirmed network connectivity.
3. Confirmed the Linux system could reach the Wazuh server.
4. Opened **Agents Management** in Wazuh.
5. Selected **Deploy new agent**.
6. Chose Linux and selected the correct Ubuntu package.
7. Entered the Wazuh server IP address.
8. Used the generated command to install the Wazuh agent.
9. Started and enabled the Wazuh agent service.
10. Confirmed the Linux endpoint appeared as active in Wazuh.

### 6. Detected Linux SSH Authentication Failures

To test Linux authentication monitoring, I generated failed SSH login attempts.

```bash
ssh wronguser@localhost
```

This attempted to SSH into the Ubuntu machine using a non-existent user. After entering the wrong password multiple times, Wazuh generated alerts in Threat Hunting.

Example alert details:

```text
Rule Description: sshd: Attempt to login using a non-existent user
Rule ID: 5710
Rule Level: 5
```

Wazuh also generated a higher-level alert for repeated missed passwords:

```text
Rule Description: syslog: User missed the password more than one time
Rule ID: 2502
Rule Level: 10
```

### 7. Lab Summary

In this lab, I successfully built a Wazuh SIEM environment and connected both Windows and Linux endpoints. I generated failed login activity on both systems and confirmed that Wazuh was ingesting logs, creating alerts, and allowing me to investigate events through Threat Hunting.

This project helped me build practical experience with SIEM monitoring, endpoint visibility, authentication failure detection, and SOC-style alert review.

## Screenshots


Example:

*Ref 1: Wazuh dashboard showing the Windows endpoint connected as an active agent.*
<img width="512" height="246" alt="1st" src="https://github.com/user-attachments/assets/abc359df-23dd-4eea-8e99-3e78b77f57d9" />

Ref 2: Wazuh Threat Hunting displaying Windows Event ID 4625 failed logon attempts generated during testing.
<img width="512" height="232" alt="2nd" src="https://github.com/user-attachments/assets/9e9f5d7c-ca44-41d5-b9fe-5fa77f149fa4" />


