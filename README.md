# COMP-5002-CW2

## Introduction
A Security Operations Centre (SOC) is responsible for continuously monitoring an organisation’s systems, detecting malicious activity, and coordinating incident response [1]. The Boss of the SOC v3 (BOTSv3) exercise provides a realistic Splunk‑based simulation of these responsibilities through a multistage cyber incident affecting the fictional company Frothly. The dataset includes network, endpoint, email, and cloud logs, enabling analysts to reconstruct attacker behaviour and identify indicators of compromise.
The objective of this investigation is to analyse the BOTSv3 dataset using Splunk, answer the 300‑level guided questions, and evaluate how SOC roles, processes, and incident‑handling methodologies apply to the scenario. The report also reflects on prevention, detection, response, and recovery activities relevant to the incident.
* Scope
 * Analysis is limited to the BOTSv3 dataset and Splunk environment.
 * Only the provided log sources are used
 * Focus is on the 300‑level questions and their relevance to SOC operations.
* Assumptions
 * BOTSv3 logs are complete and accurately represent the incident timeline.
 * Splunk is assumed to be correctly configured.
 

# SOC Roles and Incident handling
SOC teams operate in structured tiers; there are three different tiers, with each tier contributing a distinct layer of defence during a cyber incident.

The first tier is analyst or alert monitoring. They are responsible for monitoring alerts and identifying suspicious activity [2]. In relation to the BOTSv3 dataset this tier team would be the first to observe any suspicious activity or anomalies. Their role would be to determine whether an activity or alert requires an escalation.

Tier 2 is the incident response; they are responsible for diving deeper into the logs and/or alerts from tier 1 team. They also correlate other events to detect any patterns and to try to isolate the compromised host [2]. In relation to the BOTSv3 dataset tier 2 would use Splunk queries to trace the attackers' actions, email logs and cloud services forming a report on the situation. 

Tier 3 is the threat hunters; they are responsible for advanced forensics and reverse engineering malicious code so that they can improve their detection for future attacks [2]. Within the BOTSv3 dataset their responsibilities would include identifying mechanisms and analysing the malicious scripts that had been detected such as in email attachments or uploaded files to the cloud services.

The BOTSv3 exercise demonstrates how SOC tiers and incident handling methodologies operate as a cohesive system. Tier 1 identifies potential threats, tier 2 investigates and contains them, and tier 3 provides deep analysis to strengthen future defences. Together, these roles support the full incident lifecycle from initial detection to long term recovery and prevention, highlighting the importance of structured processes, comprehensive log visibility, and continuous improvement in SOC operations [3].


# Installation and Data Preparation
To investigate the BOTSv3 dataset and answer the guided questions Splunk was installed and configured to run on an Ubuntu Virtual Machine (VM). Splunk is a tool for data like logs or events allowing users to search the data and analyse it. For SOC teams it allows them to detect, investigate and respond to cyber threats in real time allowing easy access/readability to the dataset [4].

## Splunk installation
To download Splunk an account is required. After logging in, navigate to the Lunix section as I used Ubuntu. I copied the wget for tgz and pasted it into the terminal. Once downloaded it had to be unzipped and moved to the /opt directory. 

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/SplunkWebDownload.png">
<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/UnzippingSplunk.png">

From here navigate to the /opt/splunk/bin and run sudo ./splunk start --accept-license. As it is the first time running Splunk it will ask to enter username and password. After it will then start running on localhost:8000. When you navigate to localhost:8000 you are met with a login page. Once have logged in you will see the dashboard confirming that the installation was successful.
<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/splunkLoginPage.png">


## Data Preparation
Once Splunk had been set up I needed to download the BOTSv3 dataset from GitHub[5] and then load dataset into Splunk. Once downloaded the file needed extracting.

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/SplunkDashboard.png">
 
<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/BOTSv3Git.png">
Before loading the BOTSv3 dataset Splunk was stopped using sudo ./splunk stop. Once extracted use the terminal to copy it into Splunk so that it can be accessed. To copy use the following command sudo cp -r botsv3_data_set /opt/splunk/etc/apps/. Putting the dataset in this location means that Splunk can access the logs; this allows the dataset to be queried and analysed. 
<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/BOTSv3CopyCommand.png">

Using Splunk on a VM is how SOC teams set up their environment as it ensures Splunk performs reliably without affecting the host system. VM is a lightweight and stable Operating System (OS) and works well with Splunk. SOC teams typically rely on isolated, controlled, and reproducible environments for analysis, testing, and training. The BOTSv3 dataset includes malware samples, malicious macros, and attacker tools. Analysing these within a VM ensures they cannot interact with the host OS or network, aligning with SOC containment principles.
# BOTSv3Questions
## Question1
