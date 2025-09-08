# Demo Company Network Lab

This repository documents a **virtual demo company network** built with Hyper-V to simulate a small corporate IT environment.  
It includes **Active Directory (AD), DNS, SQL Server, and Windows clients**, designed as a learning project for administration, networking, and database basics.

---

## 🎯 Goals
- Practice Windows Server administration (Active Directory, DNS, DHCP).  
- Simulate a small company environment with realistic users, groups, and OUs.  
- Install and manage SQL Server in a domain environment.  
- Test T-SQL queries from domain-joined clients.  
- Document the setup for reproducibility and portfolio showcase.

---

## 🏗️ Lab Architecture & AD Hierarchy

|Internal Network 10.0.0.0/24 (Hyper-V NAT)     |
|--------------|---------------|----------------|
| lab-dc01     | lab-sql01     | lab-clt01      |
| DC + DNS     | SQL Server    | Win11 Client   |
| IP: 10.0.0.1 | IP: 10.0.0.10 | IP: 10.0.0.100 |


lab.local (Domain)
\|  
\+-- Employees  
\| \|  
\| \+-- IT  
\| \| \+-- a.weber (SQL\_Admins)  
\| \| \+-- e.fischer  
\| \|  
\| \+-- Finance  
\| \| \+-- b.schmidt (SQL\_Admins)  
\| \| \+-- f.klein  
\| \|  
\| \+-- HR  
\| \| \+-- c.müller  
\| \|  
\| \+-- Sales  
\| \+-- d.meier  
\|  
\+-- Groups  
\| \|  
\| \+-- SQL\_Admins  
\| \| \+-- a.weber  
\| \| \+-- b.schmidt  
\| \|  
\| \+-- Helpdesk\_Team  
\| \+-- VPN\_Users  
\| \+-- Finance\_Group  
\|  
\+-- Clients  
\+-- Servers  
\+-- Service Accounts

> 🔹 All VMs are connected to the **same internal NAT network** (10.0.0.0/24).  
> 🔹 Users are organized into OUs and groups for permissions.  
> 🔹 SQL_Admins members have full sysadmin access to SQL Server.

---

## 💻 VM & IP Overview

| VM Name    | Role                        | IP Address   | Notes                        |
|------------|-----------------------------|-------------|------------------------------|
| lab-dc01   | Domain Controller + DNS      | 10.0.0.1    | Forest: `lab.local`          |
| lab-sql01  | SQL Server 2022 Developer    | 10.0.0.10   | TCP/IP enabled, port 1433    |
| lab-clt01  | Windows 11 Client            | 10.0.0.100  | Domain-joined, SSMS client   |

---

## 👤 Users & Groups

| Username  | Full Name       | OU               | AD Groups            | SQL Server Role     |
|-----------|----------------|-----------------|--------------------|------------------|
| a.weber   | Alice Weber    | Employees\IT    | SQL_Admins          | sysadmin         |
| e.fischer | Eva Fischer    | Employees\IT    | —                  | read-only / none |
| b.schmidt | Bob Schmidt    | Employees\Finance | SQL_Admins        | sysadmin         |
| f.klein   | Frank Klein    | Employees\Finance | —                | read-only / none |
| c.müller  | Carol Müller   | Employees\HR    | —                  | read-only / none |
| d.meier   | Daniel Meier   | Employees\Sales | —                  | read-only / none |

> 🔹 Membership in **SQL_Admins** grants full sysadmin access to SQL Server.  
> 🔹 Users not in SQL_Admins have no default access to SQL Server.  

---

## ⚙️ Setup Overview
1. **Created domain**: `lab.local` on `lab-dc01`.  
2. **Configured DNS** with forwarders to public DNS (1.1.1.1).  
3. **Joined Windows 11 client** to the domain.  
4. **Installed SQL Server Developer Edition** on `lab-sql01`.  
5. **Configured firewall & TCP/IP (1433)** for remote connections.  
6. **Created AD groups & users** to simulate departments.  
7. **Tested SQL logins** via SSMS from client machine.  

---

## 🌐 NAT & Virtual NIC Setup
- **Hyper-V Virtual Switch**: Created an **Internal NAT switch** for lab VMs.  
- **Virtual NIC on Host**: Assigned a static IP in the 10.0.0.0/24 network to the host’s virtual adapter to allow the host to communicate with lab VMs.  
- **NAT Configuration**: Enabled NAT on the Hyper-V virtual switch to allow lab VMs to access the Internet while staying isolated from the home network.  
- **Result**:  
  - VMs have internal IPs like 10.0.0.x.
  - VE Hosts vNIC IP is 10.0.0.254
  - DC, SQL Server, and Client are on the same subnet.  
  - Internet is reachable via NAT without exposing the lab directly to the home LAN.

---

## 📚 Next Steps
- Adding AdventureWorks.db to the SQL Server to train more complex and realistic SQL Queries.
- Add **DHCP server** for automatic IP assignment.  
- Deploy **file shares** with NTFS and share permissions.  
- Experiment with **Group Policy Objects (GPOs)**.  
- Expand network with more servers (IIS/Web, Exchange demo, etc.).  

---

## 📄 License
This project is for **learning and demonstration purposes only**.  
All Microsoft products used are in **evaluation / developer editions**.  
