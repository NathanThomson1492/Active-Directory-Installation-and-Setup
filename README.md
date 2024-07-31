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
Now that we have both our server and our client machines created, let's use Remote Desktop to log into Client-1 and test the connection between the two devices. After logging into Client-1 open up an instance of command prompt and utilize the 'ping' command to test Client-1's connectivity to the server. 
</p>


![5 ping_DC1_from_CLIENT1](https://github.com/user-attachments/assets/470ee415-1745-43d6-ae56-573d9ba0fc26)
<br />
![8 Install_ActiveDIrectory](https://github.com/user-attachments/assets/5c3c106e-0eeb-44c0-8dd3-4af5e4680e3c)
<p>
 lorem ipsus 
</p>

![9 promteDC1toDOMAINCONTRLLER](https://github.com/user-attachments/assets/6743c7b9-3f3d-4fdc-ad68-ee0ad54c8b28)
<p>
 lorem ipsus 
</p>

![11 CREATE_ORGINIZATIONAL_UNITS](https://github.com/user-attachments/assets/ccce09c2-950a-4099-b119-9f88d12ade9e)

<p>
 lorem ipsus 
</p>

![12_NEW_ORGINIZATIONAL_UNITS](https://github.com/user-attachments/assets/0b4e42a9-4e35-4e2b-9c4e-0ad71fabf3f7)
<p>
 lorem ipsus 
</p>

![13create_admin_useraccount](https://github.com/user-attachments/assets/b1e15e1c-0931-43ae-9fb1-b7cb68e11b7e)

<p>
 lorem ipsus 
</p>

![14 add_adminaccount_to_domainadmins_group](https://github.com/user-attachments/assets/a51f4162-d077-4d8e-b428-fb8794f0cc76)

<p>
 lorem ipsus 
</p>

![15-16 LOGINasNEWadminACCOUNT](https://github.com/user-attachments/assets/86a35612-ed61-458c-b056-827c2d88e89d)
<p>
 lorem ipsus 
</p>


![17 changeDNSforCLIENT1](https://github.com/user-attachments/assets/3bcd9806-b163-4024-8c1d-6c2e9c81d3ee)

<p>
 lorem ipsus 
</p>

![19 changeDOMAINforCLIENT1](https://github.com/user-attachments/assets/2ecec55a-61d3-4326-8841-02744c6410d2)

<p>
 lorem ipsus 
</p>


![20 verifyCLIENT1is_inAD](https://github.com/user-attachments/assets/472161ee-af33-4539-b31f-2f543a5ae427)

<p>
 lorem ipsus 
</p>

![22-26ADDdom_usersTOclient1](https://github.com/user-attachments/assets/424fe53a-458b-498d-8006-6610d249f31e)

<p>
 lorem ipsus 
</p>


![ADD3nuUSERS](https://github.com/user-attachments/assets/ce575289-cca4-483a-9521-5e264109b862)

<p>
 lorem ipsus 
</p>

![BENDOElocksOUTofACCOUNT](https://github.com/user-attachments/assets/76d27db0-3d5b-4c94-9e71-b2063019e3c0)

<p>
 lorem ipsus 
</p>


![Enable_ICMP_DC-1](https://github.com/user-attachments/assets/9d077f6a-4799-4eda-9f1e-07b0e8cd1532)









