# IP Addressing Plan

## Overview
This document outlines the IP addressing scheme for the multi-site business network.

## Addressing Philosophy
- **192.168.10.x** - Headquarters
- **192.168.20.x** - Branch Office
- **10.10.10.x** - WAN Links
- **/26 subnet mask** - Consistent across all departments

## Headquarters (HQ)

### Network: 192.168.10.0/24

| VLAN | Name | Network | Mask | Gateway | Broadcast | Host Range |
|------|------|---------|------|---------|----------|------------|
| 10 | Sales | 192.168.10.0/26 | 255.255.255.192 | .1 | .63 | .1 - .62 |
| 20 | Engineering | 192.168.10.64/26 | 255.255.255.192 | .65 | .127 | .65 - .126 |
| 30 | HR | 192.168.10.128/26 | 255.255.255.192 | .129 | .191 | .129 - .190 |
| 40 | IT | 192.168.10.192/26 | 255.255.255.192 | .193 | .255 | .193 - .254 |

### Host Assignments
| Device | VLAN | IP Address | Purpose |
|--------|------|------------|---------|
| PC0 | 10 | 192.168.10.10 | Sales Employee |
| PC1 | 10 | 192.168.10.11 | Sales Employee |
| PC2 | 20 | 192.168.10.70 | Engineering Employee |
| PC3 | 20 | 192.168.10.71 | Engineering Employee |
| PC4 | 30 | 192.168.10.130 | HR Employee |
| PC5 | 30 | 192.168.10.131 | HR Employee |
| PC6 | 40 | 192.168.10.200 | IT Employee |
| PC7 | 40 | 192.168.10.201 | IT Employee |

### Reserved Addresses
| Range | Purpose |
|-------|---------|
| .1 | Default Gateway |
| .2 - .9 | Network Devices |
| .63, .127, .191, .255 | Broadcast Addresses |

---

## Branch Office

### Network: 192.168.20.0/24

| VLAN | Name | Network | Mask | Gateway | Broadcast | Host Range |
|------|------|---------|------|---------|----------|------------|
| 10 | Sales | 192.168.20.0/26 | 255.255.255.192 | .1 | .63 | .1 - .62 |
| 20 | Support | 192.168.20.64/26 | 255.255.255.192 | .65 | .127 | .65 - .126 |

### Host Assignments
| Device | VLAN | IP Address | Purpose |
|--------|------|------------|---------|
| PC8 | 10 | 192.168.20.10 | Sales Employee |
| PC9 | 10 | 192.168.20.11 | Sales Employee |
| PC10 | 20 | 192.168.20.70 | Support Employee |
| PC11 | 20 | 192.168.20.71 | Support Employee |

### Reserved Addresses
| Range | Purpose |
|-------|---------|
| .1 | Default Gateway |
| .2 - .9 | Network Devices |
| .63, .127 | Broadcast Addresses |

---

## WAN Link

| Connection | Network | Mask | HQ IP | Branch IP | Broadcast |
|------------|---------|------|-------|-----------|-----------|
| HQ ↔ Branch | 10.10.10.0/30 | 255.255.255.252 | 10.10.10.1 | 10.10.10.2 | 10.10.10.3 |

---

## Subnet Calculations

### HQ Subnets (/26)
Magic Number: 256 - 192 = 64
Subnets: 0, 64, 128, 192

Sales: 192.168.10.0/26
Engineering: 192.168.10.64/26
HR: 192.168.10.128/26
IT: 192.168.10.192/26


### Branch Subnets (/26)
Magic Number: 256 - 192 = 64
Subnets: 0, 64, 128, 192

Sales: 192.168.20.0/26
Support: 192.168.20.64/26
(Reserved: 192.168.20.128/26, 192.168.20.192/26)


### WAN Link (/30)
Magic Number: 256 - 252 = 4
Subnets: 0, 4, 8, 12, ...

WAN: 10.10.10.0/30
Usable IPs: .1 and .2


---

## IP Allocation Summary

| Site | Network Range | Used Subnets | Free Subnets |
|------|---------------|--------------|--------------|
| HQ | 192.168.10.0/24 | 4 | 0 |
| Branch | 192.168.20.0/24 | 2 | 2 |
| WAN | 10.10.10.0/30 | 1 | Many |

---

## Growth Planning

### Future Subnets Available
- **Branch**: 2 additional /26 subnets (192.168.20.128/26, 192.168.20.192/26)
- **WAN**: Additional /30 subnets (10.10.10.4/30, 10.10.10.8/30, etc.)
- **New Sites**: Can use 192.168.30.0/24, 192.168.40.0/24, etc.