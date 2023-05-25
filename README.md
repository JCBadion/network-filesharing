# network-filesharing
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Connecting Networks and Filesharing Within the Cloud (Azure)</h1>
This tutorial outlines the process of filesharing between a Server Host and its network.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>List of Prerequisites</h2>

- Active Directory is set up. [Learn how to set up and configure here](https://github.com/JCBadion/configure-ad)
- Client VM is connecting to Server Host

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Folders on Server Host and Set Permissions (DC-1)
- Navigate Created Folders from Client-1
- Create New Security Group and Permissions

<h2>Deployment and Configuration Steps</h2>

<p>
<h3>DC-1</h3>
<img src="https://i.imgur.com/JbxD7oq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h3>Client-1</h3>
<img src="https://i.imgur.com/uBjOZcr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h3>DC-1 New Folders</h3>
<img src="https://i.imgur.com/j3ghhlj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First, make sure you are logged into the Server Host (DC-1) with an administrator user (Use Jane_Admin from our previous tutorial linked above). Next, log in to Client-1's VM with one of the non-administrative usernames that we created in the previous tutorial (For this example we will continue using Fico Xidu). To make sure you don't have the two VMs mixed up, open up the command prompt in each one and type in "whoami" and "hostname" to confirm you logged into the correct machines. Next, back in DC-1, open up file explorer and navigate to the local disk (C:) and in there, create three new folders titled as:
  
- Read-Access
- Write-Access
- No-Access
- Accounting

We will be editing these folders with different permissions that determine whether non-administrative users can access them or not. 
</p>
<br />

<p>
<h3>Adding Permissions to Folders</h3>
<img src="https://i.imgur.com/lCa4Rqv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
For each of the folders, right-click on them and go to 'Properties'. Next, go the to 'Sharing' tab and click 'Share...'. To include all users in the domain type in "Domain Users" and click add. For the read Folder, Allow the Domain Users to only read the file. For the write folder, allow users to both read and write on the file. As for the No-Access Folder, because domain users that no access to it there is no need to modify that folder. For now we can skip editing the "Accounting" folder and we will come back to it later.
</p>
<br />

<p>
<h3>Client-1 Shared Network to DC-1</h3>
<img src="https://i.imgur.com/39BOaSZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h3>Denied Access to Modify</h3>
<img src="https://i.imgur.com/njtbLqu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h3>Access to Read & Modify</h3>
<img src="https://i.imgur.com/w7fCOGu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h3>Denied Access</h3>
<img src="https://i.imgur.com/hk576if.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

</p>
<p>
Let's go to Client-1 and look at the newly created folders on the shared-network. To do this, go to File Explorer and click on the bar at the top, and type in "\\DC-1". This will take you to the shared network and you will be able to see the shared files, as shown above. When we try to access different folders, we will notice that for some of the folders we have more permissions than others. For example, in the Read Folder we can only read the material or save what's in it to our VM, but we cannot directly modify anything within.
  
However, if we access the Write Folder, not only can we read what is inside it, but we can also modify any file within it and the changes can be seen for everyone on the network to see.
  
Lastly, notice how we can't access the No-Access Folder because as a non-administrative user, you do not have permission to neither modify or even read what is in this folder.
  
Permissions like in these examples are useful for increasing security within a domain, as well as to prevent more sensitive information from leaking.
</p>
<br />

<p>
<h3>Active Directory New Security Group</h3>
<img src="https://i.imgur.com/zbC4fpN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h3>Accountant Permissions</h3>
<img src="https://i.imgur.com/spZ02SQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Let's create a new group to utilize these permissions. In the DC-1 VM, go to the start menu and go to "Active Directory Users and Computers". Under the Users section in the "mydomain" category, we will create a new security group called "Accountants". Go back to File Explorer (C:) and modify the Accounting Folder so that only the Accountants Security Group can access this folder. When we try to access this folder as our current user in Client-1, we will be denied access because we are not part of the Accountants group.
</p>
<br />

<p>
<h3>Adding Security Group to User</h3>
<img src="https://i.imgur.com/QK7YbTN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To gain access to this folder, we will go back to the Active Directory Users and Computers. And in the _EMPLOYEES folder we will change the security group of one of the users there. For this example we will use Pira.Hule. To change their security group, right-click on a user and go to Properties. Then click on the 'member of' tab and click "add". Type in the newly-created "Accountants" Security and press "apply". This will now categorize Pira.Hule as a user in the Accounting department. To test this out we will log out of Client-1 and log back in as Pira.Hule.
</p>
<br />

<p>
<h3>Accounting now accessible</h3>
<img src="https://i.imgur.com/AaRMXGZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once logged back in as Pira.Hule, we can go to the file explorer again and type in \\DC-1 to open up the Shared Network with DC-1 and we can see that the Accounting folder is there. With our current permissions as part of the Accounting Security Group, we are now able to access the accounting folder and read/write whatever would be in it.
  
These branches of permissions can go even further by giving employees access to read and write in some folders while denying access to entire files within those same folders. This is just one of many ways that companies can keep information safe and secure!
</p>
<br />
