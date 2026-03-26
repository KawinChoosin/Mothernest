---
icon: gear
---

# Server Management

## Server

| Type                                         | CPU                                         | Storage                                                                               | Operating System | Software                       |
| -------------------------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------- | ---------------- | ------------------------------ |
| Virtualization & Compute Servers             | Intel Xeon Gold 5418Y 24 Cores / 48 Threads | <p>2 × 480GB NVMe RAID 1</p><p>4 × 960GB SAS SSD RAID 10</p><p>2 × 4TB HDD RAID 1</p> | Rocky Linux 9    | -                              |
| Infrastructure Servers (Monitoring & Backup) | Intel Xeon Gold 5418Y 8 Cores               | 4 × 4TB HDD RAID 1                                                                    | Rocky Linux 9    | Zabbix + Proxmox Backup Server |

## Virtual Machine Cluster

<table><thead><tr><th width="162.04296875" valign="middle">VM Type</th><th width="277.9765625">Usage</th><th width="105.09765625">Amount</th><th width="167.90625">Operating System</th><th>Software</th></tr></thead><tbody><tr><td valign="middle">K8s Control</td><td>สั่งการและควบคุมดูแลความเรียบร้อยของ Kubernetes ทั้งหมด</td><td>1</td><td>Ubuntu Server 24.04 LTS</td><td>kubeadm, containerd, Etcd</td></tr><tr><td valign="middle">K8s Worker</td><td>เป็นพื้นที่สำหรับรัน Containersในชั้น Application Services Layer</td><td>2</td><td>Ubuntu Server 24.04 LTS</td><td>Kubelet, Kube-proxy, Docker Runtime</td></tr><tr><td valign="middle">Database Primary</td><td>เป็นฐานข้อมูลหลักสำหรับการเขียนข้อมูล (Write Operations) และเก็บข้อมูลที่ต้องการความถูกต้องสูง (ACID Compliance) เช่น ข้อมูลสุขภาพและบัญชีผู้ใช้งาน</td><td>1</td><td>Ubuntu Server 24.04 LTS</td><td>PostgreSQL 16, pgvector, pgBouncer</td></tr><tr><td valign="middle">Database Replica</td><td>รองรับการอ่านข้อมูล และทำ Vector Search สำหรับ AI เพื่อลดภาระเครื่อง Primary ไม่ให้ระบบหน่วง</td><td>1</td><td>Ubuntu Server 24.04 LTS</td><td>PostgreSQL 16, pgvector, pgBouncer</td></tr><tr><td valign="middle">Object Storage</td><td>ใช้สำหรับเก็บไฟล์ขนาดใหญ่แบบ On-premise เช่น รูปภาพโปรไฟล์ชั่วคราว หรือไฟล์ Log ก่อนที่จะถูก Sync ขึ้นไปสำรองบน Cloud Storage เพื่อความรวดเร็วในการเข้าถึงภายในเครือข่าย</td><td>1</td><td>Ubuntu Server 24.04 LTS</td><td>MinIO</td></tr><tr><td valign="middle">Active Directory / LDAP</td><td>จัดการสิทธิ์การเข้าถึง (RBAC) และการยืนยันตัวตนของเจ้าหน้าที่ในองค์กรที่เข้ามาดูแลระบบทั้งหมดแบบรวมศูนย์</td><td>2</td><td>Ubuntu Server (Samba4)</td><td>Active Directory Domain Services (AD DS), LDAPS</td></tr></tbody></table>

## Application Services Layer

<table><thead><tr><th width="165.125">Service Name</th><th width="281.45703125">Usage</th><th width="102.71484375" data-type="number">CPU (vCPU)</th><th width="96.04296875" data-type="number">RAM (GB)</th><th width="221.47265625">Deployment Type</th><th>Linked Database</th></tr></thead><tbody><tr><td>API Gateway</td><td>ทำหน้าที่ Reverse Proxy, Load Balancing และจัดการ Traffic ขาเข้า</td><td>4</td><td>8</td><td>K8s Worker</td><td>-</td></tr><tr><td>Auth Service</td><td>จัดการ JWT Token, User Session และติดต่อ Database (PostgreSQL) เพื่อยืนยันตัวตน</td><td>4</td><td>8</td><td>K8s Worker</td><td>PostgreSQL</td></tr><tr><td>AI Inference</td><td>ประมวลผลโมเดล AI โดยโหลด Model File จาก Object Storage (MinIO)</td><td>24</td><td>64</td><td>K8s Worker</td><td>Object Storage (Models)</td></tr><tr><td>Data Sanitization</td><td>ใช้สำหรับ Pre-processing ข้อมูล, Clean ข้อมูลขยะ และจัดฟอร์แมตก่อนส่งให้ AI หรือ Database</td><td>8</td><td>16</td><td>K8s Worker</td><td>-</td></tr></tbody></table>

## On-Premise Infrastructure Overview

### Secure Datacenter

