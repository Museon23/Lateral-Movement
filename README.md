# Research

## Lateral Movement  
Some credit due to [Splunk Lateral Movement](https://www.splunk.com/en_us/blog/security/spotting-the-signs-of-lateral-movement.html), [Logrhythm](https://logrhythm.com/blog/what-is-lateral-movement-and-how-to-detect-it/), and [Illusive Networks](https://go.illusivenetworks.com/hubfs/Whitepaper_Breaking%20Banks_Ransomware%20Big%20Game%20Hunting%20in%20FS_Final.pdf?hsCtaTracking=7236503b-115f-4318-a75f-8017a2816fb2%7C23d25c7c-d6c8-42ee-b2fb-1025a9e248ee)
>When looking for lateral movement, we're identifying processes connecting remotely into a host. Our initial search could use Windows security logs, looking for authentication events over the network from rare or unusual hosts or users.  

`Example query`

>index=wineventlog sourcetype=WinEventLog:Security (EventCode=4624 OR EventCode=4672) Logon_Type=3 NOT user="*$" NOT user="ANONYMOUS LOGON" 
| stats  count  BY dest src_ip dest_nt_domain user EventCode 
| sort count

`Ways to move laterally`
- Reconnaissance - using tools such as OpenVAS, Nmap, Shodan to find holes/vulnerabilities and exploiting them  
  - perform similar scanning in the enviornment to highlight, or remediate, or improve segmentation
- Living off the Land - The concept of a syndicate using already-available tools built into the operating systems in order to achieve their goals rather than downloading and using malicious tools that might otherwise be detected and denied.
- Remotely creating WMI processes
- Scheduling tasks
- Creating services (service creation event ID 7036)
  - Attackers often create a new service to host their malicious code, or they may take a non-critical service or one that is disabled and modify it to point to their malware, enabling the service if necessary.  It is unusual for a service to be created or modified using the sc.exe utility, so you want to look for instances of this occurring so you can investigate further.  
    - For reference: Microsoft's command-line "Service Configuration Tool" program, named "sc.exe", is in "C:\Windows\System32". It allows administrative users to establish a program as a Windows service in the Service Control Manager (SCM) database and the Registry, either locally or remotely
- Newly create service principals (service accounts)
- Detecting beaconing activity (need to rely on DNS data though)
- Pass the hash (PtH)
- Pass the ticket (PtT)
- Exploitation of remote services, like RDP
- Internal spear phishing to potentiallialy steal credentials allowing for escalation 
- SSH hijacking
- Windows admin shares  

`Examples of tools used`
- Masscan - used for port scanning
- NLBrute (AKA nl.exe or nlbrute.exe) - this brute forces Windows credentials over RDP
- RDP Brute (z668) - Similar to the above being a high-performance brute force utility
- Mimikatz - allows for dumping credential and ultimately escalating privileges
- Everything.exe - tool for searching Windows domains for specific files or keyword searches.
- LaZagne - free and opensource tool to dump passwords for applications on a host, such as passwords for websites within web browsers
- Qwinsta - short for "query session" which can query sessions on terminal servers or remote desktop
- ProcDump - used to dump credentials from LSASS
- PSExec - allows remote execution of binaries on remote systems
- PowerShell Empire - a post exploit agent designed to use and replace PowerShell without having to use PowerShell.exe
- csvde.exe - a built-in Windows CLI capable of importing and exporting data into a CSV.  Use to dump user accounts, endpoints, and applications in the AD forest with minimal noise compared to other AD cracking tools.
- Colbalt Strike - A commercial tool, though cracked versions are available.  A fileless, multi-stage agent it calls beacons that don’t install or touch the victim host’s file system and hard disk allowing it to go undetected by most antimalware solutions. The beacons stack multiple utilities in a single agent, including Mimikatz, key logging, SOCKS proxy, persistent backdoor access, privilege escalation, and more.
