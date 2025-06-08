# SIEM integration
As stated in the projects overview the SIEM solution that I'll be using is Splunk, and it will be configured and setup on an Ubuntu server.

## installing Splunk
To install splunk on the Ubuntu Server:
- SSH into the server and make sure all the system packages are updated using  
```bash 
sudo apt update && sudo apt upgrade
```
- I'll download Splunk enteprise with the following command:
```bash
sudo wget -O splunk-9.4.2-e9664af3d956-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/9.4.2/linux/splunk-9.4.2-e9664af3d956-linux-amd64.deb"
```
![Screenshot 2025-05-30 153721](https://github.com/user-attachments/assets/0d3e9a08-6566-4004-badf-1f57ad0c8426)
- install Splunk enteprise by using the following command
```bash
sudo dpkg -i splunk-9.4.2-e9664af3d956-linux-amd64.deb
```
- once installed cd to to directory splunk was installed on 
```bash
cd /opt/splunk/bin
```
- Start the service
```bash
./splunk start
```
- After the installation is complete, set a username and password
- Access the Splunk dashboard with a browser using the servers IP address and the port the Splunk service listens on.


### Configuring Splunk
I'll be configuring Splunk to enable it receive and index logs from the endpoints seamlessly
- Step 1: I'll be installing the Splunk add-on for windows to helps parse and normalize Windows log data.
- Step 2: Create a new index to store all the logs from the endpoints
- Step 3: Configure the port Splunk will be receiving the logs on (Port 9997) and add a new Azure NSG inbound rule to allow traffic on port 9997 to the Splunk VM.
- Step 4: Download and Install the Universal Forwarder on the Agent and Domain Controller  
  1. Download the Splunk Universal Forwarder from the official Splunk website.
  2. Run the installer.
  3. During the setup, select "Use the universal forwarder with an on-prem Splunk Enterprise instance."
  4. Create a username for the Universal Forwarder.
  5. Set the receiving indexer IP address to the IP of your Splunk server, and use port 9997 as the receiving port.
  6. Copy the inputs.conf file from `C:\Program Files\SplunkUniversalForwarder\etc\system\default` to `C:\Program Files\SplunkUniversalForwarder\etc\system\local`
  7. Open the inputs.conf file in Notepad, and add the following lines at the end of the file:
   ```conf
   [WinEventLog://Security]
   index = <name-of-index>
   disabled = false
   ```
# Creating Splunk Alerts & Rules

Now that Splunk is getting telemetry from both the domain controller and the agent, it's time to set up rules to watch for suspicious activity, and alerts that will trigger when something happens.

---

## Alert for Multiple Failed RDP Login Attempts

We want to create an alert for multiple unsuccessful RDP login attempts on our agent.

- Open the **Search & Reporting** app in Splunk.
- Use this SPL search to find failed RDP logins (Event ID 4625, Logon Type 10) that come from outside our trusted IP ranges:

```spl
index=ad-lab EventCode=4625
| where NOT like(Source_Network_Address, "10.%") AND NOT like(Source_Network_Address, "192.168.%") AND NOT like(Source_Network_Address, "172.1%") AND NOT like(Source_Network_Address, "172.2%") AND NOT like(Source_Network_Address, "172.3%")
| stats count by Source_Network_Address, Account_Name, host
```
- This query filters for failed RDP logins from IPs outside our private network ranges, then counts how many attempts came from each IP and which accounts and hosts were targeted.

- click Save As > Alert.
- **Title:** Failed External RDP Logins  
- **Type/Schedule:** Scheduled (Cron: `0 * * * *`, runs hourly) | **Trigger Actions:** Add to triggered alerts, Severity: High
- **Expiration:** 7 days from creation  

### What I found
For this demonstration, I turned off the network firewall, as expected there was a huge spike in failed RDP login attempts. One notable IP address consistently showed up **45.134.26.142**
