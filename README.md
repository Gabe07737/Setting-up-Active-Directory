<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" height="40%" width="70%"alt="Microsoft Active Directory Logo"/>
</p>

<h1>Configuring Active Directory (On-Premises) Within Azure</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<!-- <h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com) -->

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Resources
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>
<br />
<br />
<h3 align="center">Setup Resources in Azure</h3>
<br />
<p>
  Create A Resource Group called RG-AD and a Domain Controller VM (Windows Server 2022). Name it “DC-1”:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/c74e6c4e-c1e8-422b-930a-d082a3fd0854" height="75%" width="100%" alt="resource group"/>
  <img src="https://github.com/user-attachments/assets/36da890e-b44d-4862-877e-13d7c29a5e07" height="75%" width="100%" alt="vm ms server"/>
  <img src="https://github.com/user-attachments/assets/783bb5bd-5266-44b9-9aec-2e183c47e088" height="75%" width="100%" alt="vm ms server"/>
  <img src="https://github.com/user-attachments/assets/b662bb45-a4dc-4196-8dc7-64a941b71a02" height="75%" width="100%" alt="vm ms server"/>
</p>
<p>
  Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in previous step. Make sure it is in the same network as domain virtual machine:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/46ae8329-3ecf-43ef-a049-04b6867d17f9" height="75%" width="100%" alt="vm windows"/>
  <img src="https://github.com/user-attachments/assets/4c684b7b-6de5-468b-8f9b-45035f80d104" height="75%" width="100%" alt="vm windows"/>
  <img src="https://github.com/user-attachments/assets/41307718-93c1-4a55-ac64-1a845607a9c4" height="75%" width="100%" alt="vm windows"/>
</p>
<p>
  Set Domain Controller’s NIC Private IP address to be static:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/5c4f10d6-e0b4-455f-ab19-48348da31adf" height="75%" width="100%" alt="static ip"/>
  <img src="https://github.com/user-attachments/assets/227f194c-e95f-4d45-931b-40e5f760b721" height="75%" width="100%" alt="static ip"/>
  <img src="https://github.com/user-attachments/assets/6374f4bb-16ae-4b8c-b5c4-4a3bb63d4bca" height="75%" width="100%" alt="static ip"/>
</p>
<p>
  From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/13c6e1d0-23fc-4f98-a718-1c3d99e20e31" height="75%" width="100%" alt="client dns settings"/>
  <img src="https://github.com/user-attachments/assets/0a43a6a6-69ea-4ebe-9609-4305b776f1be" height="75%" width="100%" alt="client dns settings"/>
</p>
<br />
<br />
<h3 align="center">Ensure Connectivity between the client and Domain Controller</h3>
<br />
<p>
  Login to Client-1 with Remote Desktop and Attempt to ping DC-1’s private IP address,ensure the ping succeeded:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/89b91251-024d-4df6-887b-984c6a18dacc" height="75%" width="100%" alt="perpetual ping"/>
</p>
<p>
  Login to the Domain Controller and disable the Windows Firewall (for testing connectivity).Make sure the Domain,Private and Public profile tab is off:
<p>
  <img src="https://github.com/user-attachments/assets/126b3f22-596c-4547-b1f6-c8e9ad502672" height="75%" width="100%" alt="enable ICMPv4"/>
  <img src="https://github.com/user-attachments/assets/c33dacf2-cfc0-443b-878a-25c6da6452c7" height="75%" width="100%" alt="enable ICMPv4"/>
</p>
<p> 
   From Client-1, open PowerShell and run ipconfig /all,The output for the DNS settings should show DC-1’s private IP Address
</p>
<p>
  <img src="https://github.com/user-attachments/assets/922e1a52-0e5c-44bf-8a08-3e9d968d08fe" height="75%" width="100%" alt="ping success"/>
