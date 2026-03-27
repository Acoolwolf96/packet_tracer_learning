# Multi-Site Small Business Network

## Project Overview
This project simulates a **multi-site business network** with headquarters and a branch office connected via a WAN link. The network supports 12 employees across 6 departments with VLAN isolation and inter-site routing.

### Business Scenario
A growing company with two locations needs:
- Separate VLANs for each department
- Inter-VLAN routing within each site
- WAN connectivity between HQ and Branch
- Cross-site communication for collaboration
- Scalable design for future growth

### Locations
| Site | Departments | Employees | VLANs |
|------|-------------|-----------|-------|
| Headquarters | Sales, Engineering, HR, IT | 8 | 4 |
| Branch Office | Sales, Support | 4 | 2 |

---

## Network Topology

### Devices Used
| Device Type | Model | Quantity | Location |
|-------------|-------|----------|----------|
| Router | Cisco 2911 | 2 | HQ, Branch |
| Core Switch | Cisco 2960-24TT | 2 | HQ, Branch |
| Department Switches | Cisco 2960-24TT | 6 | Both sites |
| Workstations | Generic PC-PT | 12 | Both sites |


---

## IP Addressing Scheme

### Headquarters (HQ)
| Department | VLAN | Network | Subnet Mask | Gateway | PCs | IP Range |
|------------|------|---------|-------------|---------|-----|----------|
| Sales | 10 | 192.168.10.0/26 | 255.255.255.192 | 192.168.10.1 | PC0, PC1 | .10, .11 |
| Engineering | 20 | 192.168.10.64/26 | 255.255.255.192 | 192.168.10.65 | PC2, PC3 | .70, .71 |
| HR | 30 | 192.168.10.128/26 | 255.255.255.192 | 192.168.10.129 | PC4, PC5 | .130, .131 |
| IT | 40 | 192.168.10.192/26 | 255.255.255.192 | 192.168.10.193 | PC6, PC7 | .200, .201 |

### Branch Office
| Department | VLAN | Network | Subnet Mask | Gateway | PCs | IP Range |
|------------|------|---------|-------------|---------|-----|----------|
| Sales | 10 | 192.168.20.0/26 | 255.255.255.192 | 192.168.20.1 | PC8, PC9 | .10, .11 |
| Support | 20 | 192.168.20.64/26 | 255.255.255.192 | 192.168.20.65 | PC10, PC11 | .70, .71 |

### WAN Link
| Connection | Network | Subnet Mask | HQ IP | Branch IP |
|------------|---------|-------------|-------|-----------|
| HQ ↔ Branch | 10.10.10.0/30 | 255.255.255.252 | 10.10.10.1 | 10.10.10.2 |

---

## Key Technologies Used

| Technology | Implementation |
|------------|----------------|
| **VLANs** | 802.1Q tagging for department isolation |
| **Trunking** | Carrying multiple VLANs over single links |
| **Router-on-a-Stick** | Subinterfaces for inter-VLAN routing |
| **WAN Link** | /30 point-to-point connection |
| **Static Routing** | Manual routes for cross-site communication |
| **Private IP Addressing** | RFC 1918 compliant |

---

## Configuration Files

| File | Description |
|------|-------------|
| `hq-router-config.txt` | HQ Router subinterface and WAN configuration |
| `branch-router-config.txt` | Branch Router subinterface and WAN configuration |
| `core-switch-config.txt` | HQ Core Switch VLAN and trunk configuration |
| `branch-core-switch-config.txt` | Branch Core Switch configuration |
| `department-switch-config.txt` | Template for department switches |
| `ip-addressing-plan.md` | Detailed IP allocation documentation |
| `static-routes-summary.md` | Static route calculations |

---

## Verification Results

### Internal Connectivity Tests
| Test | Source | Destination | Result |
|------|--------|-------------|--------|
| Same VLAN (HQ) | PC0 | PC1 
| Cross VLAN (HQ) | PC0 | PC2 
| Same VLAN (Branch) | PC8 | PC9 
| Cross VLAN (Branch) | PC8 | PC10 | 

### Cross-Site Connectivity Tests
| Test | Source | Destination | Result |
|------|--------|-------------|--------|
| HQ Sales → Branch Sales | PC0 | 192.168.20.10 
| HQ Sales → Branch Support | PC0 | 192.168.20.70 
| Branch Sales → HQ Engineering | PC8 | 192.168.10.70 
| Branch Support → HQ IT | PC10 | 192.168.10.200 

### WAN Connectivity
| Test | Source | Destination | Result |
|------|--------|-------------|--------|
| Ping WAN Link (HQ) | HQ Router | 10.10.10.2 
| Ping WAN Link (Branch) | Branch Router | 10.10.10.1 

---

## Lessons Learned

1. **VLAN Planning**: Proper VLAN numbering (10,20,30,40) makes troubleshooting easier
2. **IP Addressing**: Using different third octets (10 vs 20) helps identify location by IP
3. **WAN Design**: /30 subnets are most efficient for point-to-point links
4. **Static Routes**: Must be configured on both routers for bidirectional communication
5. **Documentation**: Essential for maintaining complex networks

---


## How to Use This Project

1. Clone this repository
2. Open `multi-site-business-network.pkt` with Cisco Packet Tracer
3. Review the configuration files to understand the setup
4. Run verification tests to confirm connectivity