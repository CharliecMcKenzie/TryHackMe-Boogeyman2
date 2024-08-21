# TryHackMe Boogeyman 2 Challenge

## Introduction
After having a severe attack from the Boogeyman, Quick Logistics LLC improved its security defences. However, the Boogeyman returns with new and improved tactics, techniques and procedures. 
In this room, I will be tasked to analyse the new tactics, techniques, and procedures (TTPs) of the threat group named Boogeyman. 

### Artefacts
For the investigation, you will be provided with the following artefacts:
- Copy of the phishing email.
- Memory dump of the victim's workstation.

## Walk-through:

I started the investigation by analysing the email with PhishTool, which made the process easier. Through this analysis, I identified the sender of the phishing email, the victim employee's email address, the name of the attached malicious document, and its MD5 hash. 
<p align="center">
<img src="https://i.imgur.com/vJTwHRO.png" height="80%" width="80%" alt="Phishtool1"/>
<img src="https://i.imgur.com/7r07w2y.png" height="80%" width="80%" alt="Phishtool2"/>

Next, I used Olevba to extract the VBA macros from the attached document. This revealed the URL used to download the stage 2 payload, the process that executed it, and the file path of the payload. 
<p align="center">
<img src="https://i.imgur.com/k4743fs.png" height="80%" width="80%" alt="olevba"/>

To identify the PID of the malicious process and the parent process PID that executed the stage 2 payload, I used Volatility with the ‘windows.pslist’ plugin to extract artifacts from the memory dump.
<p align="center">
<img src="https://i.imgur.com/NP3h2T2.png" height="80%" width="80%" alt="pslist"/>

The URL used to download the malicious binary executed by the stage 2 payload was likely the same as the one found earlier, but with a ‘.exe’ extension. I confirmed this by searching for the ‘files.boogeymanisback.lol’ string within the memory dump. 
<p align="center">
<img src="https://i.imgur.com/poolIqx.png" height="80%" width="80%" alt="exe"/>

I used the ‘windows.netscan’ plugin to identify the IP address, port, and PID associated with the C2 connection initiated by the malicious binary. 
<p align="center">
<img src="https://i.imgur.com/7lgHlQP.png" height="80%" width="80%" alt="netscan"/>

By using the ‘windows.cmdline’ plugin, I found the full file path of the malicious process that established the C2 connection, as well as the path to the malicious email attachment. 
<p align="center">
<img src="https://i.imgur.com/FBC44qD.png" height="80%" width="80%" alt="cmdline"/>

The attacker implanted a scheduled task immediately after establishing the C2 callback. I discovered the command used to maintain persistent access by using the ‘strings’ command to search for ‘schtasks’. 
<p align="center">
<img src="https://i.imgur.com/0biH1LX.png" height="80%" width="80%" alt="schtask"/>

## Summary
The TryHackMe "Boogeyman 2" challenge is an excellent follow-up for those looking to deepen their cybersecurity skills. It builds on foundational concepts from the first challenge, adding complexity with more advanced techniques and tools. You are required to analyse memory dumps, investigate network traffic, and dissect malicious scripts, all within a realistic attack scenario. The challenge effectively reinforces key skills like persistence hunting, process analysis, and C2 detection, making it a well-rounded and immersive learning experience. "Boogeyman 2" was a lot of fun and is ideal for anyone wanting to enhance their incident response and threat analysis capabilities. 
