# Pyramid of Pain

Disclaimer:

### Details of the alert
SHA256 file hash: 54e6ea47eb04634d3e87fd7787e2136ccfbcc80ade34f246a12cf93bab527f6b

A timeline of the events leading up to this alert:
- 1:11 p.m.: An employee receives an email containing a file attachment.
- 1:13 p.m.: The employee successfully downloads and opens the file.
- 1:15 p.m.: Multiple unauthorized executable files are created on the employee's computer.
- 1:20 p.m.: An intrusion detection system detects the executable files and sends out an alert to the SOC.

💡 For the purpose of this activity, I'll focus on evaluating VirusTotal results. 
However, no single tool can detect all types of malicious activity. 
Security analysts will often use a combination of other tools to carefully evaluate the results of a scan before making a decision about the file.

### VirusTotal report tabs
1) Detection: This tab provides a list of third-party security vendors and their detection verdicts on an artifact.
   Detection verdicts include: malicious, suspicious, unsafe, and others. Notice how many security vendors have reported this hash as malicious and how many have not.

<img src="https://github.com/melaniedaniel7/Investigate-a-suspicious-file-hash/blob/13804362ba7a24e7f196ff644cb56a190e14cb5b/Screenshot%202024-11-11%20at%2018.48.57.png" width="800" />

2) Details: This tab provides additional information extracted from a static analysis of the IoC.
   Notice the additional hashes associated with this malware like MD5, SHA-1, and more. 

<img src="https://github.com/melaniedaniel7/Investigate-a-suspicious-file-hash/blob/3926074f2db15bfd8e68b79bcdf7d8e25db0b86c/Screenshot%202024-11-11%20at%2018.49.55.png" width="800" />

3) Relations: This tab contains information about the network connections this malware has made with URLs, domain names, and IP addresses.
   The Detections column indicates how many vendors have flagged the URL or IP address as malicious.

<img src="https://github.com/melaniedaniel7/Investigate-a-suspicious-file-hash/blob/138665873c37fa6fa9d71570431f60e38ae0320b/Screenshot%202024-11-11%20at%2018.53.05.png" width="800" />

4) Behavior: This tab contains information related to the observed activity and behaviors of an artifact after executing it in a controlled environment,
   such as a sandboxed environment. A sandboxed environment is an isolated environment that allows a file to be executed and observed by analysts and researchers.
   Information about the malware's behavioral patterns is provided through sandbox reports.
   Sandbox reports include information about the specific actions the file takes when it's executed in a sandboxed environment, such as registry and file system actions,
   processes, and more. Notice the different types of tactics and techniques used by this malware and the files it created.

<img src="https://github.com/melaniedaniel7/Investigate-a-suspicious-file-hash/blob/abf4aa17a3d40992f51b1a0806a9f5f92016cc97/Screenshot%202024-11-11%20at%2019.25.22.png" width="800" />

💡 Sandbox reports are useful in understanding the behavior of a file, but they might contain information that is not relevant to the analysis of the file. 
By default, VirusTotal shows all sandbox reports in the Behavior tab. One can select individual sandbox reports to view. 
This is helpful because one can view the similarities and differences between reports so that it's easier to identify which behaviors are likely to be associated with the file.

### Has this file been identified as malicious? Explanation of why or why not.
Yes this file has been identified as malicious because: 
1) Tt has a high vendors ratio, 58/71 security vendors flagged this file as malicious.
2) It has a negative community score of -217.
3) In the security vendors' analysis section malware was detected. The threat category for this malware was a trojan.

💡 The Vendors' ratio is based on security vendors' detections and vendors might not always detect malicious files. 
The Community Score is based on the opinions and insights from the VirusTotal community. 
If a file's scores are low, it doesn't necessarily mean that the file is safe. 
It is recommended to use multiple sources of information when evaluating files.

### Pyramid of Pain

<img src="" width="800" />

In this activity I was only required to include three IoC examples. Therefore, the IoC examples that have an "NB" infront of them are from the memo and the memo explanation is included below for my own learning purposes.

IoC examples from the memo:
- IP address: 207.148.109.242 is listed as one of many IP addresses under the Relations tab in the VirusTotal report. This IP address is also associated with the org.misecure.com domain as listed in the DNS Resolutions section under the Behavior tab from the Zenbox sandbox report.
- Network/host artifacts: Network-related artifacts that have been observed in this malware are HTTP requests made to the org.misecure.com domain. This is listed in the Network Communications section under the Behavior tab from the Venus Eye Sandbox and Rising MOVES sandbox reports.
- Tools: Input capture is listed in the Collection section under the Behavior tab from the Zenbox sandbox report. Malicious actors use input capture to steal user input such as passwords, credit card numbers, and other sensitive information.

💡 VirusTotal reports can contain legitimate domains and IP addresses that are not considered malicious. 
