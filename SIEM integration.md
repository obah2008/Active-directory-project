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
