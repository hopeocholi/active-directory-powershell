# Active Directory Home Lab

A comprehensive Windows Active Directory environment built with virtualization technology to simulate enterprise network infrastructure.

## Overview

This project implements a fully functional Active Directory domain using Windows Server 2019 and Windows 10 clients in Oracle VirtualBox. The lab demonstrates practical skills in Windows server administration, network configuration, and identity management.

## Architecture

- **Domain Controller**: Windows Server 2019 with dual network interfaces
  - External NIC: Internet connectivity via NAT
  - Internal NIC: Private network for client connections (172.16.0.1/24)
- **Client Machine**: Windows 10 connected to internal network
- **Domain**: mydomain.com

## Features

- Active Directory Domain Services configuration
- DNS service for domain name resolution
- DHCP server for automatic IP assignment (172.16.0.100-200)
- NAT routing for client internet access
- PowerShell automation for bulk user creation

## Implementation Steps

1. **Base Infrastructure Setup**
   - Oracle VirtualBox installation with Extension Pack
   - Windows Server 2019 and Windows 10 ISO preparation

2. **Domain Controller Configuration**
   - Dual-NIC setup (External/Internal networks)
   - IP addressing configuration
   - Active Directory Domain Services installation
   - Domain creation (mydomain.com)

3. **Network Services Deployment**
   - Routing and Remote Access Service (RRAS) with NAT
   - DHCP server with appropriate scope
   - DNS integrated with Active Directory

4. **User Management Automation**
   - PowerShell script development for bulk user creation
   - Creation of administrative and standard user accounts

5. **Client Integration**
   - Windows 10 client deployment
   - Domain joining and verification
   - User authentication testing

## PowerShell Automation

The project includes a PowerShell script that creates 1,000+ test user accounts from a list of names. The script demonstrates:

```powershell
# Sample from the user creation script
$UserFirstLastList = Get-Content .\names.txt
foreach ($n in $UserFirstLastList) {
    $first = ($n.Split(" ")[0]).ToLower()
    $last = ($n.Split(" ")[1]).ToLower()
    $username = $first.Substring(0,1) + $last
    
    Write-Host "Creating user: $username" -ForegroundColor Cyan
    New-ADUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "OU=_Users,DC=mydomain,DC=com" `
               -Enabled $true
}
