### 1. Infrastructure as Code (Terraform)

use **Terraform** as the primary orchestration tool to provision the underlying virtual hardware. 

- **Provider Management:** Terraform interacts with cloud providers (AWS, Azure, or GCP) or private hypervisors (Proxmox/VMware) to create VPCs, subnets, and VM instances.
    
- **Scalability:** For individual "spawnable" machines, Terraform allows them to create a fresh, isolated instance for a user and tear it down immediately after use.
    

### 2. Configuration Management (Ansible)

Once the "blank" VMs are created by Terraform, **Ansible** is used to turn them into functional Active Directory environments.

- **Domain Setup:** Ansible playbooks automate the installation of the AD DS (Active Directory Domain Services) role, creation of Forest/Domain structures, and joining workstations to the domain.
    
- **Vulnerability Seeding:** This is the most critical part. Ansible scripts are used to create "misconfigurations" (e.g., GPP passwords in SYSVOL, Kerberoasting service accounts, or overly permissive ACLs) that make the lab a "puzzle."
    
- **Software Deployment:** It installs specific vulnerable versions of software, web servers, or databases required for the exploit path.