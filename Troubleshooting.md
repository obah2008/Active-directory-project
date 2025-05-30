## Issue 1:
The client failed to resolve the Active directory domain name while attempting to join the agent to the domian.
### Troubleshooting
To see if the client can resolve the AD's domain name, we can run:
`nslookup <domain name>` 
output
```powershell
Server: Unknown
Address:XXX.XXX.XXX.XXX
Request to Unknown timed-out
 ```

- The output of the command shows the agent is in capable of resolving the domain name, we can now set static routes on the agent and set a preffered DNS server address


- Configuring static routes on the agent:  
    We can manually configure the agents IPV4 static routes.  
      1. IP Address: x.x.x.x  
      2. Subnet mask: 255.255.255.0  
      3. Default gatewatay: 10.0.0.1(Azure's default gafetway)  
      4. Preferred DNS Server: Domain controller's IP  
 1. Set a static IP address on the Domain Controller(It should be the same as that of the IP address of the "preferred DNS server" on the agent)
- Restest DNS resolution on the Agent:  
`nslookup <domain name>`  
      We still encounter the non existent domain error. 
      This suggests the problem might be the domain controller.

- Configure the static routes on teh Domain controller:  
  1. Set a static IP address on the Domain Controller(It should be the same as that of the IP address of the "preferred DNS server" on the agent)  
  2. Set the Domain controller's preferred DNS server to it's own private IP
  
- Restart DNS services on the Domain controller:
  To apply the changes:
``` Powershell
ipconfig /flushdns
ipconfig /registerdns
net stop dns
net start dns 
```
- Retry DNS lookup from the Domain controller:
run `nslookup obah.AD-lab`  
The expected output should be   
```
Server: <Ad domain name>
Address: x.x.x.x
Name: <AD domain name>
Address: x.x.x.x
```
- We can now try joining the the Agent to the AD domain once more.

![Screenshot 2025-05-25 115827](https://github.com/user-attachments/assets/685488e4-86c4-4c1f-a656-4c2c12a89a4c)

 

  
