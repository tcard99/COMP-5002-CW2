# COMP-5002-CW2

## Introduction
A security Operations Centre (SOC) is the central area in an organisation which is responsible for monitoring, detecting and responding to cyber threats. The main purpose of is to maintain continuous visibility over security events, identify malicious activity and coordinate an effective response to protect the organisations infrastructure [1]. This report will show my investigation into the Boss of the SOCv3(BOTSv3) dataset for a fictitious brewing company called Frothly. This dataset is a Splunk based capture the flag(CTF). Its aim is to simulate a realistic cyber incident which contains collection of logs including: 
to cyber threats for the organisation [1]. This report will show my investigation into into the Boss of the SOCv3(BOTSv3) dataset. This dataset is a Splunk based capture the flag(CTF). Its aim is to simulate a realistic cyber incident which contains collection of logs including 

* network
* endpoint
* email
* cloud data from services like Amazon AWS and Microsoft Azure

By analysing the dataset using Splunk it becomes possible to reconstruct/analyse the attacks behaviour, identify the indications of system compromise and be able to answer guided questions using Splunk. 

This report will be split into 4 sections. SOC roles and incident handling. Which will reflect on how SOC tiers, responsibilities and incident handling methodologies relate to the BOTSv3 exercise. As well as discussing prevention, detection, response and recovery phases. The next section will be installation and data preparation. This is where I will document the installation of Splunk, setting up the dataset. The third section is the guided questions. This is where I will answer the BOTSv3 300 level questions using Splunk queries and analysis. Finaly I will summarise my findings, key lessons and SOC strategy implications as well as highlighting improvements for detection and response. 

# SOC Roles and Incident handling
SOC teams often operate in a structured tiers each have their own roles and responsibilities. There are three different tiers. The first tier is SOC analyst or alert monitoring. This tier is responsible for constantly monitoring alerts and identifying suspicious activity [2]. In relation to the BOTSv3 dataset the tier 1 team would be the first to observe any suspicious activity or anomalies. For example, suspicious email attachments or unexpected cloud activity or unusual authentication attempts. Their role from here would be to determine whether an activity or alert requires an escalation.

Tier 2 is the incident response. This tier is responsible for diving deeper into the logs and/or alerts from tier 1 team. They would also correlate other events to detect any patterns and to try and isolate the compromised host [2]. In relation to the BOTSv3 dataset tier 2 is the area in which this report takes on using Splunk queries to trace the attackers actions, email logs and cloud services forming a report on what has happened and how. 
Tier 3 is the threat hunters. They are responsible for advanced forensics and reverse engineering malicious code so that they can improve their detection for future attacks [2]. Within the BOTSv3 dataset the tier 3 responsibilities would include identifying mechanisms and analysing the malicious scripts that had been detected such as in email attachments or uploaded files to 
the cloud services like OneDrive. 

Each tier would document their findings to help the next tier up or for future use in order to help explain why the incident happened and why and how it can be prevented to stop future attacks. 

The BOTSv3 dataset and the guided questions help to represent SOC workflows by requiring me to investigate logs and correlate events across the sources as well as to critically think about what has happened and where the gaps in the system could be. It also helps to reinforce the need for structure and clear documentation as to help have cross team collaboration and informing relevant people. 

# Splunk installation
To download Splunk Enterprise  a user account is required. After logged in navigate to the Lunix section as I am using Ubuntu. You are met with three options .deb, .tgz and .rpm as seen in the screenshot. I copied the wget for tgz and pasted it into the terminal. Once downloaded it had to be unzipped and moved to the /opt directory. As shown in the screenshot.

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/SplunkWebDownload.png">
<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/UnzippingSplunk.png">

From here navigate to the /opt/splunk/bin and run sudo ./splunk start –accept-license. As it is the first time running Splunk it will ask to enter username and password. After that it will then start running on localhost:8000. When you navigate to localhost:8000 you are met with a login page. 

