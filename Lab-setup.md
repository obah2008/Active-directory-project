## Lab Setup
### Cloud VM Deployment
All three virtual machines used in this project will be hosted in the cloud. For this project, I'll be using Microsoft Azure as the cloud provider. Azure was chosen because of its seamless integration with Active Directory. Since I’ll primarily be using the $200 student credit Azure provides, this lab is designed to be as cost-effective as possible.

#### 1. Domain Controller(DCn1)
As earlier mentioned we'll be using Windows server 2022 as the operating system for Domain controller. for the domain controller I'll be using a B2s instance

Specs:
- CPU: 2vCPU
- Ram: 4GB ram
- Storage: 128GB
- Operating system: Windows Server 2022

#### 2. Agent
The agent VM will also run Windows Server 2022. However, we’ll use a B1ms instance since the workload on this machine will be minimal.

Specs:
- CPU: 1vCPU
- Ram: 2GB ram
- Storage: 64GB
- Operating System: Windows Server 2022

#### 3. Splunk Server
The Splunk server will run Ubuntu 22.04, and we’ll use a DS1_v2 instance. This provides sufficient processing power, memory, and storage for log indexing and analysis.

Specs:
- CPU: 1vCPU
- Ram: 3.5GB ram
- Storage: 128GB
- Operating System: Ubuntu 22.04

### Installing the Virtual Machines

#### Deploying the Domain controller VM
On the Azure dashboard, we'll hover over the VM dashboard and select create new Azure machine,  
- Resource group: I'll create a new resource group and name it AD-lab
- Virtual machine name:  DCn1
- Region: I'll be selecting the region closest to me, South Africa
- Image: I'll be using a windows server 2022 ISO
- Size: B2s instance
- Inbound traffic: I'll allow inbound traffic from RDP(port 3389
- Virtual Network: AD-Vnet

#### Deploying the Agent VM
Folowing the same steps as used to deploy the domain controller VM, we'll create one for the Agent, but instead of using a B2s We'll use a B1ms instance instead
- Resource group: We'll set the resource group as AD-lab
- Virtual machine name: I'll name the virtual machine Agent
- Region: Selected the region closest to me
- Image: Windows server 2022 
- Size: B1ms instance
- Inbound traffic: inbound traffic from RDP 3389
- Virtual Network: AD-Vnet

#### Deploying the Splunk Server
For the splunk Server we'll be using Ubuntu 22.04 and a Ds1 instance
- Resource group: AD-lab
- Virtual machine name: Splunk-server
- Region: South Africa
- Image: Ubuntu 22.04
- Size: DS1v2
- Inboud traffic: Allow inboud traffic from SSH(port 22)
- Virtual Network: AD-Vnet

Since all of the virtual machines are assigned to the same virtual network they can communicate with each other.
The next step is to take note of the private address of each VM

With all the VMs up and running we can now RDP into our Domain controller and Agent, and send pings to test connectivity


### Installing Active Directory
On the DCn1 Server, we can set up active directory and promote the Server to a Domain controller by 
- Opening server manager
- On the quick start menu, select Add roles and features
- Accept default configurations unto we get to Server roles, where "Active directory domain services" will be selected, and then I'll add features
- Default configurations will be followed until we finish the installation. 

### Promoting the Server to a domain controller
Back on the Server dashboard
- Navigate to the top right corner and click on the flag icon 
- On the post deployment configuration section. click on "promote this server to a domain controller"
- Select add a new forest
- Name the Root domain
- Add a DSRM password
- Accept default configuration until install is complete
- After the installation is complete, the server reboots to apply changes

### Creatting new AD users
With the server now fully installed:
We can head over to "AD users and computers" to begin creating new users
On the "AD users and computers" window, we can create a new user by right clicking and selecing new and then User. Give the user a name and assign a password.

#### Joining the Agent to the Active directory domain
- Navigate to Computer name/domain changes
- Select member of domain and input the name of the root domain we used when setting up AD on the domain controller
 
 If you have an issue like the domain name could not be found or located, click [here](https://github.com/obah2008/Active-directory-project/blob/main/Troubleshooting.md) to see how to resolve that issue

- once successful, restart the agent to save changes