สำหรับ Secure Datacenter เป็นชั้นของโครงสร้างพื้นฐานทางกายภาพที่ใช้สำหรับรองรับระบบ On-Premise โดยประกอบด้วยเซิร์ฟเวอร์จำนวน 4 เครื่อง โดยเซิร์ฟเวอร์ทั้งหมดถูกเชื่อมต่อกับเครือข่ายภายในผ่าน Core Switch จำนวน 2 ตัว (Core Switch A และ Core Switch B) เพื่อรองรับการทำงานแบบ Redundancy ในกรณีที่สวิตช์ตัวใดตัวหนึ่งเกิดปัญหา

ในด้านความปลอดภัยของเครือข่าย ระบบมีการติดตั้ง Fortinet FortiGate Firewall จำนวน 2 เครื่อง ในรูปแบบ Active–Passive High Availability โดย Firewall ทำหน้าที่ควบคุมการเข้าถึงเครือข่ายและป้องกันภัยคุกคามจากภายนอก ซึ่งการทำงานแบบ Active–Passive นี้ จะช่วยให้ระบบยังคงปลอดภัยอย่างต่อเนื่อง หาก Firewall เครื่องหลักเกิดความล้มเหลว และ Firewall ยังทำหน้าที่สร้าง Site-to-Site VPN เพื่อเชื่อมต่อระหว่าง Data Center ภายในองค์กรกับ Cloud Platform ทำให้ระบบสามารถทำงานร่วมกันระหว่าง On-Premise Infrastructure และ Cloud Services ได้อย่างปลอดภัยอีกด้วย

นอกจากนี้ ภายในระบบยังมี Infrastructure Server ซึ่งประกอบด้วยบริการ Zabbix สำหรับการตรวจสอบสถานะของระบบ (Monitoring) และ Proxmox Backup Server สำหรับการสำรองข้อมูลของ Virtual Machines

### Platform Layer

#### Virtualization Platform

บนเซิร์ฟเวอร์แต่ละเครื่องมีการติดตั้ง Proxmox Virtual Environment (Proxmox VE) ซึ่งทำหน้าที่เป็น Hypervisor สำหรับสร้างและบริหารจัดการ Virtual Machines (VMs) โดย Proxmox VE จะทำหน้าที่จัดสรรทรัพยากรของระบบ เช่น CPU, Memory และ Storage ให้กับ VMs ที่ใช้สำหรับรัน Kubernetes Cluster และบริการต่าง ๆ ของระบบ

เซิร์ฟเวอร์ทั้ง 4 เครื่องถูกนำมารวมกันเป็น Proxmox Cluster ซึ่งถูกควบคุมและบริหารจัดการผ่าน Proxmox Cluster Manager โดยส่วนนี้ทำหน้าที่เป็นศูนย์กลางในการจัดการ VMs ทั้งหมดภายใน Cluster รวมถึงการกำหนดตำแหน่งการทำงานของแต่ละ VM บนเซิร์ฟเวอร์แต่ละเครื่องอย่างเหมาะสมตามทรัพยากรที่มีอยู่

นอกจากนี้ Proxmox Cluster Manager ยังรองรับการย้าย Virtual Machines ระหว่างเซิร์ฟเวอร์ภายใน Cluster (VM Migration) ซึ่งช่วยให้สามารถปรับสมดุลภาระงาน (Load Balancing) และเพิ่มความยืดหยุ่นในการใช้งานระบบได้

อย่างไรก็ตาม เพื่อรองรับการย้าย Virtual Machines และการทำ High Availability (HA) ในกรณีที่เซิร์ฟเวอร์เครื่องใดเครื่องหนึ่งเกิดความล้มเหลว ระบบจึงใช้ Ceph ที่เป็นฟีเจอร์ Build-in ใน Proxmox VE ทำหน้าที่เป็น Shared Storage ช่วยให้ทุกเซิร์ฟเวอร์ภายใน Cluster สามารถเข้าถึงข้อมูลของ VM ได้ร่วมกัน ทำให้สามารถย้ายหรือเริ่มต้นการทำงานของ VM บนเซิร์ฟเวอร์เครื่องอื่นได้อย่างต่อเนื่อง

#### Virtual Machine Cluster

Virtual Machine Cluster ทำหน้าที่เป็นสภาพแวดล้อมสำหรับการประมวลผลหลักของระบบ โดย VMs เหล่านี้ถูกสร้างขึ้นโดย Proxmox VE และถูกใช้เป็นโครงสร้างพื้นฐานสำหรับการทำงานของ Kubernetes Cluster และบริการต่าง ๆ ภายในระบบ

ภายในระบบมีการกำหนด Virtual Machines จำนวนหนึ่งให้ทำหน้าที่เป็น Kubernetes Control Plane Nodes ซึ่งมีหน้าที่ควบคุมการทำงานของ Kubernetes Cluster โดยทำหน้าที่จัดการสถานะของระบบภายใน Cluster ควบคุมการทำงานของ components ต่าง ๆ และทำการตัดสินใจในการจัดสรร workloads ไปยัง Kubernetes Worker Node ที่เหมาะสม

