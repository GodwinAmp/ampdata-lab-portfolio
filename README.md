# Active Directory Home Lab: ampdata.org

A hands-on home lab where I built a Windows Server 2022 virtual machine and used it to learn Active Directory Domain Services (AD DS): domains, organizational units, users, groups, account management, sites, and replication.

**Live page:** https://godwinamp.github.io/ampdata-lab-portfolio/

## Environment and tools

| Layer | What I used | Role in the lab |
|---|---|---|
| Host computer | Windows 11 (64-bit) | The physical PC that runs everything else |
| Virtualization | Oracle VirtualBox | Runs the server as a virtual machine, isolated from my PC |
| Server VM | Windows Server 2022 | The domain controller for my domain, ampdata.org |
| Directory service | Active Directory Domain Services (AD DS) | Stores users, groups, and OUs, and handles sign-ins |

**Utilities used on the server:**

- **Server Manager:** the main control panel for installing roles and managing the server
- **Active Directory Users and Computers:** creating OUs, users, and groups, and managing accounts
- **Active Directory Sites and Services:** creating sites and learning replication
- **Active Directory Domains and Trusts:** exploring the forest and trust relationships

## Repository contents

```
ampdata-lab-portfolio/
├── README.md                  This write-up
├── index.html                 The same write-up as a web page (GitHub Pages)
├── server-manager.png         Screenshots from the lab
├── ou-structure.png
├── it-ou-and-managers.png
└── hamburgsite.png
```

## Part 1: Building the server VM

A virtual machine is a full computer that runs inside a window on my own PC. It gives me a safe place to learn: if I make a mistake, I can fix it or rebuild, and nothing outside the VM is affected.

```
My PC: Windows 11 (the physical host)
  └─ Oracle VirtualBox (runs virtual machines)
       └─ Windows Server 2022 (the VM, named "server")
```

1. **Got the installer.** I downloaded a Windows Server 2022 ISO, which is a disc image of the operating system.
2. **Created the VM in VirtualBox.** I gave it a name, pointed it to the ISO, and set aside memory, CPU, and a virtual disk from my PC.
3. **Installed Windows Server 2022.** I booted the VM from the ISO, ran the installer, and set the Administrator password.
4. **Opened Server Manager.** This is the main control panel of a Windows Server. It shows the server's roles, status, and tools in one place.

![Server Manager running in the VirtualBox VM](server-manager.png)

*My server running in VirtualBox, with Server Manager open.*

## Part 2: Active Directory Domain Services

I installed the AD DS role and promoted the server to a domain controller, which created my domain, **ampdata.org**, and made it the root of my forest.

```
ampdata.org (forest root)
├─ Finance   └─ users
├─ IT        └─ users
├─ HR        └─ users
└─ Managers (group): members from all 3 OUs

Site: HamburgSite
```

### The logical side: who and what

**Organizational units.** I created OUs for Finance, IT, and HR so accounts are organized the way a real company would structure them.

![Active Directory Users and Computers showing the Finance, HR, and IT OUs](ou-structure.png)

*Active Directory Users and Computers with my Finance, HR, and IT OUs.*

**Users and groups.** I created users inside each OU, then made a security group called **Managers** and added users from different OUs to it.

> **Why groups matter:** permissions given to a group are easier to scale and audit than permissions given to each person one by one.

![The IT OU with its users and the Managers security group](it-ou-and-managers.png)

*The IT OU, with its users and the Managers security group. User names are hidden.*

### Managing user accounts

Day-to-day admin work happens in Active Directory Users and Computers. Right-clicking a user lets me:

- **Reset the password**, and require the user to change it at next sign-in.
- **Disable an account** when someone leaves or should not have access, and enable it again later.
- **Unlock an account** that was locked after too many failed sign-in attempts.

### The physical side: where and how

**Sites.** I created a site named **HamburgSite** in Active Directory Sites and Services. Sites describe the real network layout, and AD uses them to decide where clients sign in and how domain controllers sync.

![Active Directory Sites and Services showing HamburgSite](hamburgsite.png)

*HamburgSite next to the default site in Sites and Services.*

**Replication.** I am learning the two kinds:

- **Intra-site** replication happens between domain controllers in the same site. It is fast and automatic.
- **Inter-site** replication happens between sites over site links. It runs on a schedule to save bandwidth.

### Domains and trusts

A **forest** is the top-level container that holds one or more domains. My first domain is the forest root. In the Active Directory Domains and Trusts console, you can create **trust relationships** so users in one domain or forest can use resources in another. This is common when two organizations work together.

## Skills demonstrated

- Setting up a virtual lab with VirtualBox
- Installing and navigating Windows Server 2022
- Creating and promoting a domain controller
- Designing OUs, users, and security groups
- Resetting passwords and disabling accounts
- Creating AD sites and understanding replication

## What I learned

- A VM lets me run a full server on a normal PC and experiment without risk.
- The logical structure (domain, OUs, groups) and the physical structure (sites, replication) are separate ideas.
- Organize with OUs, assign access with groups.

## Up next

- [ ] Take a VM snapshot so I can roll back after risky changes
- [ ] Group Policy Objects (GPOs)
- [ ] Add a second domain controller and watch replication work
- [ ] File share permissions for the Managers group
