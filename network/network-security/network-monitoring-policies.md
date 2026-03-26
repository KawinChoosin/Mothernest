---
icon: chart-line
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

# Network Monitoring Policies

**This page defines how traffic, bandwidth, devices, and threats are monitored across the organization.**

The organization monitors network traffic, bandwidth usage, connected devices, and security events across both sites.

### Traffic monitoring

* Monitor all traffic across the headquarters and branch networks.
* Log all inbound and outbound connections.
* Retain logs for at least 90 days.

### Bandwidth monitoring

* Monitor bandwidth usage per user and device.
* Alert on unusual data transfers above **500 MB per hour**.
* Apply QoS to prioritize business-critical traffic.

### Device management

* Track all connected devices, including MAC address, IP address, and hostname.
* Alert on unauthorized or unknown devices.
* Update the device inventory every month.

### Security threat detection

* Monitor for suspicious behavior, including port scanning and DDoS attempts.
* Implement an intrusion detection system.
* Alert the security team immediately when threats are detected.
