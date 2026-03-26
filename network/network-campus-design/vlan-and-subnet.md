---
icon: network-wired
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: false
  tags:
    visible: false
tags:
  - network
---

# VLAN and Subnet

This page defines the VLAN IDs, subnet ranges, and device groups for the headquarters and branch locations.

### Headquarters VLAN and subnet

The headquarters design supports the main office network and shared infrastructure services.

#### Planning assumptions

* Supports 50 to 60 employees
* Includes guest access

#### VLAN and subnet plan

| VLAN | Subnet           | User (Devices)     |
| ---- | ---------------- | ------------------ |
| 2    | 192.168.2.0/29   | IT (5)             |
| 3    | 192.168.3.0/29   | NOC/SOC (5)        |
| 4    | 192.168.4.0/26   | CCTV (60)          |
| 5    | 192.168.5.0/27   | DEV (28)           |
| 6    | 192.168.6.0/27   | Power Control (28) |
| 8    | 192.168.8.0/26   | MDF IDF (60)       |
| 9    | 192.168.9.0/26   | Data Center (60)   |
| 10   | 192.168.10.0/27  | Large Meeting (28) |
| 11   | 192.168.11.0/28  | Small Meeting (10) |
| 12   | 192.168.12.0/29  | HR (5)             |
| 13   | 192.168.13.0/29  | Marketing (5)      |
| 14   | 192.168.14.0/29  | Finance (5)        |
| 15   | 192.168.15.0/29  | Reception (4)      |
| 16   | 192.168.16.0/27  | DMZ (28)           |
| 99   | 192.168.99.0/28  | Management (10)    |
| 100  | 192.168.100.0/25 | User (124)         |
| 101  | 192.168.101.0/25 | Employee (124)     |

### Branch VLAN and subnet

The branch design supports day-to-day operations and local network services at the branch site.

#### Planning assumptions

* Supports 10 to 20 employees
* Includes guest access

#### VLAN and subnet plan

| VLAN | Subnet          | User (Devices)            |
| ---- | --------------- | ------------------------- |
| 17   | 192.168.17.0/28 | Meeting Room (10)         |
| 19   | 192.168.19.0/29 | Reception (2)             |
| 20   | 192.168.20.0/28 | Network & Mini server (6) |
| 21   | 192.168.21.0/28 | Marketing Room (10)       |
| 22   | 192.168.22.0/30 | IT Support Room (1)       |
| 23   | 192.168.23.0/27 | CCTV (28)                 |
| 24   | 192.168.24.0/26 | Guest (60)                |
| 25   | 192.168.25.0/26 | Employee (60)             |
