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

- Setup resources in Microsoft Azure
- Ensure connectivity between the client and Domain Controller
- Install Active Directory 
- Create an Admin in Active Directory 
- Join the client to the domain
- Setup Remote Desktop for non-administrative users
- Create additional users and attempt to log into the client 

<h2>Deployment and Configuration Steps</h2>

<p>
<img width="500" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/71ff2fab-8f10-4997-9aa5-d46baa82f572/"
</p>
<p>
After setting up my resources in Microsoft Azure, I needed to ensure connectivity between the Domain Controller and the client. To do this I first initiated a perpetual ping from the client to the Domain Controller to see if it succeeds (it should fail). Then I enabled ICMPv4 on the Domain Controller's local Windows Firewall. 
</p>
<br />

<p>
<img width="500" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/92ba30ed-fcf9-4fe1-b4cf-0b7a46f2e24d/"
</p>
<p>
After enabling ICMPv4 on the Domain Controller's Firewall, I observed the perpetual ping from the client succeeding and I have now ensured connectivity between the client and Domain Controller. 
</p>
<br />

<p>
<img width="500" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/c512afa9-5668-4f4d-a452-92429cac5680">
</p>
<p>
Next, I logged into the Domain Controller and installed Active Directory Domain Services using the Server Manager. 
</p>
<br />

<p>
<img width="500" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/6c5e5174-3cef-49d1-9b19-d5f2b1cfa309">
</p>
<p>
Next, I promoted the Domain Controller and set up a new forest with the root name "mydomain.com".
</p>
<br />

<p>
<img width="500" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/c2616421-2aa2-4711-bae1-b43672a1c392">
</p>
<p>
Next, in Active Directory Users and Computers, I created two organizational units "_EMPLOYEES" and "_ADMINS". 
</p>
<br />

<p>
<img width="500" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/db80465f-00d7-4656-a959-67bdf707bac4">
</p>
<p>
Next, I created a new user as an "employee" named Jane Doe. Then I added Jane Doe to the Domain Admins Security Group to make Jane an admin. 
</p>
<br />

<p>
<img width="400" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/76f272f4-a1f1-4f7e-bdcf-13b340fba1f7">
</p>
<p>
Next, I disconnected the remote connection to the Domain Controller and logged back in as Jane Doe using the username and password I created. 
</p>
<br />

<p>
<img width="500" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/6ddba145-2079-4c03-8873-823e06d06c1b">
</p>
<p>
Next, from the Azure Portal, I set the client's DNS settings to the Domain Controller's private IP address. 
</p>
<br />

<p>
<img width="400" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/4a563a77-115a-48a1-b742-8d3f2687bae3">
</p>
<p>
Next, I logged into the client virtual machine and joined it to the domain "mydomain.com".
</p>
<br />

<p>
<img width="500" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/3ebdebaf-d2eb-4da7-87e5-b5407b305f84">
</p>
<p>
Next, I logged back into the Domain Controller to verify that the client had joined the domain. I went to Active Directory Users and Computers, checked inside the "Computers" container on the root of the domain, and verified "Client-1" was there. 
</p>
<br />

<p>
<img width="500" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/1985f5df-657f-4e6c-a43d-b1f8bcd4e83f">
</p>
<p>
Next, I logged into the client as mydomain.com\jane_admin. Inside system properties I allowed all domain users (non-administrative) to remotely access the desktop. 
</p>
<br />

<p>
<img width="500" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/46631afa-1aab-4142-bd38-b71d974e9dc2">
</p>
<p>
Next, on the Domain Controller, I opened PowerShell ISE as an administrator and ran a script to create 100 random users on the domain.
</p>
<br />

<p>
<img width="500" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/c28516ce-f5d3-459f-be8b-0ce5656c37d0">
</p>
<p>
I observed the random users generated inside ADUC (Active Directory Users and Computers) and took note of the first user "bapule.cavu". I will attempt to log in to the client with this user. 
</p>
<br />

<p>
<img width="500" alt="image" src="https://github.com/devinlanouette904/configuread/assets/142081954/ba848154-23c6-4663-bc64-687964fc2936">
</p>
<p>
I successfully logged in to the client as "bapule.cava", a random user on the domain.
