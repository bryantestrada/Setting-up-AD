# Setting-up-Active-Directory
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Getting Started
- Create a Domain controller (DC) and Client VM
- Set your DC's Private IP to static
- Ensure connectivity between the Client and the Domain Controller
- Create an Admin
- Join the Client VM to your Domain controller

<h2>Deployment and Configuration Steps</h2>

<h2>Getting Started</h2>
<p>
First, open portal.azure.com and sign into your account; If you don't have one, sign up for a free 30-day subscription and create a username and password (tenant). When you first sign up for Microsoft Azure, during your 30-day trial, you are given 200 credits that you can use to access paid services such as VMs, Storage accounts, etc.
<p>
<br />
  
 <img width="352" alt="Screenshot 2023-07-25 at 6 43 40 PM" src="https://github.com/bryantestrada/Deploying-AD-within-Azure-VMs/assets/133170900/897c1a9f-91eb-4cd7-941b-eb03d6539b88">
 
<br />
<h2>Create a Domain controller and Client VM</h2>
<p>
Next, we will create a Virtual Domain Controller to manage our server and host Active Directory; Navigate to Virtual Machines and create a new VM. 
</p>
<img width="512" alt="DC0" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/ae4efd49-6e12-4818-adcc-84c69377498a">
<p>
Now first, click Create New and name the Resource Group (RG_AD) that will store the VMs we will create; Next, name your Domain Controller (DC1) and select the region it will be hosted in (use the same region for your client VM); Then, select Windows Server 2022 in the image tab as the OS for the Domain controller.
</p>
<img width="512" alt="DC1" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/6ecec165-399f-44fc-b2b6-dabad1620fa4">
<br />
<p>
Next, scroll down, select the size, and choose 2 Vcpus for smooth performance; During the free trial, you are limited to only using four Vcpus simultaneously between all your VMs. Input a username and password that will be used to log into your DC. Click Next: Disks.
</p>
<img width="512" alt="DC2" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/b332b1f2-2710-400d-91a3-eaea39577a11">
<br />
<p>
Click Next: Networking; Take note of the Domain Controller's virtual network; the DC and the client VM are on the same Vnet. Then, click Review and Create.
</p>
<img width="512" alt="DC3" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/0d595348-5b7d-4acb-b244-ac3157fb6f5c">
<img width="512" alt="DC4" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/4cb21a60-c28f-4184-ab97-1f9ac9c714db">
<p>
While the Domain Controller is deploying, navigate to Virtual Machines and create a new VM as our client computer.
</p>
<img width="512" alt="Client0" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/c17b4b56-8788-4ae8-aefd-273d38c82623">
<br /> 
<p>
Store it in the same Resource Group as your Domain Controller (RG_AD), name the VM (Client-1), select the same region used for the DC, and select Windows 10 Pro, version 22H2, as the OS for the Client VM. 
</p>
<img width="512" alt="Client1" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/24d6eebb-346d-4c1d-a19d-1a1fcd3438f2">
<br />
<p>
Scroll down, select the size (2 Vcpus), enter a username and password used to log into the Client VM, and check the licensing box. Then click Next: Disks.
</p>
<img width="512" alt="Client2" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/dca745ed-279f-495a-a856-c775454e32bd">
<br />
<p>
Click Next: Networking, ensure the Client VM is on the DC's network, then click Review and Create.
</p>
<img width="512" alt="Client3" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/a0c1ed6d-3cf8-4b15-a458-63a9fdd9daae">
<br />
<h2>Set your DC's Private IP to static</h2>
<p>
Navigate to your Virtual machines and select your Domain controller (DC1) to see its overview; Then, navigate to DC1's networking tab under settings, and click the Domain controller's Network Interface Card (NIC) number. 
</p>
<img width="512" alt="NIC" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/24834f17-b932-498d-885a-eeb826401be4">
<br />
<p>
Once you enter the NIC's overview, navigate to IP configurations, and select "Ipconfig1". Change its "allocation" from dynamic to static and click Save.
</p>
<img width="640" alt="NIC1" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/c795d275-d815-49e1-b7dc-5c10d0c8a771">

