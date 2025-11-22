# Active_Directory_Lab

I built an Active Directory environment using VMware. This lab uses two virtual machines:
- Windows Server 2022 - Domain Controller
- Windows 10 Client
Both machines are connected using VMware network adapters, and this shows how a real internal corporate network works.

## Network Diagram Explanation

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Network%20Diagram.png?raw=true">

- This Diagram Shows my Active Directory setup. The Domain Controller (172.16.0.1) provides DNS, DHCP, Gateway to the Internal Networks. Windows 10 receives an IP from the DHCP scope (172.16.0.100-200) and uses the Domain Controller for both DNS and Internet routing. This setup shows a small corporate network environment.

## Network Adapter Configuration

I created two network adapters:
- NAT Adapter - This one connects to our internet, assigns an IP Address automatically.
- LAN Segment - This one is used to connect the internal network, especially connecting with Windows 10 clients.

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%201.png?raw=true">

## Assign a Static IP address
I assigned the static IP address to the internal network on the Domain Controller, so the server always has the same IP address and the client can reliably find it.
- IP Address - 172.16.0.1
- Subnet Mask - 255.255.255.0

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%2013.png?raw=true">

## Installed Active Directory Domain Service
I installed Active Directory Domain Service, which can manage users, computers, and group policies within the domain. Right after installation, I did the Post Deployment Configuration.
- Root Domain Name - mydomain.com

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%203.png?raw=true">

## Creating an Organizational Unit
I created an Organizational Unit by opening Active Directory Users & Computers
- Organizational Unit - Admins
- Under the Admins - Iman Kasthuri

I gave Admin power to the user Iman Kasthuri. Admins have full control over the whole network; they can add new users, delete users, etc.

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%204.png?raw=true">

## Adding RAS/NAT TO the Domain Controller
I installed the Remote Access role on the Domain Controller, so the Client Windows can access the internet through the Server.

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%205.png?raw=true">

## Adding a DHCP Server to the Domain Controller
Added a DHCP Server, so the Windows 10 Client automatically assigns an IP address, DNS, and Gateway without manually setting it.
- DHCP Range - 172.16.0.100/200

We set a DHCP range because the server must know which DHCP IP address it can use for the clients. Windows 10 clients get an IP address from this range, like 172.16.0.100/200.

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%206.png?raw=true">

## Creating Users
I used a PowerShell script to automatically add a larger number of users (1000 users) for my lab environment. The purpose of this script is to simulate a large company with many users. For further reference, I added my script file separately.

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%207.png?raw=true">

## Fixing Windows 10 Client not Getting an IP Address
During the setup of my Windows 10 Client, the machine was receiving the APIPA Address 169.254.178.190. This indicates the client is not receiving DHCP from the Domain Controller.

### Issue

- Typo issue in the Domain Controller Internal Network Adapter. I typed 172.160.0.1, but the correct IP Address was 172.16.0.1
- The DHCP Server is missing the Router 003 option, which tells the client what the Gateway is 


<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%209.png?raw=true">

### Fix

- I manually added the missing DHCP Router 003 option, set to 172.16.0.1 and then restarted the DHCP service.
- I changed the Internal Network Adapter IP address to 172.16.0.1
- Disabled the Internal Network Adapter for a few seconds and enabled it again
- The next step was to verify DHCP Bindings, which means I checked that DHCP was bound to the Internal Network.
- I used PowerShell to restart the DHCP (Restart-Service dhcpserver)

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%208.png?raw=true">

- After fixing the missing DHCP Router 003 option and typo mistake, the Windows 10 client machine successfully received an IP address from the Domain Controller
  
 <img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%2010.png?raw=true">
 
## Verifying Network Connectivity

I verified that my Windows 10 client was connected to the Domain Controller, and I checked that the Windows 10 client could reach the Domain Controller using the Ping Command.
- Ping 172.16.0.1

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%2011.png?raw=true">

## Domain Join
Finally, I tested the AD connectivity by joining the Domain.

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%2012.png?raw=true">

## Conclusion

This project helped me to understand how  a real Active Directory environment works, including DNS, DHCP, NAT, User Management, and Domain Joining. And also, I gained hands-on experience in troubleshooting network issues. Overall, this lab strengthened my foundational IT and Cybersecurity skills.

## Credits

This Active Directory lab project was inspired by Josh Madakor’s “Active Directory Home Lab” YouTube tutorial.  
All configuration, troubleshooting, and documentation in this repository were done by me.






