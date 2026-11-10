Azure VNet Peering & Custom RBAC 
1. Introduction
Client is evaluating Microsoft Azure as a cloud platform for hosting internal workloads. To support this assessment, a controlled test environment was deployed to demonstrate Azure’s networking capabilities, workload connectivity, identity management, and role-based access control (RBAC). This report documents the full implementation process, including virtual network creation, workload deployment, VNet peering, user onboarding, and custom RBAC role assignment.
________________________________________
2. Environment Setup
2.1 Resource Group
A dedicated resource group was created to contain all test resources:
•	Resource Group Name: Rand-Test-RG
•	Region: As specified by Client
This ensures clean separation from production workloads and simplifies access control.
________________________________________
3. Network Architecture
3.1 Virtual Networks
Two virtual networks were created to simulate isolated workload environments:
VNet	Address Space	Subnet	Purpose
VNet-A	10.0.0.0/16	Subnet-A (10.0.1.0/24)	Hosts VM-A
VNet-B	10.1.0.0/16	Subnet-B (10.1.1.0/24)	Hosts VM-B
Non-overlapping IP ranges were used to ensure compatibility with VNet peering.
________________________________________
4. Workload Deployment
4.1 Virtual Machines
Two test virtual machines were deployed—one in each VNet—to validate connectivity:
VM Name	VNet	OS Image	Size
VM-A	VNet-A	Windows Server Datacenter (Gen2)	Standard_DS1_v2
VM-B	VNet-B	Windows Server Datacenter (Gen2)	Standard_DS1_v2
Each VM was deployed with:
•	Standard security type
•	Default OS disk size (127 GiB)
•	Standard HDD or Premium SSD depending on subscription policy
•	Local admin credentials for testing
________________________________________
5. Network Connectivity
5.1 VNet Peering
Bidirectional VNet peering was configured between VNet-A and VNet-B:
•	A-to-B Peering
•	B-to-A Peering
Peering enables:
•	Private IP communication
•	Low-latency, high-bandwidth traffic
•	No public internet exposure
5.2 Connectivity Validation
Connectivity was verified by:
•	Pinging private IPs between VMs
•	Testing RDP/SSH access
•	Confirming routing tables updated automatically
This validated that workloads in separate VNets can communicate securely.
________________________________________
6. Identity Onboarding
6.1 Employee Creation in Azure AD
A new user was onboarded into the default Microsoft Entra ID tenant:
•	Username: operator@client.com
•	User Type: Member
•	Password: Temporary password assigned
•	Security: Password reset required on first login
This identity will be used to test RBAC permissions.
________________________________________
7. Custom RBAC Role Creation
7.1 Purpose
Client required a role that allows an operator to:
•	View VM, network, and storage resources
•	Start and restart virtual machines
•	Nothing more
This aligns with the principle of least privilege.
7.2 Role Definition
A custom role named Computer Operator was created with the following permissions:
Read Permissions
•	Microsoft.Compute/*/read
•	Microsoft.Network/*/read
•	Microsoft.Storage/*/read
Operational Permissions
•	Microsoft.Compute/virtualMachines/start/action
•	Microsoft.Compute/virtualMachines/restart/action
7.3 Scope
The role was scoped only to the resource group Rand-Test-RG. This ensures:
•	No subscription-wide access
•	No access to other resource groups
•	Strict isolation of operator capabilities
________________________________________
8. Role Assignment
The onboarded user was assigned the Computer Operator role at the resource group level:
•	Resource Group: Rand-Test-RG
•	Role: Computer Operator
•	Assignee: operator@client.com
This grants the user access only to the resources required for VM operations.
________________________________________
9. Validation
9.1 Operator Login
The operator logged into the Azure Portal and validated:
Allowed Actions
•	View VMs
•	Start VMs
•	Restart VMs
•	View VNets, NICs, IPs
•	View storage accounts
Blocked Actions
•	Stop VMs
•	Delete VMs
•	Modify VNets
•	Create new resources
•	Access other resource groups
•	Modify IAM settings
This confirms the custom RBAC role is correctly implemented.
________________________________________
10. Conclusion
This project successfully demonstrated Azure’s capabilities in:
•	Secure network segmentation
•	Controlled workload deployment
•	Private VNet-to-VNet communication
•	Identity onboarding
•	Least-privilege access control
Client now has a validated reference environment showing how Azure can support secure operations, controlled access, and scalable workload connectivity.

