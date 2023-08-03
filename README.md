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

- Setup Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into client-1 with one of the users



<h2>Deployment and Configuration Steps</h2>


<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/737aa4d4-4969-428f-a878-ffad06466d7d" height="50%" width="50%"/>
<p>
1. Create the Domain Controller VM (Windows Server 2022) named “DC-1” with 2vcpu's for efficiency
  a.Set a Resouce Group name and take note of it and the Virtual Network (Vnet) that gets created under the networking tab.<br/>
*Keep note of credentials used*</p>
  <br/>
  <br/>
  
<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/c8270412-ed41-4da8-9745-e1532a30f061"  height="50%" width="50%"/>
<p>2. Set Domain Controller’s NIC Private IP address to be static</p>
   <br/>
   <br/>
   
<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/9745f392-0260-4d9e-9ac2-1654959bd2e1" height="50%" width="50%"/>
<p>3. Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
 <br/> 4. Ensure that both VMs are in the same Vnet<br/>
  *Keep note of credentials used*
</p>
<br />
<br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/83827681-13d1-473d-89e5-d06f4a8304b0" height="50%" width="50%"/>
<br/>
<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/361b6d19-a23f-48c2-9672-e3936ca7ef9e" height="50%" width="50%"/>
<p>5. Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)</ip>
  <br/>
  <br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/c5fb079a-231c-49f1-9fe9-b5fc8e3d8b9d"  height="50%" width="50%"/>
<p>6. Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall</p>
<br/>
<br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/181101b3-0d57-401a-981a-f2ec3d1ac5c2"  height="50%" width="50%"/>
<p>7. Check back at Client-1 to see the ping succeed</p>
<br/>
<br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/29b3d017-83c5-4a46-ae28-f8bd239d3c15" height="50%" width="50%"/>
<p>8. On DC-1 go to Server Manager--> Add Roles and Features--> install Active Directory Domain Services.</p>
<br/>
<br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/fd4af794-f871-4b98-b9ef-bc94bc549052" height="50%" width="50%"/>
<p>9. Click the notification in Server Manager and promote as a Domain Controller. Setup a new forest. <br/>
*Keep note of the credentials used during this step* In this example we'll use mydomain.com. <br/>
10. Restart and then log back into DC-1 as user: mydomain.com\labuser (labuser is the username we used to create DC-1.)<br/>
*The IP address for DC-1 might have changed. Make sure to check in the Azure Portal.</p>
<br />
<br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/c1da1fcf-ff72-4abb-b261-9f45117364dc" height="50%" width="50%"/>
<p>11. In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
  <br/> * Right click mydomain.com--> new--> organizational unit*<br/>
12. Create a new OU named “_ADMINS”</p>
  <br/>
  <br/>
  
<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/5ea860a3-04b8-4d0d-abbd-e04e20264e66" height="50%" width="50%"/>
<p>13. In the OU _Admins, create a new employee for this example we will name the employee “Jane Doe” with the username of “jane_admin”</p>
<br/>
<br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/143c5d1f-4978-491a-bdd2-6abcc6a2f53c" height="50%" width="50%"/>
<p>14. Add jane_admin to the “Domain Admins” Security Group. Right click on Jane Doe--> Properties--> Member of--> Add--> type Domain Admins and add.</p>
<br/>
<br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/d4b266b3-8bf1-4b2e-9d08-352f4b56db92" height="50%" width="50%"/>
<p>15. Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”<br/>
16. User jane_admin as your admin account from now on</p>
<br/>
<br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/5114582b-297e-4f11-a7bb-b3d996595d2e" height="50%" width="50%"/>
<p> 17. From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address.<br/>
18. From the Azure Portal, restart Client-1</p>
  <br/>
  <br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/ba2b59dd-d720-4f51-b998-59b5bafce20f" height="50%" width="50%"/>
<p>19. Login to Client-1 (Remote Desktop) as the original local admin (labuser)<br/>
20. Join Client-1 to the domain. Right click the start on the bottom left--> System--> Rename this PC (advanced)--> Change--> Click on Domain and type mydomain.com.   (computer will restart)</p>
<br/>
<br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/7768f532-5b06-4f81-adec-f8fb5450a59e" height="50%" width="50%"/>
<p>21. Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain</p>
<br/>
<br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/90ce5911-97c9-46a1-be46-1f73f6706fa1" height="50%" width="50%"/>
<p>22. Log into Client-1 as mydomain.com\jane_admin and open system properties<br/>
23. Click “Remote Desktop”<br/>
24. Allow “domain users” access to remote desktop
25. You can now log into Client-1 as a normal, non-administrative user now</p>
<br/>
<br/>

<p>26. Login to DC-1 as jane_admin</p>
<br/>
<br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/26333f40-f773-4643-b18d-35dfa9f0869e" height="50%" width="50%"/>
<p>27. Open PowerShell_ise as an administrator<br/>
  *search powershell ise then right click and click on open as an administrator.*</p>
  <br/>
  <br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/808e0c4e-7539-4d95-9c4b-18fbf3f0c6db" height="50%" width="50%"/>
<p>28. Create a new File and paste the contents of the <a href="https://www.github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1" title="Script"/> script</a> into it.<br/>
29. Run the script and observe the accounts being created. </p>
<br/>
<br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/4fdf2f90-711d-4688-936a-c9301e1a99a2" height="50%" width="50%"/>
<p>30. When finished, open ADUC (Active Directory Users and Computers) and observe the accounts in the appropriate OU.</p>
<br/>
<br/>

<img src="https://github.com/d-mccardell/configure-ad/assets/116754993/d7d928b7-0015-4ffc-8f0e-ab8c2e4203ac" height="50%" width="50%"/>
<p>31. Attempt to log into Client-1 with one of the accounts (take note of the password in the script)</p>






