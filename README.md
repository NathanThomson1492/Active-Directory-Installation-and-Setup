# Active-Directory-Installation-and-Setup
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>


This tutorial will outline the implementation of on-premises Active Directory within Azure Virtual Machines.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Domain Controller)
- Remote Desktop
- Active Directory Domain Services
  

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Introductin to Active Directory</h2>
<p>
Windows Active Directory is a directory service developed by Microsoft for Windows domain networks. It provides a centralized and standardized system for automating network management of user data, security, and distributed resources, and enables interoperation with other directories. Active Directory stores information about objects on the network and makes this information easy for administrators and users to find and use. Active Directory simplifies management and improves security by offering a single point of control for network resources and policies. It is a critical component for organizations using Windows servers and clients, helping to streamline operations and ensure that users have secure, reliable access to the resources they need.
</p>

<h2>High-Level Deployment and Configuration Steps</h2>

1. Create Resource Group, Virtual Network, and Domain Controller
2. Configure Domain Controller's IP address to be static
3. Create a Windows 10 client (virtual machine)
4. Ensure connectivity between client and domain controller
5. Install Active Directory on Domain Controller
6. Promote Windows Server VM as a domain controller create a domain
7. Join client machine to your domain
8. Setup client machine for non-admin user access
9. Create additional user accounts 

<h2>Deployment and Configuration</h2>
<p>
To start things off we first need to log into Azure to create our virtual machine that will serve as our server that will eventually be promoted into a domain controller. To do this we start by navigatiing to the virtual machines tab and then choose "create new virtual machine". We then need to choose the proper region that our machine is held in as well as the machine name, image and resource group. Remember for the machine that you will set up as a domain controller, it must be set up as a server. In this lesson we will be using Windows Server 2022 as our image.
</p>

