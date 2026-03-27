---
icon: database
---

# Storage Distribution Management

เพื่อให้การบริหารจัดการต้นทุน (Cost Optimization) สอดคล้องกับประสิทธิภาพความเร็วในการเข้าถึงข้อมูล และเป็นไปตามข้อบังคับด้านการคุ้มครองข้อมูลส่วนบุคคล (PDPA) โครงการ MotherNest ได้ออกแบบระบบจัดเก็บข้อมูลแบบผสมผสาน (Hybrid Data Segregation) โดยแบ่งการจัดเก็บข้อมูลระหว่าง Local Storage และ Cloud Storage ดังนี้:

<figure><img src="../.gitbook/assets/Share Knowledge-1.png" alt=""><figcaption></figcaption></figure>

| ประเภทข้อมูล (Data Type)                                                                                                                 | ระดับความอ่อนไหว (Sensitivity) | สถานที่จัดเก็บ (Location)                          | บริการ/เทคโนโลยี (Technology)                                                             | เหตุผลเชิงสถาปัตยกรรม (Justification)                                                                                                               |
| ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ | -------------------------------------------------- | ----------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>ข้อมูลระบุตัวตนและประวัติสุขภาพ (PII &#x26; Medical Records)</p><p></p><p><em>(เช่น ชื่อ, อายุครรภ์โดยละเอียด, ประวัติแพ้ยา)</em></p> | 🔴 สูงมาก (Critical)           | <p>Local Storage</p><p>(On-Premise Datacenter)</p> | <p>PostgreSQL</p><p>(Tier 2: SAS SSD)</p>                                                 | Data Sovereignty: ล็อกข้อมูลไว้ในเซิร์ฟเวอร์ภายในองค์กรเท่านั้น เพื่อปฏิบัติตามกฎหมาย PDPA อย่างเคร่งครัด ป้องกันการเข้าถึงจากภายนอก                |
| <p>บริบททางการแพทย์สำหรับ AI (Medical Vector Context)</p><p></p><p><em>(เวกเตอร์ประวัติและคำแนะนำเฉพาะบุคคล)</em></p>                    | 🔴 สูง (High)                  | <p>Local Storage</p><p>(On-Premise Datacenter)</p> | <p>PostgreSQL</p><p>(pgvector)</p>                                                        | Privacy in AI: ให้ AI ประมวลผล RAG ผ่านอุโมงค์ VPN โดยตรง เพื่อไม่ให้เวกเตอร์ข้อมูลสุขภาพหลุดไปอยู่บนฐานข้อมูลสาธารณะ                               |
| <p>ข้อมูลสำรองและประวัติระบบ (System Backups &#x26; Long-term Logs)</p><p></p><p><em>(เช่น VM Snapshots, Zabbix Logs)</em></p>           | 🔴 สูง (High)                  | <p>Local Storage</p><p>(On-Premise Datacenter)</p> | <p><br></p><p>(On-Premise Datacenter)</p><p>Proxmox Backup</p><p>(Tier 3: HDD RAID 1)</p> | Cost-Efficiency & DR: ข้อมูลขนาดใหญ่ที่ไม่ได้เรียกใช้บ่อย (Cold Data) การเก็บใน HDD ช่วยประหยัดงบประมาณมหาศาล และพร้อมใช้สำหรับ Disaster Recovery   |
| รหัสผ่านและข้อมูลยืนยันตัวตนเจ้าหน้าที่ (Admin Credentials)                                                                              | 🔴 สูง (High)                  | <p>Local Storage</p><p>(On-Premise Datacenter)</p> | <p>Active Directory</p><p>(LDAP / Samba4)</p>                                             | Centralized Security: เก็บข้อมูลสิทธิ์การเข้าถึงเซิร์ฟเวอร์ไว้ในเครือข่ายปิด เพื่อป้องกันการโดนเจาะระบบจากอินเทอร์เน็ต                              |
| <p>ระบบปฏิบัติการและข้อมูลชั่วคราว (OS &#x26; Ephemeral Data)</p><p></p><p><em>(เช่น Proxmox VE, K8s Swap, Cache)</em></p>               | 🟡 ปานกลาง (Internal)          | <p>Local Storage</p><p>(On-Premise Datacenter)</p> | <p>Proxmox / K8s</p><p>(Tier 1: NVMe RAID 1)</p>                                          | Max Performance: ข้อมูลระบบที่ต้องการความเร็วในการอ่าน/เขียน (IOPS) ระดับสูงสุด เพื่อให้เซิร์ฟเวอร์บูตและจัดการคอนเทนเนอร์ได้อย่างฉับไวไร้ความหน่วง |
| <p>แคชโปรไฟล์และสถานะแชท (Anonymized Profile &#x26; Chat State)</p><p></p><p><em>(ข้อมูลที่ถูกปิดบังตัวตนแล้ว)</em></p>                  | 🟡 ปานกลาง (Medium)            | <p>Cloud Storage</p><p>(Google Cloud)</p>          | <p>Firestore &#x26;</p><p>Redis Cache</p>                                                 | Latency Mitigation: วางข้อมูล State ไว้ใกล้ฝั่งแอปพลิเคชันที่สุด เพื่อให้แชทบอทและหน้า Feed โหลดได้ทันทีโดยไม่ต้องรอคิวรีผ่าน VPN ทุกครั้ง          |
| <p>เวกเตอร์ของบทความ (Article Vectors)</p><p></p><p><em>(จุดพิกัดความหมายของบทความทั่วไป)</em></p>                                       | 🟢 ต่ำ (Low)                   | <p>Cloud Storage</p><p>(Google Cloud)</p>          | Vertex AI & Vector Search                                                                 | Performance Scale: ใช้ประสิทธิภาพของคลาวด์ในการทำ Similarity Search ค้นหาบทความหลายหมื่นชิ้นได้ในระดับมิลลิวินาที                                   |
| <p>พฤติกรรมการใช้งาน (Click Logs &#x26; Features)</p><p></p><p><em>(ประวัติการกดอ่านบทความเพื่อเทรนโมเดล)</em></p>                       | 🟢 ต่ำ (Low)                   | <p>Cloud Storage</p><p>(Google Cloud)</p>          | <p>BigQuery</p><p>(Data Warehouse)</p>                                                    | MLOps Integration: รวบรวม Log ปริมาณมหาศาลไว้บน BigQuery เพื่อความสะดวกและประหยัดต้นทุนในการป้อนข้อมูลเข้าสู่ Vertex AI Training                    |
| <p>รูปภาพประกอบแอปพลิเคชัน (Public Media &#x26; Assets)</p><p></p><p><em>(ภาพบทความ, กราฟิกทั่วไป)</em></p>                              | ⚪ ทั่วไป (Public)              | <p>Cloud Storage</p><p>(Google Cloud)</p>          | <p>Cloud Storage +</p><p>Cloud CDN</p>                                                    | Bandwidth Optimization: ใช้ CDN กระจายไฟล์ภาพไปตาม Edge Server เพื่อลดภาระแบนด์วิดท์และทำให้แอปพลิเคชันโหลดรูปภาพได้รวดเร็ว                         |

#### 1. Local Server Storage

เซิร์ฟเวอร์ภายในศูนย์ข้อมูล (On-Premise) ถูกวางบทบาทให้เป็น "พื้นที่จัดเก็บข้อมูลที่มีความอ่อนไหวสูง" โดยเน้นที่ความปลอดภัยสูงสุดและความซ้ำซ้อนของข้อมูล (Redundancy)

* ข้อมูลที่จัดเก็บ: ข้อมูลประวัติสุขภาพส่วนบุคคล (PII), อายุครรภ์โดยละเอียด, และฐานข้อมูลเวกเตอร์ทางการแพทย์ (Medical Vector Database) บน PostgreSQL
* โครงสร้างการจัดเก็บ (Storage Tiering):
  * Tier 1 (NVMe RAID 1): สำหรับ OS และ Hot Data เพื่อความเร็วสูงสุด
  * Tier 2 (SAS SSD RAID 10): สำหรับ Vector Database และฐานข้อมูลหลัก
  * Tier 3 (HDD RAID 1): สำหรับข้อมูลสำรอง (Backup) และ Cold Data
* จุดประสงค์หลัก: เพื่อรักษาอธิปไตยของข้อมูล (Data Sovereignty) ไม่ให้ข้อมูลสำคัญหลุดออกไปยัง Public Cloud และรองรับการทำ Automated Backup แบบวงรอบเพื่อป้องกัน Ransomware

#### 2. Cloud Storage & Managed Services

Google Cloud Platform (GCP) ถูกวางบทบาทให้เป็น "พื้นที่จัดเก็บข้อมูลเพื่อการให้บริการและประมวลผล AI" โดยเน้นที่ความเร็วในการตอบสนองผู้ใช้และความยืดหยุ่นในการขยายตัว (Scalability)

*   ข้อมูลที่จัดเก็บ: \* ไฟล์มีเดียและเอกสารดิบ: เช่น รูปภาพประกอบแอปพลิเคชันหรือเอกสาร PDF ทั่วไป (เก็บใน Cloud Storage)

    * แคชและข้อมูลสถานะ (State & Cache): ประวัติการแชทชั่วคราว และ Profile Cache ที่ถูกระบุตัวตนแบบไม่เปิดเผยชื่อ (Anonymized) เพื่อให้แอปพลิเคชันดึงไปใช้งานได้ทันที (เก็บใน Firestore และ Redis)
    * ข้อมูลพฤติกรรมการใช้งาน: ประวัติการคลิกบทความ (Click Logs) สำหรับเทรน AI (เก็บใน BigQuery)


* Vector Database (Serving Index): ใช้ Vertex AI Vector Search เพื่อทำ ANN (Approximate Nearest Neighbor) Query สำหรับระบบแนะนำบทความ (Feed) และ Chatbot ให้ตอบสนองได้รวดเร็ว
* จุดประสงค์หลัก: เพื่อให้ผู้ใช้งาน Mobile App สามารถโหลดหน้าฟีดและแชทบอทได้อย่างรวดเร็ว โดยลดภาระการดึงข้อมูลข้ามเครือข่าย VPN (Data Locality)

#### 3. Storage Optimization & Lifecycle

เพื่อให้สถาปัตยกรรมทำงานร่วมกันได้อย่างมีประสิทธิภาพสูงสุด โครงการได้กำหนดนโยบายการจัดการวงจรข้อมูล (Data Lifecycle Policies) ดังนี้:

* ลดภาระค่าใช้จ่ายเครือข่าย (Egress Cost Reduction): เลือกดึงเฉพาะข้อมูลที่จำเป็นจริงๆ จาก Local Server ไปประมวลผลบนคลาวด์ และนำผลลัพธ์ไปพักไว้ในระบบ Cache บนคลาวด์เพื่อลดการเรียกขอข้อมูลซ้ำ (Deduplication of Requests)
* การจัดการข้อมูลย้อนหลัง: ข้อมูลการสำรองระบบ (System Backups) หรือบันทึกประวัติการเข้าใช้งาน (Audit Logs) ที่มีอายุเกินกำหนด จะถูกย้ายจากพื้นที่จัดเก็บความเร็วสูง (SSD) ไปยังพื้นที่จัดเก็บต้นทุนต่ำ (Cold Storage/HDD) ทั้งในฝั่ง On-Premise และ Cloud เพื่อประหยัดงบประมาณในระยะยาว

#### 💡 Architectural Justifications

ในการออกแบบยุทธศาสตร์การกระจายข้อมูลระหว่าง On-Premise และ Google Cloud Platform โครงการ MotherNest ได้ตัดสินใจบนพื้นฐานของความปลอดภัย (Security), ประสิทธิภาพ (Performance), และความคุ้มค่า (Cost-Efficiency) ดังนี้:

1\. การรักษาอธิปไตยของข้อมูล (Data Sovereignty) และ PDPA

* เหตุผล: ข้อมูลสุขภาพ (Health Data) ถือเป็นข้อมูลส่วนบุคคลที่มีความอ่อนไหวสูงมากตามกฎหมาย PDPA การนำข้อมูลประเภทนี้ไปฝากไว้บนฐานข้อมูลสาธารณะ (Public Cloud) ทั้งหมด อาจสร้างความเสี่ยงด้านการรั่วไหลและข้อจำกัดทางกฎหมาย
* ผลลัพธ์: โครงการจึงตัดสินใจใช้สถาปัตยกรรมแบบ Hybrid โดยบังคับให้ข้อมูล PII และประวัติการแพทย์ (Medical Records) ถูก "ล็อก" ไว้ที่ Local Server Storage ภายในศูนย์ข้อมูลองค์กรเท่านั้น สิ่งที่อนุญาตให้ส่งขึ้นไปประมวลผลบน Cloud จะมีเพียงข้อมูลที่ถูกปิดบังตัวตน (Anonymized) หรืออยู่ในรูปแบบของเวกเตอร์ (Embeddings) เท่านั้น ซึ่งเป็นการออกแบบระบบรักษาความปลอดภัยแบบ Privacy by Design อย่างแท้จริง

2\. การลดความหน่วงเครือข่ายเพื่อประสบการณ์ผู้ใช้ (Latency Mitigation)

* เหตุผล: หากผู้ใช้งาน Mobile App 1,000 คน ทำการร้องขอหน้า Feed หรือพูดคุยกับแชทบอท แล้วระบบต้องวิ่งผ่านอุโมงค์ VPN กลับมาคิวรีฐานข้อมูลที่ Local Server ทุกครั้ง จะทำให้เกิดปัญหาคอขวด (Network Bottleneck) และแอปพลิเคชันตอบสนองช้า
* ผลลัพธ์: โครงการจึงออกแบบให้มีการ กระจายข้อมูล (Data Distribution) นำข้อมูลที่จำเป็นต่อการแสดงผล (เช่น Profile Cache หรือ Chat Session) ไปวางไว้ที่ Cloud Storage, Firestore, และ Redis บนคลาวด์ เพื่อให้ข้อมูลอยู่ใกล้กับระบบประมวลผลแอปพลิเคชัน (Data Locality) มากที่สุด ทำให้แอปพลิเคชันสามารถตอบสนองผู้ใช้ได้ในระดับมิลลิวินาที (Milliseconds)

3\. การบริหารจัดการต้นทุนด้วย Storage Tiering และ Lifecycle (Sustainable Cost Optimization)

* เหตุผล: ฐานข้อมูลเวกเตอร์ (Vector Database) ของ AI ต้องการความเร็วของ SSD/NVMe ในการทำงานขั้นสูงสุด แต่การใช้ฮาร์ดดิสก์ความเร็วสูงทั้งหมดเพื่อเก็บข้อมูลทุกประเภท (รวมถึงไฟล์ Backup) เป็นการสิ้นเปลืองงบประมาณอย่างมาก
* ผลลัพธ์: โครงการจึงบังคับใช้นโยบาย Storage Tiering ในระดับ Local Server (ใช้ SSD เฉพาะข้อมูล AI/Database และใช้ HDD สำหรับข้อมูลสำรอง) ควบคู่ไปกับ Lifecycle Policies ทั้งบน Local และ Cloud เพื่อย้ายข้อมูลที่หมดความจำเป็นหรือเก่าเกินกำหนดไปสู่พื้นที่จัดเก็บต้นทุนต่ำ กลยุทธ์นี้ช่วยให้โครงการสามารถขยายสเกลระบบได้ในระยะยาวโดยที่ค่าใช้จ่ายฮาร์ดแวร์และค่าบริการ Cloud ไม่พุ่งสูงจนเกินควบคุม
