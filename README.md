<h1>Active-Directory-Lab</h1>

<h2>Description</h2>
Project consists of using VirtualBox to emulate Microsoft Server 2019 and setting up an "coorporate-like" enviornment that contains, DNS, DHCP, and over 1,000 users.
<br />

<h2>Environments Used </h2>

- <b>Windows 10</b>
- <b>Windows Server 2019</b>
<h2>Lab walk-through:</h2>

1. Create VM:
   - Create a new virtual machine using the Windows Server 2019 ISO.

2. Configure Network:
   - Rename Network Adapters:
   - Access Settings/Network/Ethernet/Change adapter options.
   - Rename both NICs for clarity, distinguishing between internal and external.

3. Set IP Addresses for Internal Network:
   - Navigate to the properties of the internal network card.
   - Go to IPV4 settings and assign the following:
   - IP Address: 172.16.0.1
   - Subnet Mask: 255.255.255.0
   - Default Gateway: Not required (Domain controller serves as default gateway.)
   - DNS Server: Use loopback address - 127.0.0.1 (refers to itself).

This configuration ensures proper networking within the internal environment, with the domain controller serving as both the default gateway and DNS server.

4. Install Active Directory Domain Services
   - Select add roles and features from the Server Manager dashboard.
   - Pick the server where you want to install Active Directory Domain services.
   - Choose active directory domain service.
   - Select next then Install.

5. Post Deployment Configuration
   - Under post deployment configuration select “Promote this server to a domain controller”.
   - Add a new forest and name your domain accordingly.
   - Add a password for the domain.
   - Select next then install (this will restart your computer.)

6. Create Dedicated Domain Admin Account
   - Under administrative tools select AD users and computers.
   - Create a new OU under your newly created domain named Admins.
   - Create a new user and name accordingly.
   - Add the user to a group called “Domain Admins”.
   - (You can test this by signing out and signing back in as another user.)

7. Routing and Remote Access
   - In the server manager dashboard under tools select routing and remote access.
   - By right clicking on the local server select configure and enable.
   - Follow the instructions on the wizard and select NAT to allow an internal client to connect to the internet using one public IP address.
   - Then select your internet facing network interface to be used.

8. Set Up DHCP server
   - In the dashboard select “add roles and features” .
   - Select DHCP server under server roles.
   - Select next then install.

9. Creating DHCP Scope
   - Under tools select DHCP.
   - Under IPV4 select “view scope” and name accordingly.
   - Set your Ip address range as needed including the subnet mask.
   - Set your Lease duration as needed as well.
   - Set your domain controller address as the default gateway.
   - (You may need to click authorize on your domain server for these actions to take effect.)

 10. Source Code for Powershell Script
      - (For this lab we will be using a script that will automatically make around 1,000 new users.)

 11. Windows 10 Client
     - Set up a new VM using a windows 10 ISO.
     - Under system in settings, go to “rename this pc advanced”.
     - Select “member of Domain” and type in your domain name. (This will require a username and password from an already joined member of the domain.)
     - (This will restart your pc.)

 12. Verifying Client Joining
     - Under DHCP and address leases you will be able to view your client computer and its lease given by the DHCP server. You will also be able to view the client computer in “AD users and computers. 
