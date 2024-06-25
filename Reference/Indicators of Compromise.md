## Overview
Indicators of compromise are patterns of anomalous activity that indicate a potential breach or other security incident. In general, IoCs are fairly easy to spot. Broadly speaking, you should look for communications between devices that don't normally communicate, resource usage spiking and staying higher than usual, and files being moved around abnormally. The #1 IoC to look for in every case is beaconing because this is how commands are passed to compromised machines and one way sensitive data can leave your network.
## Core
### Network Related: 
- Bandwidth Consumption – Look for large data transfers and high packet counts 
- Beaconing – Look for long connections and high packet counts  
- Irregular P2P Communication – Look for connections between workstations that aren't expected local traffic 
- Rogue Devices – Look for unauthenticated devices or devices that don't match known hostnames 
- Scan/Sweep - IDS systems will usually pick up scanning; however, you should look for devices that connect to a large number of other devices 
- Traffic Spikes – Look for unusual spikes in traffic particularly at odd times 
- Non-Standard Port – Look for common traffic using non-standard ports
### Host Related: 
- Resource Consumption – Use Task Manager to quickly identify processes using high amounts of resources 
	-   CPU - Malware often causes race conditions that lead to high CPU utilization  
	-   RAM - Malware is prone to memory leaks and high memory usage   
	-   Disk - High disk usage is common with malware, but sadly is also common with anti-malware solutions  
- Unauthorized Software – Software should be whitelisted to avoid unauthorized software from running undetected 
- Malicious Process – Malicious processes can be spotted using a number of techniques most often leveraging memory forensics 
	-   Service without path to executable on disk 
	-   Process with PPID that shouldn't be possible 
	-   Process without a PPID  
	-   Offline process with associated network connections   
	-   Processes with incorrect file locations  
	-   Processes which specify command line options 
	-   Processes which call .dlls that they should not be accessing 
- Unauthorized Change – Look for processes that are changing registry keys or system settings that it shouldn't 
- Unauthorized Privilege – Look for processes and service accounts with more access than they should have 
- Data Exfiltration – Look for hosts with low storage being used as staging areas for data exfiltration 
- Abnormal OS Process Behavior – Use anomaly detection to look for system processes behaving strangely 
- File System Change – Look for odd folders/files particularly with randomly generated names 
- Registry Change – Compare against known-good baselines and look at reg keys known to be used by malware 
- Scheduled Tasks – Periodically audit scheduled tasks to make sure they aren't being used for persistence 
- Subscriptions – Check for odd WMI subscriptions
### Application Related: 
- Anomalous Activity – Look for applications sending unusual traffic or accessing resources it shouldn't 
- Account Activity – Look for applications using system permissions to create or modify accounts 
- Unexpected Output – Look for applications that are generating strange logs or user feedback 
- Unexpected Communication – Look for applications sending traffic that isn't expected 
- Service Interruption – Look for applications with unexpected down time  
- Application Log – Check application logs for IoCs
## References
