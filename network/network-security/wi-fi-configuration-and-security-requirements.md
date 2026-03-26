---
icon: wifi
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

# Wi-Fi Configuration and Security Requirements

**This page defines the Wi-Fi network design and security requirements for employees and guests.**

The network uses separate SSIDs for employees and guests to improve security and management.

| Network Name      | Purpose                                        | Security Level                               |
| ----------------- | ---------------------------------------------- | -------------------------------------------- |
| COMPANY\_EMPLOYEE | Staff only with access to internal resources   | High (WPA3/WPA2 + individual authentication) |
| COMPANY\_GUEST    | Guests and customers with internet-only access | Standard (WPA2 + shared password)            |

### Employee Wi-Fi network

* **SSID:** `COMPANY_EMPLOYEE`
* **Purpose:** Secure network for employees only
* **Security:** WPA3, or WPA2 if WPA3 is unavailable
* **Authentication:** Individual credentials or certificate-based access
* **Access:** Internal systems, servers, and business applications
* **Allowed devices:** Company laptops, desktops, and approved mobile devices

### Guest Wi-Fi network

* **SSID:** `COMPANY_GUEST`
* **Purpose:** Network access for visitors, guests, and customers
* **Security:** WPA2 with shared password
* **Access:** Internet only, isolated from internal systems
* **Bandwidth limit:** 5–10 Mbps per device
* **Session timeout:** 4 hours

### Wi-Fi security requirements

* Change Wi-Fi passwords every 90 days.
* Disable WPS on all access points.
* Disable default SSID password hints.
* Enable MAC address filtering for the employee network.
* Physically secure all access points and document their locations.
