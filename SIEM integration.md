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
wget -O splunk-9.4.2-e9664af3d956-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/9.4.2/linux/splunk-9.4.2-e9664af3d956-linux-amd64.deb"
```
![Screenshot 2025-05-30 153721](https://github.com/user-attachments/assets/0d3e9a08-6566-4004-badf-1f57ad0c8426)
- install Splunk enteprise by using the following command
```bash
dpkg -i splunk-9.4.2-e9664af3d956-linux-amd64.deb
```
