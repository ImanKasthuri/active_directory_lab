# Active_Directory_Home_Lab

I built an Active Directory environment using VMware. This lab uses two virtual machines:
- Windows Server 2019 - Domain Controller
- Windows 10 Client
Both machines are connected using VMware network adapters, and this shows how a real internal corporate network works.

## Network Adapter Configuration

I created two network adapters:
- NAT Adapter - This one connects to our internet, assigns an IP Address automatically.
- LAN Segment - This one is used to connect the internal network, especially connecting with Windows 10 clients.

## Assighn Static IP address
I assigned the static IP address to the internal network on the Domain Controller, so the server always has the same IP address and the client can reliably find it.
- IP Address - 172.16.0.1
- Subnet Mask - 255.255.255.0

## Installed Active Directory Domain Service
I installed Active Directory Domain Service, which can manage users, computers, and group policies within the domain. Right after installation, I did the Post Deployment Configuration.
- Root Domain Name - mydomain.com

## Creating an Organizational Unit
I created an Organizational Unit by opening Active Directory Users & Computers
- Organizational Unit - Admins
- Under the Admins - Iman Kasthuri
I gave Admin power to the user Iman Kasthuri. Admins have full control over the whole network; they can add new users, delete users, etc.

## Adding RAS/NAT TO the Domain Controller
I installed the Remote Access role on the Domain Controller, so the Client Windows can access the internet through the Server.

## Adding DHCP Server to the Domain Controller
Added a DHCP Server, so the Windows 10 Client automatically assigns an IP address, DNS, and Gateway without manually setting it.
- DHCP Range - 172.16.0.100/200

We set a DHCP range because the server must know which DHCP IP address it can use for the clients. Windows 10 clients get an IP address from this range, like 172.16.0.100/200

## Creating Users
I used a PowerShell script to automatically add a larger number of users (1000 users) for my lab environment. The purpose of this script is to simulate a large company with many users. For further reference, I added my script file separately.

