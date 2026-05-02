# SOC-Privilege-Escalation- Detection-Lab/
Detection engineering lab simulating Active Directory priviledge escalation techniques to analyze Windows tlemetary and build Splunk-based monitoring use cases


This module demonstrates relaying NTLM authentication to modify Group Policy Objects using GPOddity.

🔹 Requirement
	•	Ubuntu installed (attacker machine)
	•	NTLMrelayx.py available

⸻

🔹 Attack Flow
	1.	Start ntlmrelayx.py to listen for incoming NTLM authentication.
	2.	On the target system, create and execute a PowerShell shortcut to trigger authentication back to the attacker.
	3.	Upon receiving connection:
	•	In another terminal run:

     nc 127.0.0.1 11000

4.	Use relayed credentials to execute GPOddity.
	5.	Modify GPO to point audit file path to attacker-controlled file.
	6.	Before or immediately after execution:
	•	Create file with specified name.
	•	Create file share.
	•	Enable access to Everyone.
	7.	Copy GPOddity payload content into created file.
	8.	Wait for Group Policy refresh.
	9.	Test access using WinRM from a system within the affected group.
	10.	Confirm elevated domain-level control.
         


        🔹 Key Windows Event Logs




💡 If 5136 is missing, Directory Service auditing may not be enabled.

⸻

🔹 Detection Strategy
	•	Monitor 5136 for GPO modifications.
	•	Alert on GPO changes from unexpected systems.
	•	Monitor NTLM authentication to unusual hosts.
	•	Track share creation events.
	•	Correlate service account logons with directory modifications.

⸻

🔹 Mitigation
	•	Disable NTLM where possible.
	•	Enforce SMB signing.
	•	Enable LDAP signing and channel binding.
	•	Restrict GPO editing permissions.
	•	Monitor GPO changes in real time.
	•	Enable advanced AD auditing.
	•	Restrict anonymous share access.
	•	Enforce file share access control.


      