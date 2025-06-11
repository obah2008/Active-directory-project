
# Shuffle integration
In this section of the project, I'll be creating a shuffle workflow, and integrating that with our AD environment. but before we get into that, let's understand what an SOAR is and why shuffle is so Important.

## SOAR(Security Orchestration Automation and Response)
Security Orchestration, Automation, and Response (SOAR) refers to a set of tools that help security teams build automated workflows and pipelines for detecting, analyzing, and responding to security events.  
These tools connect different security systems (like SIEMs, firewalls, and communication platforms), allowing teams to automate repetitive tasks.

## Shuffle 
Shuffle is a SOAR tool that allows users to automate processes and create workflow pipeline using drag and drop blocks, without having to write a single line of code.

### Shuffle setup
- The first step to setting up shuffle is to create a new [shuffle](https://shuffler.io/) account.
- With my shuffle account setup, I can make a new workflow which I'll be calling Obah-AD
- On the new workflow dashboard we can now create a new webhook(A way for one application to send data to another when a specific event occurs)
- Copy the webhook URI(uniform resource Identifier)
- head over to the Splunk dashboard, Navigate to the alert we created in the [SIEM integration](https://github.com/obah2008/Active-directory-project/blob/main/SIEM%20integration.md) section. Edit the Alert and add the copied URI to webhook section
- Headback to the shuffle dashboard and start the webhook.
- Create A [slack](https://slack.com/) account
- Create a new Slack workspace
- Drag the slack application unto the workflow, and name it Alert-notification
- Authenticate slack with the workspace we created 
- Create a new channel on slack called alerts
- Edit the test field under slack on the Shuffle dashboard
```bash
Alert: $exec.search_name \nIP address: $exec.result.Source_Network_Address \nUser: $exec.result.Account_Name \nhost: $exec.result.host
```
- On the **Slack** dashboard head over to the channel we created a few steps ago, and copy the channel ID from the URL
- Back on the **Shuffle** dashboard paste the channel ID on on the channel section on the Shuffle-slack app
- On slack we can now see the alerts from splunk
- Now we can create an email that's sent to the SOC analyst. using the User input trigger we can create a message that is sent to the SOC analyst to trigger the rest of the playbook
![image](https://github.com/user-attachments/assets/5feec893-93ae-4182-a7a0-5dd5ae23f2fa)

### Creating a playbook
A playbook are step by step in that tell a security team what to do in case of an incident. Our playbook in this case will be a set of actions that would be triggered when a condition is true  
Playbook details: The playbook goes like this
| Step | Description |
|------|-------------|
| 1    | Splunk detects multiple failed RDP login attempts and triggers an alert |
| 2    | Shuffle receives the alert and starts the playbook |
| 3    | A Slack message is sent to the SOC analyst with alert details and a button to block the IP |
| 4    | If the analyst clicks "Block IP", the playbook proceeds to block the attacker's IP using Windows Firewall |
| 5    | A confirmation message is sent back to Slack, informing the analyst of the result |

- We've already created part of the playbook to collect the alerts from splunk and alert the soc analyst, The next step will be choosing a new trigger for user input. I'll name it "Actions" in the information field of the user_input trigger. under the input option field chose email and enter the SOC email, we can create the message that will be sent to the SOC analyst by pasting what is below in the text field
```bash
Do you want to block the source IP address? Start parameters: 
```
- Now when ever the splunk alert is triggered we receive an email from slack asking if we want to block the source IP.

![Screenshot 2025-06-11 010621](https://github.com/user-attachments/assets/a18ca240-9c70-4c23-baf2-498039a33fb6)

- The next step is using Shuffle to trigger a firewall rule to block an IP based on the response the SOC analyst gives Slack response.

- To do this I'll be using Azure firewall

## Setting up Azure firewall and integrating it to slack
- Create a new firewall policy
- Select the same resource group and region the virtual machines were installed in
- Create a new IP group.
- Create a Rule collection in the firewall policy we created

## Note:
I original planned on using a PFsense rule, 
