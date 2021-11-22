# Researching

## Lateral Movement  
Somewhat taken from [Splunk Lateral Movement](https://www.splunk.com/en_us/blog/security/spotting-the-signs-of-lateral-movement.html)  
>When looking for lateral movement, we're identifying processes connecting remotely into a host. Our initial search could use Windows security logs, looking for authentication events over the network from rare or unusual hosts or users.  

Example query  

>index=wineventlog sourcetype=WinEventLog:Security (EventCode=4624 OR EventCode=4672) Logon_Type=3 NOT user="*$" NOT user="ANONYMOUS LOGON" 
| stats  count  BY dest src_ip dest_nt_domain user EventCode 
| sort count

Processes - How to move laterally?
- Remotely creating WMI processes
- Scheduling tasks
- Creating services (service creation event ID 7036)
  - Attackers often create a new service to host their malicious code, or they may take a non-critical service or one that is disabled and modify it to point to their malware, enabling the service if necessary.  It is unusual for a service to be created or modified using the sc.exe utility, so you want to look for instances of this occurring so you can investigate further.  
    - For reference: Microsoft's command-line "Service Configuration Tool" program, named "sc.exe", is in "C:\Windows\System32". It allows administrative users to establish a program as a Windows service in the Service Control Manager (SCM) database and the Registry, either locally or remotely
- Newly create service principals (service accounts)
- Detecting beaconing activity (need to rely on DNS data though)
- Pass the hash (PtH)
- Pass the ticket (PtT)
- Exploitation of remote services
- Internal spear phishing
- SSH hijacking
- Windows admin shares
