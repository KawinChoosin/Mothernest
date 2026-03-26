---
icon: house-medical
---

# Data Center Facility & Operations Management

เพื่อให้ศูนย์ข้อมูล (Data Center) ของระบบ MotherNest สามารถรองรับการทำงานของแอปพลิเคชันทางการแพทย์และระบบ AI ได้อย่างมีประสิทธิภาพและปลอดภัยขั้นสูงสุด โครงการจึงได้ออกแบบโครงสร้างพื้นฐานทางกายภาพและมาตรฐานการปฏิบัติงาน (Facility & Operations) โดยมุ่งเน้นที่ความพร้อมใช้งานสูง (High Availability) และความปลอดภัย (Security) ใน 5 มิติหลัก ดังนี้:

#### 1. ระบบไฟฟ้าสำรองและการจ่ายไฟ (Power Supply & Redundancy System)

<figure><img src="../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

เพื่อป้องกันการหยุดชะงักของระบบจากปัญหาไฟฟ้า ศูนย์ข้อมูลได้ออกแบบระบบแหล่งจ่ายไฟที่มีความเสถียรสูง:

* Parallel Redundant (N+1) UPS: ใช้ระบบเครื่องสำรองไฟฟ้า (UPS) แบบ N+1 ทำงานร่วมกับเครื่องปั่นไฟ (Generator) เพื่อรับประกันการจ่ายไฟต่อเนื่อง 24 ชั่วโมง (24/7) แม้ในกรณีที่เกิดกระแสไฟฟ้าหลักขัดข้อง (Utility Grid Failure)
* Dual Feed System: อุปกรณ์หัวใจสำคัญ เช่น Server และ Core Switch จะได้รับการเชื่อมต่อแหล่งจ่ายไฟแบบคู่ (A และ B) ซึ่งแยกวงจรออกจากกันอย่างเด็ดขาด การออกแบบนี้ช่วยขจัดปัญหาจุดหย่อนคล้อยจุดเดียว (Single Point of Failure) ทำให้ระบบยังทำงานได้แม้แหล่งจ่ายไฟฝั่งใดฝั่งหนึ่งมีปัญหา

#### 2. ระบบจัดเก็บและสำรองข้อมูล (Data Storage & Backup System)

<figure><img src="../../.gitbook/assets/image (8).png" alt="" width="548"><figcaption></figcaption></figure>

เนื่องจากระบบจัดการกับข้อมูลสุขภาพส่วนบุคคล (PDPA) ความปลอดภัยของข้อมูล (Data Integrity) จึงเป็นสิ่งสำคัญสูงสุด:

* RAID 10 Architecture: เซิร์ฟเวอร์จัดเก็บข้อมูลหลักทำงานบนมาตรฐาน RAID 10 (Disk Striping and Mirroring) ซึ่งให้สมดุลที่ยอดเยี่ยมทั้งในด้านประสิทธิภาพความเร็วในการอ่าน/เขียน (Performance Balance) และความปลอดภัยของข้อมูล
* High Availability Storage: หากมีฮาร์ดดิสก์ลูกใดลูกหนึ่งเกิดความเสียหาย ระบบจะยังคงสามารถให้บริการและอ่านข้อมูลต่อได้โดยไม่หยุดชะงัก (Zero Downtime)
* Automated & Off-site Backup: ระบบมีการตั้งเวลาสำรองข้อมูลฐานข้อมูล PostgreSQL และ MinIO แบบอัตโนมัติตามวงรอบ (Daily Full & Hourly Incremental) พร้อมทั้งมีนโยบายเก็บสำเนาข้อมูลไว้นอกศูนย์ข้อมูล (Off-site Copy) เพื่อรับมือกับเหตุภัยพิบัติ (Disaster Recovery) หรือการโจมตีจาก Ransomware

#### 3. ระบบเฝ้าระวังและแจ้งเตือน (Proactive Monitoring System)

<figure><img src="../../.gitbook/assets/image (14) (1).png" alt="" width="563"><figcaption></figcaption></figure>

* Real-time Monitoring: มีการเฝ้าระวังสถานะของ Server, Network, Database และ Storage แบบเรียลไทม์ตลอด 24/7
* Automated Alerting: ระบบจะส่งการแจ้งเตือนอัตโนมัติทันทีเมื่อทรัพยากรระบบถูกใช้งานถึงขีดจำกัด (Threshold) เช่น CPU ทำงานหนักเกินไป, พื้นที่ Disk ใกล้เต็ม, หรือ Database ไม่ตอบสนอง
* Operational Visibility: ข้อมูลเหล่านี้ช่วยให้ผู้ดูแลระบบ (System Administrator) สามารถมองเห็นแนวโน้มของปัญหาและเข้าแก้ไขได้ในเชิงรุก (Proactive) ก่อนที่จะเกิดความเสียหายร้ายแรงต่อระบบ

#### 4. การควบคุมสภาพแวดล้อมและความปลอดภัยทางกายภาพ (Environmental & Physical Security)

<div><figure><img src="../../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (11) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

* Precision Cooling & Humidity Control: ใช้ระบบปรับอากาศแบบควบคุมความแม่นยำสูง (Precision Cooling) เพื่อรักษาอุณหภูมิห้องเซิร์ฟเวอร์ให้อยู่ในระดับ 18-22 องศาเซลเซียส และมีการควบคุมความชื้นสัมพัทธ์เพื่อป้องกันการเกิดไฟฟ้าสถิตที่อาจทำลายฮาร์ดแวร์
* Physical Access Control: พื้นที่ศูนย์ข้อมูลถูกจำกัดการเข้าถึงอย่างเข้มงวด อนุญาตเฉพาะผู้ดูแลระบบที่มี Keycard เท่านั้น
* Surveillance & Fire Detection: มีระบบกล้องวงจรปิด (CCTV) บันทึกภาพตลอด 24/7 และบันทึกประวัติ (Log) การเข้าออกทุกครั้ง รวมไปถึงการติดตั้งระบบตรวจจับควันและระงับอัคคีภัยขั้นสูง

#### 5. มาตรฐานการปฏิบัติงาน (Documentation & SOP)

<figure><img src="../../.gitbook/assets/image (13) (1).png" alt="" width="563"><figcaption></figcaption></figure>

เพื่อลดความเสี่ยงจากความผิดพลาดของมนุษย์ (Human Error) และการพึ่งพาบุคคลใดบุคคลหนึ่ง (Knowledge Silos) ศูนย์ข้อมูลได้จัดทำคู่มือและมาตรฐานการปฏิบัติงาน (Standard Operating Procedure - SOP):

* Runbooks: คู่มือการตั้งค่าและการแก้ไขปัญหาเบื้องต้นสำหรับ Server, Network และ Database
* Incident Response Plan: ขั้นตอนและแผนปฏิบัติการฉุกเฉินในการรับมือกับกรณีระบบล่ม (System Outage) หรือเหตุข้อมูลเสียหาย
*   Knowledge Transfer: เอกสารทั้งหมดถูกจัดทำเพื่อสนับสนุนการถ่ายทอดความรู้ภายในทีมวิศวกรอย่างเป็นระบบ

    <a class="button secondary"></a>
