# Active Directory Lab

I built an Active Directory environment using VMware. This lab uses two virtual machines:
- Windows Server 2022 - Domain Controller
- Windows 10 Client

Both machines are connected using VMware network adapters, which simulates how a real internal corporate network operates.

## Network Diagram Explanation

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Network%20Tropology.png?raw=true">

- This diagram shows my Active Directory setup. The Domain Controller (172.16.0.1) provides DNS, DHCP, and gateway services for the internal network. The Windows 10 machine receives an IP address from the DHCP scope (172.16.0.100–200) and uses the Domain Controller for both DNS and internet routing. This setup models a small corporate network environment.

## Network Adapter Configuration

I created two network adapters:
- NAT Adapter -This one connects to the internet and automatically assigns an IP address.
- LAN Segment - This one is used to connect the internal network, especially for communication with Windows 10 clients.

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%201.png?raw=true">

## Assign a Static IP address
I assigned a static IP address to the internal network on the Domain Controller so the server always uses the same address and the client can reliably locate it.
- IP Address - 172.16.0.1
- Subnet Mask - 255.255.255.0

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%2013.png?raw=true">

## Installed Active Directory Domain Service
I installed the Active Directory Domain Services role, which allows the server to manage users, computers, and group policies within the domain. Immediately after the installation, I completed the Post-Deployment Configuration.
- Root Domain Name - mydomain.com

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%203.png?raw=true">

## Creating an Organizational Unit
I created an Organizational Unit by opening Active Directory Users and Computers.
- Organizational Unit - Admins
- Under the Admins - Iman Kasthuri

I granted administrative privileges to the user Iman Kasthuri. Administrators have full control over the entire network—they can add new users, delete users, and manage all domain resources.

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%204.png?raw=true">

## Adding RAS/NAT TO the Domain Controller
I installed the Remote Access role on the Domain Controller so the Windows client can access the internet through the server.

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%205.png?raw=true">

## Adding a DHCP Server to the Domain Controller
I added a DHCP server so the Windows 10 client automatically receives an IP address, DNS, and gateway settings without needing manual configuration
- DHCP Range - 172.16.0.100/200

We set a DHCP range because the server needs to know which IP addresses it can assign to clients. Windows 10 clients receive an IP address from this range, such as 172.16.0.100–172.16.0.200.

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%206.png?raw=true">

## Creating Users
I used a PowerShell script to automatically add a large number of users (1,000 users) to my lab environment. The purpose of this script was to simulate a large company with many users. For reference, I have included the script file separately.

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%207.png?raw=true">

## Fixing Windows 10 Client not Getting an IP Address
During the setup of my Windows 10 client, the machine was receiving an APIPA address (169.254.178.190). This indicates that the client was not receiving a DHCP address from the Domain Controller.

### Issue

- There was a typo in the Domain Controller’s internal network adapter configuration. I entered the IP address as 172.160.0.1, but the correct address was 172.16.0.1.
- The DHCP server was missing the Router (003) option, which tells the client what the default gateway is.


<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%209.png?raw=true">

### Fix

- I manually added the missing DHCP Router (003) option and set it to 172.16.0.1, then restarted the DHCP service.
- I corrected the Internal Network Adapter IP address to 172.16.0.1.
- I disabled the Internal Network Adapter for a few seconds and then re-enabled it.
- I verified the DHCP bindings to ensure the DHCP server was bound to the internal network.
- I used PowerShell to restart the DHCP using (Restart-Service dhcpserver)

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%208.png?raw=true">

- After fixing the missing DHCP Router (003) option and correcting the typo, the Windows 10 client successfully received an IP address from the Domain Controller.
  
 <img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%2010.png?raw=true">
 
## Verifying Network Connectivity

I verified that my Windows 10 client was connected to the Domain Controller, and I confirmed connectivity by successfully pinging the Domain Controller from the client.
- Ping 172.16.0.1

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%2011.png?raw=true">

## Domain Join
Finally, I tested the Active Directory connectivity by joining the Windows 10 client to the domain.

<img src="https://github.com/ImanKasthuri/active_directory_lab/blob/main/screenshot/Screenshot%2012.png?raw=true">

## Conclusion

This project helped me understand how a real Active Directory environment works, including DNS, DHCP, NAT, user management, and domain joining. I also gained hands-on experience troubleshooting network issues. Overall, this lab strengthened my foundational IT and cybersecurity skills.

## Credits

This Active Directory lab project was inspired by Josh Madakor’s “Active Directory Home Lab” YouTube tutorial.  
All configuration, troubleshooting, and documentation in this repository were done by me.






