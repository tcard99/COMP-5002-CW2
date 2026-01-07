# COMP-5002-CW2

Video: https://www.youtube.com/watch?v=KOkh1FQtEjg

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

To find the user agent string that was associated with the uploaded of a malicious link file to OneDrive. I used ‘ms:o365:management’ as the source type which logs Office365 and OneDrive activity. Running the following query returned 1,073 events.

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q1Firstsearchupclose.png">

The screenshot shows a lot of information about the event like the IP address, user agent, filename. 

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q1FirstSearch.png">

Additional filtering was required to find the user agent. The full user agent string that had uploaded link file was Mozilla/5.0 (X11; U; Linux i686; ko-KP; rv: 19.1br) Gecko/20130508 Fedora/1.9.1-2.5.rs3.0 NaenaraBrowser/3.5b4

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q1SecondSearchupclose.png">
<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q1SecondSearch.png">

User agent strings are useful SOC teams as they can be used to find out if the activity came from expected/approved sources or unexpected/suspicious sources. In this case the user agent used NaenaraBrowser which is a North Korean web browser forked from Mozilla Firefox [6]. This is unusual within the organisation, which would prompt a tier 1 SOC member to escalate this event. 

## Question2

To search for a macro enabled attachment that has been detected as malware you would need to do the following steps. As the file was delivered by email, I looked for email events. Using stream:smtp as the sourcetype and I looked for alerts. I ran the query shown in the screenshot.

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/Q2firstsearch.png">

I added a keyword filter *xlsm* to the end of the query which retuned three results however the file attachments were png and jpg and not relevant. I changed the filter from *xlsm* to *alert*. The query then returned 3 results but only one with an attachment called Malware Alert Text.txt. This is part of Office365 Advanced Threat protection where unsafe attachments are removed and are substituted for a text file [7] named above.

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q2txtfilebeforedeeper.png">

Looking at the information for this event and the email it stated here is the financial model that could be used for FY2019 with instructions saying to enable the macros. Reading on the file name was encoded using base64. Using cyberchef [8] I decoded the attachment name: Frothly-Brewery-Financial-Planning-FY2019-draft.xlsm.

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q2Rawdata.png">
<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q2base64convert.png">

Tier 1 SOC members would escalate this as malware; the other tiers would investigate if the email/attachment/macros had been opened and if any response/measures needed to take place.

## Question3

This question wanted me to find the name of the executable that was embedded in the malware from Q2. Using XmlWinEventLog:Microsoft-Windows-Sysmon/Operational as the source type. Following on from this as new the file name I added this to the query search. 

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/Q3Search.png">
<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q3Resukt.png">

Looking at the result, the name of the file was HxTsr.exe

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q3answerCloseup.png">

## Question4

This question wanted me to find the password for the user created by root. Linux uses useradd/adduser to create a new account.

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q4Firstsearch.png">
<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q4firstsearchresult.png">

After research I found that to view logs for login attempts, root/sudo usage you look at /var/log/auth.log [9]. From the initial search 67 events came back including auth.log

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q4sourcelist.png">

Adding to the search showed a user with UID 0 which = root [10] created a user called tomcat7

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q4varauth.png">

Next search was:

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q4tomcat7search.png">

Which returned 8 events 

<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q4tomcat7result.png">

Looking at the text next -p which is password when making new users showed ilovedavidverve was the password

For SOC teams identifying the password from a malicious user creation allows them to confirm whether an attacker has successfully established connection into the system as well as being able to detect other user account creations. Detecting this activity allows SOC to disable, reset passwords and look for any other unauthorised changes.

## Question5

To find the name of the user that was created after the endpoint was compromised without knowledge if the endpoint is Windows or Linux. To find the new user I used WinEventLog:Security as source and searched for eventcode 4720. 
The new account name was svcvnc.
 
<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q5Quiery.png">
<img src="https://github.com/tcard99/COMP-5002-CW2/blob/main/Screenshots/q5answer.png">

SOC detecting new user creations is important especially after being compromised. To further help in the creation eventcode 4720 is generated when a new local windows account has been created [11]. Once detected SOC teams can disable/monitor the account.

## Question6