นอกจากนี้ยังมี Virtual Machines ที่ทำหน้าที่เป็น Kubernetes Worker Nodes ซึ่งเป็น node ที่ใช้สำหรับรัน Containerized Applications โดย Kubernetes จะทำการ schedule containers หรือ pods ไปยัง worker nodes เหล่านี้เพื่อให้สามารถประมวลผลบริการของระบบได้อย่างมีประสิทธิภาพ

นอกเหนือจาก node ของ Kubernetes แล้ว ระบบยังมี Virtual Machines สำหรับบริการโครงสร้างพื้นฐานเพิ่มเติม เช่น Database Server สำหรับจัดเก็บข้อมูลเชิงโครงสร้างของระบบ และ Object Storage Server สำหรับจัดเก็บข้อมูลที่ไม่มีโครงสร้าง เช่น ไฟล์ข้อมูลขนาดใหญ่หรือโมเดล AI

แม้ว่า VMs เหล่านี้จะถูกสร้างขึ้นในระดับ Virtualization ภายใต้ Proxmox VE แต่ภายในแต่ละ VM มีการติดตั้ง Kubernetes components ทำให้สามารถทำหน้าที่เป็นส่วนหนึ่งของ Kubernetes Cluster และร่วมกันให้บริการ Containerized Applications ของระบบได้อย่างสมบูรณ์

#### Container Platform

Container Platform ทำหน้าที่เป็นแพลตฟอร์มสำหรับการจัดการและควบคุมการทำงานของ Containerized Applications ภายในระบบ โดย Docker ถูกใช้สำหรับการสร้าง container images ในขณะที่ Kubernetes ทำหน้าที่จัดการและควบคุมการทำงานของ containers

Kubernetes จะทำการ deploy และจัดสรรการทำงาน (scheduling) ของ pods ไปยัง Virtual Machines ที่ทำหน้าที่เป็น Kubernetes Worker ตามทรัพยากรที่มีอยู่ โดยภายในแต่ละเครื่องจะมี container runtime (เช่น containerd) ทำหน้าที่รัน containers ตามที่ Kubernetes กำหนด

นอกจากนี้ Kubernetes ยังรองรับการขยายตัวของบริการ (scaling) ได้อย่างยืดหยุ่น โดยสามารถเพิ่มหรือลดจำนวน pods ของแต่ละบริการตามภาระงานของระบบได้โดยอัตโนมัติ (auto-scaling) ทำให้ระบบสามารถรองรับปริมาณการใช้งานที่เพิ่มขึ้นได้อย่างมีประสิทธิภาพ

เพื่อรองรับการเข้าถึงบริการจากภายนอก Kubernetes Cluster ระบบมีการใช้งาน Ingress Controller ซึ่งทำหน้าที่เป็นจุดรับคำร้องขอจากภายนอกในระดับ Application Layer (HTTP/HTTPS) และทำการกำหนดเส้นทาง (routing) ไปยัง services ที่เกี่ยวข้องภายใน Cluster

#### Application Services Layer

Application Services Layer เป็นชั้นของบริการที่ทำหน้าที่ประมวลผลและให้บริการฟังก์ชันการทำงานของระบบ โดยบริการต่าง ๆ จะถูกพัฒนาในรูปแบบ Containerized Applications และถูก deploy ภายใน Kubernetes Cluster

บริการหลักของระบบประกอบด้วย API Gateway ซึ่งทำหน้าที่เป็นจุดรับคำร้องขอ (request) จากผู้ใช้งานหรือระบบภายนอก และทำหน้าที่กระจายคำร้องขอไปยังบริการภายในที่เกี่ยวข้อง

ระบบยังมี Authentication Service สำหรับจัดการกระบวนการยืนยันตัวตนและการกำหนดสิทธิ์การเข้าถึงของผู้ใช้งาน โดยบริการนี้จะมีการเชื่อมต่อกับฐานข้อมูล PostgreSQL เพื่อจัดเก็บและจัดการข้อมูลผู้ใช้

สำหรับการประมวลผลด้านปัญญาประดิษฐ์ ระบบมี AI Inference Service ซึ่งทำหน้าที่เรียกใช้งานโมเดล AI เพื่อทำการประมวลผลหรือวิเคราะห์ข้อมูล โดยไฟล์โมเดลจะถูกจัดเก็บไว้ใน Object Storage และถูกโหลดมาใช้งานเมื่อมีการร้องขอการประมวลผล

PostgreSQL 16, pgvector, pgBouncerนอกจากนี้ยังมี Data Sanitization Service ซึ่งทำหน้าที่ตรวจสอบและปรับปรุงข้อมูลก่อนที่จะถูกนำไปใช้ในกระบวนการประมวลผลอื่น ๆ ของระบบ เพื่อให้มั่นใจว่าข้อมูลที่เข้าสู่ระบบมีความถูกต้องและเหมาะสมสำหรับการใช้งาน
