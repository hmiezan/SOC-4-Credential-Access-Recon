# SOC-4--Credential Access Reconnaissance

Following successful authentication, SMB discovery, and PowerShell reconnaissance, an attacker established a remote WinRM session and performed reconnaissance against authentication-related Windows processes.
The objective of this investigation was to identify indicators of credential-access activity and determine whether credential theft occurred.

### Lab Environment
- Kali Linux 10.161.170.22
- Windows 11 10.161.200.x 
- Security Onion SIEM
- Elastic Agent Installed

### Incident Summary
Investigation identified a remote WinRM session established using valid credentials. During the session, authentication-related Windows processes, including LSASS, were enumerated using process discovery commands.
The observed activity is consistent with credential-access reconnaissance and process discovery behavior. No evidence of credential dumping or credential extraction was identified.


### Attack Timeline

- 2026-11-21-- 22-32.57 (UTC-6) -- Successful renote connection established
- 2026-11-21-- 22-32.57 (UTC-6) -- Windows Remote Management provider launched (Wsmprovhost.exe)
- 2026-11-21-- 22-33.12 (UTC-6) -- user Discovery performed(Whoami.exe /priv)
- 2026-11-21-- 22-33.42 (UTC-6) -- LSASS process enumeration identified (findstr.exe)

### Investigation findings

- Source IP : 10.161.170.22
- Target Accounts victim.user
- Host Targeted : WLAB01
- Logon Type : 3-Network Logon
- Evil WinRM : C:\users\victim.user\Documents

Observations

- Successful Remote WinRM session established
- Successful enumarate LSASS for preparation
- Process discovery activity identified
- Indicator of compromise (IOC) ( victim.user)

### Evidence

#### Attack from kali (Evil-WinRM-Success access)
- <img width="963" height="806" alt="Kali EvilRM command" src="https://github.com/user-attachments/assets/b833e444-a37e-4a96-a0fc-e5284b0f0a39" />

#### Confirmation WinRM
- <img width="1903" height="883" alt="Parent Process" src="https://github.com/user-attachments/assets/20d8338c-5a19-4967-8b2e-cfb4673668f7" />

#### Successfully access- Process-parent- Process Discovery
- <img width="1912" height="885" alt="Remote Powershell" src="https://github.com/user-attachments/assets/adf8f0c7-f76b-44ad-af21-e7bca029f2fa" />

- <img width="1880" height="849" alt="Evidence Process-parent After WinRMM" src="https://github.com/user-attachments/assets/0d50952b-d2cb-441b-b95f-52cd0540fb29" />



#### MITRE ATT&CK Mapping
- Tactic (Initial Access)- Valid Accounts :	T1078 
- Tactic (Lateral Movement) - Technique (WinRM): T1021.006
- Tactic (Execution)- Technique (Command and Scripting Interpreter: PowerShell): T1059.001
- Tactic (Discovery)- Technique (Process Discovery): T1057
- Tactic (Discovery)- Technique (System Owner/User Discovery): T1033
- Tactic (Credential Access)- OS Credential Dumping (Reconnaissance Stage): T1003


#### Impact Assessment

Remote access was successfully established using valid credentials. Enumeration of authentication-related processes was observed, including LSASS reconnaissance.
This should be level as High 

#### Recommendations
- Enable Microsoft Defender Credential Guard to better protect authentication-related secrets.
- Enable LSASS Protected Process (RunAsPPL) to increase protection of authentication services.
- Monitor WinRM activity and investigate unexpected executions of wsmprovhost.exe.
- Enable PowerShell Script Block Logging and Module Logging to improve visibility into remote command execution.
- Restrict WinRM access to approved administrative systems and users.
- Implement Multi-Factor Authentication (MFA) to reduce the impact of compromised credentials.
- Review privileged group memberships and remove unnecessary administrative permissions.

#### Lessons Learned

This investigation demonstrated that emote PowerShell activity can be identified through WinRM-specific processes such as wsmprovhost.exe and parent-child process relationships provide valuable context when investigating remote command execution and enumeration of LSASS and other authentication-related processes may indicate preparation for credential theft.Not all credential-access investigations result in credential theft; distinguishing reconnaissance from actual credential extraction is essential for accurate incident classification.

#### Incident Report
<a href="https://1drv.ms/w/c/b5bf0dbc64611adf/IQBk2bTttjH5RrJrtzFHAtmpAWU5O3I2Uo9Fkk4fK-pqyVA?e=mEQvYs">See report </a>">See report </a>