<br />
<h2>Ensure connectivity between the Client and the Domain Controller</h2>
<p>
Copy Client One's public IP and use it to sign in using a remote desktop.
</p>
<img width="512" alt="RD0" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/9787ee0b-19ab-4427-bd84-f928d7f6fc71">
<br />
<p>
Sign in using Client One's login credentials.
</p>
<img width="512" alt="login" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/68cc5238-529e-49fc-804c-e366778c27a2">
<br />
<p>
Open and use Client One's command line and send a perpetual ping (ping -t) to DC1's private IP address.
</p>
<img width="640" alt="Priv IP" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/c75545c7-89a4-4714-9674-e5918eb48e62">
<img width="511" alt="CMD0" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/04d2c6b2-418e-4ce0-bf83-d3ccacd7b308">
<img width="640" alt="Ping" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/20f5cea8-865c-44e3-96f3-dfd90608e6fa">
<br />
<p>
Next, add your Domain Controller's public IP to the remote desktop and log in using its credentials;
</p>
<img width="512" alt="Pub IP" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/4ac8b797-83d6-407a-ac88-6b8792ba8d75">
<img width="512" alt="DC IP" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/301d587e-8120-42f3-80e1-2b6e34c1eb64">
<br />
<p>
Open Windows Defender Firewall with Advanced Security; Click the inbound rules folder and sort the protocol to filter ICMPv4 traffic.
</p>
<img width="640" alt="Firewall" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/fcd683fe-2b2c-421e-8278-25939b82e4be">
<img width="512" alt="Inbound" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/72899efe-0200-40dd-8b1d-12a781342b2d">
<img width="640" alt="Protocol" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/dc6fc738-64c9-4ad0-92fc-6d25d5b609a3">
<br />
<p>
Enable both ICMPv4 requests under the core networking diagnostics group.
</p>
<img width="1090" alt="Enable" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/5b8e820a-a3c1-4a3e-893f-ec343b971530">
<br />
<p>
Then, go back to the Client's VM, and DC1 should be replying to the ping. Press Control+C to stop the ping.
</p>
<img width="640" alt="CMD1" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/78cd1957-9d68-44d0-b4ea-de6553876a15">
<br />
<h2>Install Actiive Directory</h2>
<p>
Start in the Domain Controller's remote desktop. The server manager should open as soon as you log in to the VM, but if not, you can access it through the start menu. 
</p>
<img width="640" alt="Serv Man" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/afd0cf46-87a3-43e4-8978-c4823eebf782">
<br />
<p>
Once open, click Add Roles and Features.
</p>
<img width="640" alt="roles" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/645bfcd5-d5f5-4a76-a019-ab603fc0bb4d">
<br />
<p>
Click next and select Active Directory Domain Services as a server role.
</p>
<img width="640" alt="domain" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/fbf4e9a4-5a5c-4bf3-9afc-c00d1fc2a647">
<br />
<p>
Continue next and install the Active directory on your Domain controller.
</p>
<img width="640" alt="install" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/8355cd36-707f-428f-a154-e8f77f0b46f9">
<br />
<p>
Once installed, close the install window, and click the flag notification to promote DC1's server to a Domain Controller.
</p>
<img width="640" alt="Flag" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/b946bccb-7824-40c2-afc9-a66661a8656d">
<br />
<p>
Select Add a new Forest and add a Root domain name (mydomain.com); Then click next and add a password. Click next and finish installing the Active Directory. Once finished, the VM will restart. 
</p>
<img width="640" alt="Root name" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/0a71fbba-7679-4e35-a41e-d3c9a8630e9a">
<img width="512" alt="pass" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/31ca8e0f-7fa4-43c7-aa77-7a098d6ca42a">
<img width="640" alt="AD in" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/90df7ecb-0c78-406b-9d7f-1ad5e08c8cd0">
<br />
<p>
Log back in using the root domain name (labuser@mydomain.com)and the password for your DC.
</p>
<img width="640" alt="New" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/9088ab46-fe89-48c8-814d-f9dcd73390c6">
<br />
<h2>Create an Admin</h2>
<p>
Go back to the server manager and open Active Directory Users and Computers; under "tools" in the top right of the screen.
</p>
<img width="640" alt="ADuc" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/350e6185-3229-4b1c-a9fb-3772fe5b6341">
<br />
<p>
Open the drop-down list; under your root domain name and add two Organizational Units (OUs) called "_ADMINS" and "_EMPLOYEES" by right-clicking inside the Root Domain's folder; Then, refresh the Root Domain.
</p>
<img width="640" alt="menu" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/0e3097b0-a5b7-425b-a4c5-01d90d3475b3">
<img width="640" alt="OU" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/a157bfb7-4781-4a2a-ac9e-5176f26e35f7">
<img width="640" alt="Refresh" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/cb108172-dfc5-4e38-afbf-dac0be30f5a4">
<br />
<p>
Create a new user inside the "_ADMINS" folder, and give it a name (Jane Doe) and a username (Jane.admin). Click next and give it a password. You can use these credentials to do any administrative tasks using any computer connected to your domain controller; First, you must add the new user to the "Domain Admins" Security Group.
</p>
<img width="640" alt="user" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/817d5a59-96fa-4d7d-9ddb-1ae86cd81fb6">
<img width="640" alt="User1" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/3f3fc920-b003-4fae-a6f8-1b925251814c">
<img width="640" alt="Userpass" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/332eea4d-fff6-4404-acad-535ff971637c">
<br />
<p>
Right-click the user inside the "_ADMINS" folder and select properties, then click the "Member Of" tab, and click "Add..."; Then type Domain Admin in the text box, click check names, click OK, and click Apply. The user is now part of the Domain Admins Security Group.
</p>
<img width="640" alt="Properties" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/15c16c9f-d00d-404c-9b98-1fc52bc3c2dc">
<img width="640" alt="Member" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/8ba7b5eb-9f9f-4c80-a75b-a1436edf3972">
<img width="640" alt="Check Names" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/df50ac2a-2f73-4042-a316-7dae503a304a">
<br />
<p>
Log out and log back in using the new admin's credentials.
</p>
<img width="640" alt="new user" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/b1b0d061-414e-48ad-ac15-08cbeaae4c1e">
<h2>Join the Client VM to your Domain controller</h2>
<p>
From the Azure portal, set the client VMs DNS settings to the Domain Controller's private IP address; So that the client VM can use the DC as its DNS server. First, copy the DCs private IP, Next, navigate to the client VMs networking tab under settings, and click the client VMs Network Interface Card (NIC) number. From there, navigate to "DNS server" under settings, click custom DNS server, input the DCs private IP, and click Save.
</p>
<img width="640" alt="Priv IP" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/6f393d31-fde1-4922-b62f-3e4c92506273">
<img width="640" alt="Client NIC" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/98d7dc5b-5e90-4a78-8176-aaad7f9ae8f2">
<br />
<p>
Then, restart the client;
</p>
<img width="640" alt="Restart" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/49e9191e-2db2-436e-9004-10f7f1232388">
<br />
<p>
Login to the Client VM as the original local admin. Once logged in, right-click the start menu and click System to open the settings. 
</p>
<img width="640" alt="Client VM" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/01e12070-c72b-4b9e-8724-2244a80c6f24">
<img width="634" alt="start" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/2cf37b7d-9622-47c9-914c-f3955b9b66c0">
<br />
<p>
Click Rename this PC (advanced), click "Change.." and select "Domain" to input your Domain name; Finally, click OK and enter the Admin credentials you created.
</p>
<img width="640" alt="Rename" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/8e592072-0b95-4506-a6a4-e94a2500e1cf">
<br />
<img width="400" alt="change" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/9976d154-5f37-4120-a24e-d75afda7a6ce">
<br />
<img width="400" alt="change2" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/dbc19d79-21b3-4c97-a09a-a5846f2289c3">
<br />
<img width="512" alt="Changes 3" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/51440106-b9db-4d18-a14e-272ee14d580b">
<br />
<p>
A popup will appear, welcoming you to the "Domain," and then the Client VM will restart.
</p>
<img width="400" alt="popup" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/c0064da1-b30d-4a7a-a7c8-0a5d041a8787">
<br />
<p>
Login to the Domain Controller through Remote Desktop using the Admin credentials and verify the client VM shows up in Active Directory Users and Computers; inside the computer's container on the root of the "domain."
</p>
<img width="640" alt="comp" src="https://github.com/bryantestrada/Setting-up-AD/assets/133170900/c423b31f-f348-47e0-a0f3-efbbd355c59e">
<p>
And that's it; You should now be able to create a Domain controller and Client VM using Microsoft Azure, install and configure Active Directory, and join the Client VM to the domain to access it using an admin account. Finally, disconnect from the remote desktop and delete your "Resource Group."
</p>
