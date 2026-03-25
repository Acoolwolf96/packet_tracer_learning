# Small Business Network Setup

## Project Overview
This project simulates a **complete small business network** with multiple departments, VLAN separation, and inter-department routing. The network supports 8 employees across 4 departments with secure traffic isolation and full connectivity.

### Business Scenario
A growing company with 8 employees needs a network that:
- Separates traffic between departments for security
- Allows departments to communicate when needed
- Uses a single internet connection (router) for all departments
- Is scalable for future growth

### Departments
| Department | Employees | VLAN |
|------------|-----------|------|
| Sales | 2 | 10 |
| Engineering | 2 | 20 |
| Human Resources (HR) | 2 | 30 |
| IT | 2 | 40 |

---

## Network Topology

### Devices Used
| Device Type | Model | Quantity | Purpose |
|-------------|-------|----------|---------|
| Router | Cisco 2911 | 1 | Inter-VLAN routing, gateway to internet |
| Core Switch | Cisco 2960-24TT | 1 | Central distribution, VLAN trunking |
| Department Switches | Cisco 2960-24TT | 4 | Access layer for each department |
| Workstations | Generic PC-PT | 8 | Employee workstations |

### Physical Layout
┌─────────────────────────────────────┐
│ Router (Gateway) │
│ Routes between all departments │
│ Gig0/0.10 - Sales Gateway │
│ Gig0/0.20 - Engineering Gateway │
│ Gig0/0.30 - HR Gateway │
│ Gig0/0.40 - IT Gateway │
└─────────────────┬───────────────────┘
│ Trunk (802.1Q)
┌─────────────────┴───────────────────┐
│ Core Switch │
│ Distributes VLANs to departments │
└─────┬─────┬─────┬───────────────────┘
│ │ │
┌─────────────┼─────┼─────┼─────────────┐
│ │ │ │ │
┌─────┴─────┐ ┌─────┴─────┐ ┌─────┴─────┐ ┌─────┴─────┐
│ Sales │ │Engineering│ │ HR │ │ IT │
│ Switch │ │ Switch │ │ Switch │ │ Switch │
│ VLAN 10 │ │ VLAN 20 │ │ VLAN 30 │ │ VLAN 40 │
└─────┬─────┘ └─────┬─────┘ └─────┬─────┘ └─────┬─────┘
│ │ │ │
┌─────┴─────┐ ┌─────┴─────┐ ┌─────┴─────┐ ┌─────┴─────┐
│ PC0 PC1 │ │ PC2 PC3 │ │ PC4 PC5 │ │ PC6 PC7 │
│ Sales │ │ Engineering│ │ HR │ │ IT │
└───────────┘ └───────────┘ └───────────┘ └───────────┘


---

## IP Addressing Scheme

### Subnet Design
- **Original Network:** 192.168.10.0/24
- **Subnet Mask:** 255.255.255.192 (/26)
- **Subnets Created:** 4
- **Hosts per Subnet:** 62 (plenty for future growth)

### Department IP Assignments

| Department | VLAN | Network | Gateway | PC IPs | Host Range |
|------------|------|---------|---------|--------|------------|
| Sales | 10 | 192.168.10.0/26 | 192.168.10.1 | .10, .11 | .1 - .62 |
| Engineering | 20 | 192.168.10.64/26 | 192.168.10.65 | .70, .71 | .65 - .126 |
| HR | 30 | 192.168.10.128/26 | 192.168.10.129 | .130, .131 | .129 - .190 |
| IT | 40 | 192.168.10.192/26 | 192.168.10.193 | .200, .201 | .193 - .254 |

### Why This IP Design?
- **/26 subnet mask** provides 62 usable IPs per department
- Room to add up to 60 more devices per department
- Easy to identify departments by IP range
- Conservative use of IP space

---

## Network Configuration

### Router Configuration (Router-on-a-Stick)
The router uses subinterfaces to route between VLANs:
interface g0/0.10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.192
!
interface g0/0.20
encapsulation dot1q 20
ip address 192.168.10.65 255.255.255.192
!
interface g0/0.30
encapsulation dot1q 30
ip address 192.168.10.129 255.255.255.192
!
interface g0/0.40
encapsulation dot1q 40
ip address 192.168.10.193 255.255.255.192
!
interface g0/0
no shutdown


### Core Switch Configuration
- Creates VLANs 10, 20, 30, 40
- Trunk to router carries all VLANs
- Trunk to each department switch carries only its VLAN

### Department Switch Configuration
- Trunk uplink to core switch
- Access ports for employee workstations

---

## Security Features

| Feature | Implementation |
|---------|----------------|
| **VLAN Isolation** | Each department in separate VLAN, cannot see each other's broadcast traffic |
| **Inter-VLAN Routing** | Controlled through router, allows future ACL implementation |
| **Access Ports** | PCs cannot trunk or tag their own VLANs |
| **Trunk Restriction** | Each department trunk only allows its own VLAN |



## Verification Results

### Connectivity Tests
| Test | Source | Destination | Result |
|------|--------|-------------|--------|
| Same department | PC0 (Sales) | PC1 (Sales)
| Cross department | PC0 (Sales) | PC2 (Engineering) 
| Cross department | PC0 (Sales) | PC4 (HR) 
| Cross department | PC0 (Sales) | PC6 (IT) 
| Gateway connectivity | PC0 | 192.168.10.1


