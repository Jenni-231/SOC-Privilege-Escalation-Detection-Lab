# SOC-Privilege-Escalation- Detection-Lab/
Detection engineering lab simulating Active Directory priviledge escalation techniques to analyze Windows tlemetary and build Splunk-based monitoring use cases



🔹 Overview

This module demonstrates privilege escalation through misconfigured Windows services using PowerUp inside an InvisiShell session.

The attack leverages weak service permissions that allow:
	•	Service configuration modification
	•	Service restart rights
	•	Abuse functions provided by PowerUp

The goal is to reconfigure a vulnerable service and escalate privileges to a higher-privileged account.

⸻

🔹 Attack Preconditions
	•	PowerShell execution possible (via InvisiShell)
	•	PowerUp module available
	•	A service where:
	•	The current user has SERVICE_CHANGE_CONFIG
	•	The current user has SERVICE_START or restart rights

⸻

🔹 Attack Flow
	1.	Launch InvisiShell.
	2.	Execute PowerUp and run Invoke-AllChecks.
	3.	Identify services flagged as abusable.
	4.	Locate a service where restart rights are available.
	5.	Use the abuse function with the -Username parameter to apply changes to a specified account.
	6.	Restart the service.
	7.	Confirm privilege escalation.

        


           ________________________________________________________________________________________________________

           ||Event ID                | Description               |  Why It Matters
           ________________________________________________________________________________________________________

          | 4697                    | Service installation        |Indicates service creation
           ________________________________________________________________________________________________________
          | 7045                    | Service installed           |Logged in System log
           ________________________________________________________________________________________________________
          | 4670                    | Permissions changed         |Service ACL modification
           ________________________________________________________________________________________________________
          | 4663                    | Object access               |Service file modification
           ________________________________________________________________________________________________________
          | 7036                    | Service state change        |Service restart
           ________________________________________________________________________________________________________
          | 4688                    | Process creation            | Elevated shell spawn
           ________________________________________________________________________________________________________
          | 4672                    | Special privileges assigned |SYSTEM-level token
          _________________________________________________________________________________________________________





             💡 Note: In some lab environments, 4663 or 4670 may not appear if advanced auditing is not enabled.

⸻

🔹 Detection Strategy
	•	Monitor abnormal service reconfiguration events.
	•	Alert on service restarts initiated by non-admin users.
	•	Correlate 4688 events spawned by services.
	•	Enable Object Access auditing to capture 4663 events.

⸻

🔹 Mitigation
	•	Restrict service ACLs to administrators only.
	•	Remove unnecessary service restart rights.
	•	Enforce least privilege on service accounts.
	•	Enable advanced auditing for service object access.
	•	Monitor unexpected service configuration changes.