![image](https://github.com/user-attachments/assets/67f58894-710d-4164-96eb-f857e40ff212)
<br />

<br />
<p>
After we are done with the configuration and creation of our new server we need to ensure that the machine's IP address does not change. We do this to ensure that the hidden DNS feature in our virtual network does not automatically reassgin a new IP address to our domain controller in the middle of our activities. To do this we must navigate to DC-1 and then on the IP configurations. From their you will see two options under "IP Allocations". Choose static and then click on the save button to change the IP configurations to static.  

![2 DC-1_static_IP](https://github.com/user-attachments/assets/fadc0c23-f1ba-4076-ad2f-32694a641f29)
<br />


<br />
<p>
Next we will go ahead and setup our client machine. The process is very similar to what we just accomplished with our domain controller. For this exercise we will name our client machine "Client-1" Remember to choose the same region as we did with DC-1 and to use a Windows 10 machine as our image since this virtual machine will be acting as our normal office computer that our hypothetical end users will be conducting business on. Another important point to address is to ensure that our new machine is added to the same virtual network as DC-1.
</p>

![3 create_client_machine](https://github.com/user-attachments/assets/22081c27-44f8-427e-aa35-d7328f88ed6d)

<br />


<p>
Now that we have both our server and our client machines created, let's use Remote Desktop to log into Client-1 and test the connection between the two devices. After logging into Client-1 open up an instance of command prompt and utilize the 'ping' command to test Client-1's connectivity to the server. Note that our ping request is timing out as our domain controller is currently not allowing ICMP traffic which is what the ping command uses to send it's packets. 
</p>


![5 ping_DC1_from_CLIENT1](https://github.com/user-attachments/assets/470ee415-1745-43d6-ae56-573d9ba0fc26)
<br />

<p>
Log in to the Domain Controller, and once you have successfully accessed the system, proceed to enable ICMPv4 on the local Windows Firewall. To do this, navigate to the firewall settings and locate the inbound rules. Then, create a new rule or modify an existing rule to allow ICMPv4 traffic, ensuring that the necessary permissions are set to facilitate the desired network communication. Next we can return to Client-1, use the ping command again and see that our requests are going through and thus verifying that Client-1 can communicate with our domain controller as can be seen in the following two images. 
</p>

![6 Enable_ICMP_DC-1](https://github.com/user-attachments/assets/b07e44e6-16a3-4ca6-ac10-16b6ddf168e0)

![7 verify_ping_to_DC1](https://github.com/user-attachments/assets/b437c2f7-b281-4f3f-bc71-4fed30d420e6)
<br />
<h2> Active Directory Installation</h2>
<p>
  Now we can proceed to installing Active Directory on our Domain Controller. Begin by logging into DC-1. Once you have successfully accessed the system, proceed to install the Active Directory Domain Services (AD DS) role. This can be done through the Server Manager by adding roles and features and selecting AD DS from the list. Ensure that you install Active Directory Users and Computers and proceed by installing the necessary dependencies. 
</p>

![8 Install_ActiveDIrectory](https://github.com/user-attachments/assets/5c3c106e-0eeb-44c0-8dd3-4af5e4680e3c)
<br />
<p>
Next, you will need to promote the server to a Domain Controller (DC). To do this, initiate the promotion process and select the option to set up a new forest. When prompted, specify the new forest name as "mydomain.com" and follow the steps to complete the configuration. 
</p>

![9 promteDC1toDOMAINCONTRLLER](https://github.com/user-attachments/assets/6743c7b9-3f3d-4fdc-ad68-ee0ad54c8b28)
<br />

<h2>Active Directory Configuration</h2>
<br />
  
<p>
After the promotion process is complete, restart the server to apply all changes. Once the server has rebooted, log back into DC-1 using the credentials for the newly created domain. You should log in as the user "mydomain.com\labuser" to ensure proper access to the domain controller functions and features.
</p>

<p>Open Active Directory Users and Computers (ADUC). Within the ADUC console, navigate to the desired location in the directory tree where you would like to create a new Organizational Unit (OU). Right-click on the appropriate node (as seen in the image below), select "New," and then choose "Organizational Unit." Name this new OU "_EMPLOYEES" to organize your employee accounts. </p>

![11 CREATE_ORGINIZATIONAL_UNITS](https://github.com/user-attachments/assets/ccce09c2-950a-4099-b119-9f88d12ade9e)

![12_NEW_ORGINIZATIONAL_UNITS](https://github.com/user-attachments/assets/0b4e42a9-4e35-4e2b-9c4e-0ad71fabf3f7)
<br />
<br />

<p>
 Next, you will need to create another Organizational Unit. Follow the same steps by right-clicking on the desired node, selecting "New," and then "Organizational Unit." Name this new OU "_ADMINS" to manage your administrative accounts separately.
</p>

![13create_admin_useraccount](https://github.com/user-attachments/assets/b1e15e1c-0931-43ae-9fb1-b7cb68e11b7e)
<br />
<br />
<p>
 After creating the OUs, you will need to add a new employee account. Right-click on the "_EMPLOYEES" OU, select "New," and then "User." Enter the details for the new employee, naming the account "Jane Doe" and assigning a username of "jane_admin." Ensure that you set the same password as used for other administrative accounts for consistency. 
</p>

<p>Once Jane Doe's account is created, you will need to add her to the appropriate security group. Locate the "Domain Admins" Security Group in ADUC, right-click on it, and select "Properties." Go to the "Members" tab, click "Add," and type in "nathan_admin" to add her to the "Domain Admins" Security Group. This grants her the necessary administrative privileges within the domain. </p>

![14 add_adminaccount_to_domainadmins_group](https://github.com/user-attachments/assets/a51f4162-d077-4d8e-b428-fb8794f0cc76)
<br />
<br />

<p>
 Finally, to ensure that the new account has been properly configured and the changes have taken effect, log out or close the Remote Desktop connection to DC-1. Then, initiate a new Remote Desktop connection and log back in using the credentials for "mydomain.com\jane_admin." This verifies that the new administrative account is functioning correctly.
</p>

![15-16 LOGINasNEWadminACCOUNT](https://github.com/user-attachments/assets/86a35612-ed61-458c-b056-827c2d88e89d)
<br />
<br />
<p>
Access the Azure Portal using your preferred web browser. Once you are logged in, navigate to the virtual machine settings for Client-1. Locate the DNS settings for this virtual machine and update them to reflect the Domain Controller’s (DC) private IP address. This change ensures that Client-1 uses the Domain Controller for DNS resolution.
</p>

<p> After updating the DNS settings, remain in the Azure Portal and find the option to restart Client-1. Initiate a restart of the virtual machine to apply the new DNS configuration settings. </p>

![17 changeDNSforCLIENT1](https://github.com/user-attachments/assets/3bcd9806-b163-4024-8c1d-6c2e9c81d3ee)
<br />
<br />

<p>
Once Client-1 has restarted, use Remote Desktop Protocol (RDP) to log into the machine. Log in using the credentials for the original local administrator account, typically "labuser." After successfully logging in, proceed to join Client-1 to the domain. This involves accessing the system properties, navigating to the domain settings, and entering the appropriate domain name. Confirm the action and allow the computer to restart to complete the domain joining process.
</p>

![19 changeDOMAINforCLIENT1](https://github.com/user-attachments/assets/2ecec55a-61d3-4326-8841-02744c6410d2)
<br />
<br />

<p>
 Following the restart of Client-1, establish a Remote Desktop connection to the Domain Controller. Log into the Domain Controller using the appropriate administrative credentials. Open the Active Directory Users and Computers (ADUC) console. In the ADUC interface, navigate to the “Computers” container located at the root of the domain. Verify that Client-1 is listed in this container, confirming that it has successfully joined the domain. 
</p>

![20 verifyCLIENT1is_inAD](https://github.com/user-attachments/assets/472161ee-af33-4539-b31f-2f543a5ae427)
<br />
<br />

<p>
Begin by logging into Client-1 using the credentials for the administrative account "mydomain.com\jane_admin." Once you have successfully logged in, navigate to the system properties by right-clicking on "This PC" or "Computer" on the desktop or in File Explorer, and then selecting "Properties." From there, access the "Advanced system settings" link to open the System Properties dialog box. 
</p>

![22-26ADDdom_usersTOclient1](https://github.com/user-attachments/assets/424fe53a-458b-498d-8006-6610d249f31e)
<br />
<br />
<p>
 In the System Properties window, click on the "Remote" tab to access the Remote Desktop settings. Within the Remote Desktop section, select the option to allow remote connections to this computer. Additionally, click on the "Select Users" button to specify which users or groups are permitted to access the system via Remote Desktop. 
</p>

![ADD3nuUSERS](https://github.com/user-attachments/assets/ce575289-cca4-483a-9521-5e264109b862)
<br />

<p>
 
</p>

![BENDOElocksOUTofACCOUNT](https://github.com/user-attachments/assets/76d27db0-3d5b-4c94-9e71-b2063019e3c0)
<br />

<p>
  
</p>