</p>
<br />
<br />
<h3 align="center">Install Active Directory</h3>
<br />
<p>
  Login to DC-1 and install Active Directory Domain Services:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/a18a2157-513f-4261-8dfc-ece51d82a509" height="75%" width="100%" alt="active directory install"/>
  <img src="https://github.com/user-attachments/assets/fd991303-597a-4fdc-88b3-308258e87692" height="75%" width="100%" alt="active directory install"/>
  <img src="https://github.com/user-attachments/assets/79a608a9-b8f1-469c-ae10-b2f70da4dd58" height="75%" width="100%" alt="active directory install"/>
</p>
<p>
  Promote as a Domain Controller:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/fc0c5dcb-da3d-4b67-b3d6-0ceefa053f1b" height="75%" width="100%" alt="domain controller promotion"/>
</p>
<p>
  Setup a new forest as mydomain.com (can be anything, just remember what it is - I ultimately did set it up as myadproject.com which you'll see in the next pic):
</p>
<p>
  <img src="https://github.com/user-attachments/assets/89831c0f-7bac-4d8c-9eca-d56e66d46c89" height="75%" width="100%" alt="set new forest"/>
</p>
<p>
  Restart and then log back into DC-1 as user: mydomain.com\labuser:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/0856aa21-3f56-4211-b871-b9d899abe264" height="75%" width="100%" alt="fqdn login"/>
</p>
<br />
<br />
<h3 align="center">Create an Admin and Normal User Account in AD</h3>
<br />
<p>
  In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” and another one called "_ADMINS":
</p>
<p>
  <img src="https://github.com/user-attachments/assets/96a12998-ef61-491a-aacb-93e3d43cf3ac" height="75%" width="100%" alt="organizational unit"/>
  <img src="https://github.com/user-attachments/assets/a18ff4be-bfe7-42f2-a9b1-3f8f8bc035f9" height="75%" width="100%" alt="organizational unit"/>
  <img src="https://github.com/user-attachments/assets/8b0f2572-0123-4a09-b4dd-35373f0a4d5c" height="75%" width="100%" alt="organizational unit"/>
</p>
<p>
  Create a new admin named “Jane Doe” with the username of “jane_admin”:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/233ae5aa-604a-4afd-a4b7-1dff1e102b9e" height="75%" width="100%" alt="admin creation"/>
  <img src="https://github.com/user-attachments/assets/8446e171-e68e-45aa-abca-4d76a7fc43cb" height="75%" width="100%" alt="admin creation"/>
</p>
<p>
  Add jane_admin to the “Domain Admins” Security Group:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/0167e979-b260-4422-ae3c-28f13ae77fcb" height="75%" width="100%" alt="security group"/>
  <img src="https://github.com/user-attachments/assets/6c30fa79-cb89-4c16-8865-ca4455a30cdc" height="75%" width="100%" alt="security group"/>
</p>
<p>
  Make sure to click "Apply" then "Okay" to confirm the changes.
</p>
<p>
  <img src="https://github.com/user-attachments/assets/c657b36a-3e56-4022-a4a2-2e8ba44e10ea" height="75%" width="100%" alt="security group"/>
</p>
<p>  
  Log out/close the Remote Desktop connection to DC-1 and log back in as “myadproject.com\jane_admin”. Use jane_admin as your admin account from now on:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/563c38b2-6abb-4d70-b6e4-1d2eeb646cf9" height="75%" width="100%" alt="admin login"/>
</p>
<br />
<br />
<h3 align="center">Join Client-1 to your domain (myadproject.com)</h3>
<br />
<p>
  From the Azure Portal, restart Client-1.
</p>
<p>
  Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart):
</p>
<p>
  <img src="https://github.com/user-attachments/assets/4f18b1e0-ace7-4b68-ae7a-dd2fd96b2729" height="75%" width="100%" alt="domain joining"/>
  <img src="https://github.com/user-attachments/assets/e66cf005-e4a4-4978-84af-d3de603f7a6a" height="75%" width="100%" alt="domain joining"/>
  <img src="https://github.com/user-attachments/assets/92c23fb4-b871-4fc5-a5ca-4e44ffbbe694" height="75%" width="100%" alt="domain joining"/>
