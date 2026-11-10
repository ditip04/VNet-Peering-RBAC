# Azure VNet Peering & Custom RBAC Implementation

## 📌 Project Overview

This project demonstrates the implementation of **Azure Virtual Network (VNet) Peering and Role-Based Access Control (RBAC)** using Microsoft Azure.

The objective was to create a controlled cloud environment for Client to demonstrate secure network connectivity between isolated workloads, virtual machine deployment, identity onboarding, and least-privilege access management.

## 🎯 Project Objectives

- Create and configure Azure Virtual Networks and subnets.
- Deploy Azure Virtual Machines in separate virtual networks.
- Establish private connectivity using VNet Peering.
- Validate communication between workloads across virtual networks.
- Onboard a new employee into Microsoft Entra ID.
- Create a custom Azure RBAC role with specific permissions.
- Assign role permissions at resource group scope.
- Validate allowed and restricted user operations.
- Implement the principle of least privilege.

## 🏢 Business Scenario

Client is evaluating Microsoft Azure as a cloud platform for hosting its internal workloads.

As part of the evaluation, the organisation requires a test environment to demonstrate secure network communication and controlled administrative access.

The solution addresses two main requirements:

**1. Network Connectivity**
- Deploy two isolated virtual networks.
- Host a test virtual machine in each virtual network.
- Establish connectivity using VNet Peering.
- Enable private communication without requiring public internet exposure.

**2. Identity and Access Management**
- Onboard a new employee into Microsoft Entra ID.
- Create a custom RBAC role called `Computer Operator`.
- Allow the employee to view virtual machine, networking, and storage resources.
- Permit only virtual machine start and restart operations.
- Restrict access to the designated resource group.

## 🏗️ Azure Environment Setup

### Resource Group

A dedicated resource group was created to organise and manage all project resources.

| Configuration | Value |
|---|---|
| Resource Group | Rand-Test-RG |
| Region | As specified by Client |
| Purpose | Isolated test environment |

## 🌐 Network Architecture

### Virtual Network Configuration

Two virtual networks were created with non-overlapping IP address spaces to enable successful peering.

| Configuration | VNet-A | VNet-B |
|---|---|---|
| Virtual Network | VNet-A | VNet-B |
| Address Space | 10.0.0.0/16 | 10.1.0.0/16 |
| Subnet | Subnet-A | Subnet-B |
| Subnet Address | 10.0.1.0/24 | 10.1.1.0/24 |
| Virtual Machine | VM-A | VM-B |

## 💻 Workload Deployment

### Virtual Machine Configuration

Two test virtual machines were deployed, one in each virtual network, to validate workload connectivity.

| Configuration | VM-A | VM-B |
|---|---|---|
| VM Name | VM-A | VM-B |
| Virtual Network | VNet-A | VNet-B |
| Operating System | Windows Server Datacenter (Gen2) | Windows Server Datacenter (Gen2) |
| VM Size | Standard_DS1_v2 | Standard_DS1_v2 |
| OS Disk | 127 GiB | 127 GiB |
| Security Type | Standard | Standard |

Additional deployment configurations:
- Standard HDD or Premium SSD depending on subscription policy.
- Local administrator credentials configured for testing.
- Each VM deployed into its corresponding virtual network and subnet.

## 🔗 Network Connectivity Configuration

### VNet Peering

Bidirectional VNet Peering was configured between VNet-A and VNet-B.

**Peering Connections**

| Peering Name | Source VNet | Remote VNet |
|---|---|---|
| A-to-B | VNet-A | VNet-B |
| B-to-A | VNet-B | VNet-A |

**Key Configuration:**
- Allow Virtual Network Access: Enabled.
- Peering established in both directions.
- Non-overlapping address spaces configured.
- Private IP communication enabled between workloads.

### Connectivity Validation

Connectivity between the virtual machines was validated using:

1. Private IP address ping tests.
2. RDP connectivity testing.
3. Verification of automatically updated network routes.
4. Validation of communication between both virtual networks.

Successful connectivity confirmed that workloads deployed in separate VNets could communicate over the Azure network.

## 👤 Identity and User Onboarding

### Microsoft Entra ID User Creation

A new employee account was created in the default Microsoft Entra ID tenant to test the custom RBAC configuration.

| Configuration | Value |
|---|---|
| Username | operator@client.com |
| User Type | Member |
| Password | Temporary password |
| First Login | Password reset required |

The newly created identity was used to validate the permissions assigned through Azure RBAC.

## 🔐 Custom Azure RBAC Role Creation

### Role Overview

A custom role named **Computer Operator** was created to provide limited operational access to the newly onboarded employee.

The role follows the **Principle of Least Privilege**, granting only the permissions necessary to perform the required tasks.

### Role Permissions

**Read Permissions**

| Permission | Description |
|---|---|
| Microsoft.Compute/*/read | View virtual machine-related resources |
| Microsoft.Network/*/read | View networking resources |
| Microsoft.Storage/*/read | View storage resources |

**Operational Permissions**

| Permission | Description |
|---|---|
| Microsoft.Compute/virtualMachines/start/action | Start virtual machines |
| Microsoft.Compute/virtualMachines/restart/action | Restart virtual machines |

No additional write, delete, or administrative permissions were granted.

### Role Scope

The custom role was assigned at the resource group level.

| Configuration | Value |
|---|---|
| Custom Role | Computer Operator |
| Scope | Rand-Test-RG |
| Permission Model | Least Privilege |
| Access Level | Resource Group |

This scope ensures that the operator's permissions are limited to the designated resource group rather than the entire Azure subscription.

## 📝 Role Assignment

The newly onboarded employee was assigned the custom `Computer Operator` role.

| Configuration | Value |
|---|---|
| Assignee | operator@client.com |
| Role | Computer Operator |
| Assignment Scope | Rand-Test-RG |
| Platform | Microsoft Azure |

This provided the employee with the required permissions to view resources and perform specific virtual machine operations.

## ✅ Validation and Testing

### Operator Access Validation

The custom RBAC role was tested by logging into the Azure Portal using the newly created operator account.

**Allowed Actions**

- View virtual machines.
- Start virtual machines.
- Restart virtual machines.
- View virtual networks.
- View network interfaces and IP configurations.
- View storage accounts.

**Restricted Actions**

- Stop virtual machines.
- Delete virtual machines.
- Modify virtual networks.
- Create new Azure resources.
- Access resources outside the assigned resource group.
- Modify IAM and RBAC settings.

The validation demonstrated that the operator could perform the permitted activities while restricted operations remained unavailable.

## 🔍 Key Implementation Outcomes

The project successfully demonstrated the following Azure capabilities:

| Area | Implementation |
|---|---|
| Resource Management | Created and managed Azure Resource Groups |
| Network Infrastructure | Deployed virtual networks and subnets |
| Workload Deployment | Provisioned Azure Virtual Machines |
| Network Connectivity | Configured bidirectional VNet Peering |
| Identity Management | Onboarded users using Microsoft Entra ID |
| Access Management | Created a custom Azure RBAC role |
| Least Privilege | Restricted permissions to required operations |
| Access Validation | Tested permitted and restricted user actions |

## 🧠 Technical Skills Demonstrated

- Microsoft Azure IaaS
- Azure Resource Groups
- Azure Virtual Networks (VNet)
- VNet Peering
- Azure Virtual Machines
- IP Addressing and Subnetting
- Microsoft Entra ID
- Azure Role-Based Access Control (RBAC)
- Custom Role Definitions
- Role Assignments and Sc
