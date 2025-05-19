# Active-directory-project
## Overview
In this project, I'll be simulating a real world Active Directory enterprise environment. In which I'll setting up an active directory environment with two Windows servers. Splunk will be configured as the SIEM tool and log aggregator.

Shuffle will be used as the SOAR platform to detect and respond to suspicious activity using the SOC playbook. and finally slack will be used for communication

## Contents 
- [Overview](#Overview)
- [Lab setup](#Lab-setup)
- ()


# Architecture
This lab will be built primarily using these three virtual machines:

1. Domain Controller:
This server will be configured as the domain controller for the Active directory environment and will be running Windows server

2. Agent:
This will act as the target machine and will also be running on Windows server

3. Splunk Server:
This will act as the host server for Splunk, the SIEM solution in charge of collecting and analyzing data from the Windows servers

All three virtual machines will be hosted in the cloud and configured to work together, the two servers acting as Splunk forwarders and the Server as a centralized aggregator

### Additional components
1. Shuffle: Shuffle  will act as the SOAR platform and will handle automated incident response. It will recieve alert from splunk and execute playbooks based on a set of conditions.

2. Slack: Slack will act as the communication tool and will provide real time alerts to the SOC analyst.

Attacker machine:
This will be used to simulate all the attacks that will be performed on the target.

![AD lab architecture](https://github.com/user-attachments/assets/31f8a90a-8fb1-4115-be75-7d7f0679e3e2)

## Lab Setup