</p>
<p>
  Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.
</p>
<p>
  Create a new OU named “_CLIENTS” and drag Client-1 into there:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/cbf15328-cdfc-49e4-b000-948b33ea1642" height="75%" width="100%" alt="active directory client verification"/>
</p>
<br />
<br />
<h3 align="center">Setup Remote Desktop for non-administrative users on Client-1</h3>
<br />
<p>
  Log into Client-1 as mydomain.com\jane_admin and open system properties.
</p>
<p>
  Click “Remote Desktop”.
</p>
<p>
  Allow “domain users” access to remote desktop.
</p>
<p>
  You can now log into Client-1 as a normal, non-administrative user now.
</p>
<p>
  Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab):
</p>
<p>
  <img src="https://github.com/user-attachments/assets/200ef0ef-c7e5-42cf-89f2-1da74f296e2e" height="75%" width="100%" alt="remote desktop setup"/>
  <img src="https://github.com/user-attachments/assets/4f68917c-27a2-45be-bd90-50763b611f65" height="75%" width="100%" alt="remote desktop setup"/>
  <img src="https://github.com/user-attachments/assets/e00572be-8b19-4270-ae47-788b5719db27" height="75%" width="100%" alt="remote desktop setup"/>
</p>
<br />
<br />
<h3 align="center">Create a bunch of additional users and attempt to log into client-1 with one of the users</h3>
<br />
<p>
  Login to DC-1 as jane_admin
</p>
<p>
  Open PowerShell_ise as an administrator.
</p> 
<p>  
  Create a new File and paste the contents of this script (https://github.com/Xinloiazn/configure-ad/blob/main/adscript.ps1) into it:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/fcbb5fde-df52-4fa3-93d2-e1809a574d53" height="75%" width="100%" alt="create users script"/>
  <img src="https://github.com/user-attachments/assets/7afbe7fc-29e7-451c-b621-a714c26a1309" height="75%" width="100%" alt="create users script"/>
  <img src="https://github.com/user-attachments/assets/0ef2ca27-1c58-4eb9-a221-288838749425" height="75%" width="100%" alt="create users script"/>
</p>
<p>
  Run the script and observe the accounts being created:
</p>
<p>
  <img src="https://github.com/user-attachments/assets/61d567df-e1fe-4369-a7b6-a0bf40cf11af" height="75%" width="100%" alt="observe create users script"/>
  <img src="https://github.com/user-attachments/assets/5321cfdb-929e-4762-9162-6e08c0f42aa4" height="75%" width="100%" alt="observe create users script"/>
</p>
<p>
  When finished, open ADUC and observe the accounts in the appropriate OU and attempt to log into Client-1 with one of the accounts (take note of the password in the script):
</p>
<p>
  <img src="https://github.com/user-attachments/assets/fc6db140-599c-4908-bb0b-c50af19da6be" height="75%" width="100%" alt="employee user accounts"/>
  <img src="https://github.com/user-attachments/assets/a4e5ed5c-8393-4ad0-ae9a-e093c360fab7" height="75%" width="100%" alt="employee user selection"/>
  <img src="https://github.com/user-attachments/assets/17d5097f-3f20-4f32-9557-9261253b478e" height="75%" width="100%" alt="employee user login"/>
</p>
<br />
<br />
<p>
  I hope this tutorial helped you learn a little bit about network security protocols and observe traffic between virtual machines. This can be easily done on a PC or a Mac. Mac would just have an extra step to download the Remote Desktop App.
</p>
<p>
  Now that we're done, DON'T FORGET TO CLEAN UP YOUR AZURE ENVIRONMENT so that you don't incur unnecessary charges.
</p>
<p>
  Close your Remote Desktop connection, delete the Resource Group(s) created at the beginning of this tutorial, and verify Resource Group deletion.
</p>
