---
icon: cloud
---

# Cloud Deployments

#### Simplified

<figure><img src="../.gitbook/assets/cloud (3).png" alt=""><figcaption></figcaption></figure>

#### Full

<figure><img src="../.gitbook/assets/cloudfull (4).png" alt=""><figcaption></figcaption></figure>

โครงการ MotherNest เลือกใช้ Google Cloud Platform (GCP) เป็นผู้ให้บริการคลาวด์หลักสำหรับรองรับการประมวลผลแอปพลิเคชัน, ระบบปัญญาประดิษฐ์ (AI & MLOps), และการป้องกันความปลอดภัยเครือข่ายด่านหน้า สถาปัตยกรรมถูกออกแบบภายใต้แนวคิด Serverless-First และ Managed Services เพื่อลดภาระการดูแลระบบ (Operational Overhead) และสามารถขยายตัวอัตโนมัติ (Auto-scaling) เพื่อรองรับเป้าหมายผู้ใช้งาน 1,000 DAU

งบประมาณคลาวด์รายเดือนถูกประเมินไว้ที่ประมาณ \~$360 - $370 USD ต่อเดือน (ประมาณ 12,000 - 13,000 บาท) ซึ่งถือเป็นตัวเลขที่คุ้มค่าและได้รับการปรับแต่ง (Cost-Optimized) อย่างเหมาะสมสำหรับระบบ Health-Tech ระดับ Enterprise ที่มีทั้งโครงสร้าง RAG Pipeline และ Two-Tower Recommendation Engine ทำงานอยู่เบื้องหลัง

### Provider: Google Cloud Platform

<figure><img src="../.gitbook/assets/images (1).png" alt=""><figcaption></figcaption></figure>

#### **1. Network Protection Layer** 🛡️

| Service                      | Detail                                                                                       | Version or Spec                    | Estimated Monthly Cost (USD) |
| ---------------------------- | -------------------------------------------------------------------------------------------- | ---------------------------------- | ---------------------------- |
| Cloud DNS                    | จัดการ Domain Name ของแอป                                                                    | Standard Zone                      | $1.00                        |
| Cloud Load Balancing & Armor | กระจายทราฟฟิกและป้องกันโจมตี (DDoS)                                                          | Global HTTP(S) LB + Basic Armor    | $20.00                       |
| Cloud API Gateway            | ตรวจสอบสิทธิ์เข้าใช้งาน (JWT Auth) และควบคุมโควต้าการเรียกใช้ AI (Rate Limiting) ตามรายบุคคล | Fully Managed Service (GA Version) | \~$2.00                      |
| Cloud CDN                    | ทำหน้าที่เก็บสำเนาไฟล์ ไว้บน Server ที่กระจายอยู่ใกล้ตัวผู้ใช้งาน                            | Standard/Premium Tier              | \~$7.00                      |

#### 2. CI/CD Deployment Pipeline 🚀

| Service           | Detail                                               | Version or Spec                                         | Estimated Monthly Cost (USD) |
| ----------------- | ---------------------------------------------------- | ------------------------------------------------------- | ---------------------------- |
| Cloud Build       | Build, Test และ Deploy ซอฟต์แวร์แบบอัตโนมัติ (CI/CD) | Serverless CI/CD Platform (Managed Service) รุ่นปี 2026 | \~$0.10                      |
| Artifact Registry | คลังเก็บ Docker Images พร้อมระบบสแกนช่องโหว่         | Standard Storage (50GB)                                 | \~$5.00                      |

#### **3. Application and Compute Layer** 💻

| Service          | Detail                                    | Version or Spec                            | Estimated Monthly Cost (USD) |
| ---------------- | ----------------------------------------- | ------------------------------------------ | ---------------------------- |
| Cloud Run & Jobs | รัน API Backend, RAG Pipeline และ ML Jobs | Serverless, 1 vCPU, 512MB RAM (Auto-scale) | \~$10.00                     |

#### **4. Hybrid Connectivity Layer** 🔗

| Service        | Detail                                 | Version or Spec                | Estimated Monthly Cost (USD) |
| -------------- | -------------------------------------- | ------------------------------ | ---------------------------- |
| HA VPN (IPsec) | เชื่อมต่อ On-premise Secure Datacenter | 2 Tunnels (เพื่อความเสถียร HA) | \~$72.00                     |

#### 5. Data Layer 🗄️

<table><thead><tr><th>Service</th><th>Detail</th><th width="184">Version or Spec</th><th>Estimated Monthly Cost (USD)</th></tr></thead><tbody><tr><td>Cloud Storage</td><td>เก็บไฟล์ภาพ, บทความ, เอกสารทางการแพทย์</td><td>Standard Tier (พื้นที่ประมาณ 50 GB)</td><td>~$1.00</td></tr><tr><td>Cloud SQL (PostgreSQL)</td><td>เก็บข้อมูลหลัก และทำ Vector Store (pgvector)</td><td>db-g1-small (1 vCPU, 1.7GB RAM) + 50GB</td><td>~$35.00</td></tr><tr><td>Cloud Firestore</td><td>เก็บ Feature Store, Feedback และ Logs</td><td>Standard (โควต้าอ่านเขียน 50k ครั้ง/วัน)</td><td>~$5.00</td></tr><tr><td>BigQuery</td><td>จัดเก็บ Feature Store, Click Logs และชุดข้อมูลฝึกสอน AI (MLOps)</td><td>On-demand (ฟรี 10 GB Storage / 1 TB Query แรกต่อเดือน)</td><td>~$0.00 - $2.00</td></tr></tbody></table>

#### 6. AI and MLOps Layer 🤖

| Service                   | Detail                                        | Version or Spec                                  | Estimated Monthly Cost (USD) |
| ------------------------- | --------------------------------------------- | ------------------------------------------------ | ---------------------------- |
| Vertex AI Gemini          | LLM ประมวลผล Chatbot ให้คำปรึกษาคุณแม่        | Gemini 1.5 Flash (ประเมิน 1000 DAU)              | \~$20.00                     |
| Vertex AI Vector Search   | ค้นหาและแนะนำบทความ (Feed Recommendation)     | Standard Node (เป็น Fixed Cost ขั้นต่ำของระบบ)   | \~$150.00\*                  |
| Pub/Sub                   | Message Queue จัดการ Task หลังบ้าน            | Standard (ใช้งานไม่เกิน 10 GB/month)             | $0.00 (Free Tier)            |
| Vertex AI Model Registry  | จัดเก็บและจัดการเวอร์ชันของโมเดล ML           | Standard (คิดค่าพื้นที่จัดเก็บแบบ Cloud Storage) | $0.00 (โควต้าฟรีครอบคลุม)    |
| Vertex AI Text-Embeddings | แปลง Text ให้กลายเป็น Vectors หรือ Embeddings | Standard                                         | \~$0.825                     |

#### 7. Operations, Security and Governance Layer 🔐

| Service                       | Detail                                            | Version or Spec                         | Estimated Monthly Cost (USD)    |
| ----------------------------- | ------------------------------------------------- | --------------------------------------- | ------------------------------- |
| Secret Manager & KMS          | เก็บ API Keys และรหัสผ่านฐานข้อมูล                | Standard                                | \~$1.00                         |
| Cloud Monitoring & Audit Logs | ตรวจสอบระบบแจ้งเตือนเมื่อล่ม                      | Standard (ใช้งานโควต้าฟรี)              | $0.00 (Free Tier)               |
| Cloud Scheduler               | ทริกเกอร์งานอัปเดตข้อมูลแบบตั้งเวลา (Cron)        | Standard (<10 jobs)                     | \~$1.00                         |
| IAM (RBAC)                    | จัดการสิทธิ์และการเข้าถึงทรัพยากร                 | Standard Feature                        | $0.00 (ไม่มีค่าบริการเพิ่มเติม) |
| Cloud KMS (CMEK)              | จัดการคีย์สำหรับเข้ารหัสข้อมูล (Encryption Keys)  | Standard (เก็บไม่กี่ Key และใช้งานน้อย) | \~$1.00                         |
| Cloud Audit Logs              | เก็บบันทึกประวัติการเข้าถึงและการทำงาน (Security) | Standard (ฟรี 50GB แรก/เดือน)           | $0.00 (Free Tier)               |

#### 8. BI, Analytics and Admin Layer 📊

| Service       | Detail                                             | Version or Spec         | Estimated Monthly Cost (USD) |
| ------------- | -------------------------------------------------- | ----------------------- | ---------------------------- |
| Looker Studio | ระบบ Dashboard แสดงสถิติการใช้งานและประสิทธิภาพ AI | Free Edition (Standard) | $0.00                        |

#### 💡 Architectural Justifications

เพื่อให้การลงทุนบน Google Cloud Platform เกิดความคุ้มค่าสูงสุดและสอดคล้องกับเป้าหมายทางธุรกิจ โครงการได้มีเหตุผลรองรับการตัดสินใจเลือกใช้บริการในแต่ละเลเยอร์ดังนี้:

1\. กลยุทธ์ Serverless-First เพื่อควบคุมต้นทุน (Compute & CI/CD Layer)

* เหตุผล: โครงการเลือกใช้ Cloud Run สำหรับการรัน Backend API, RAG Orchestrator, และ RecSys Engine แทนการเปิด Virtual Machine (GCE) หรือ Kubernetes (GKE) ทิ้งไว้บนคลาวด์
* ผลลัพธ์: สถาปัตยกรรม Serverless อนุญาตให้ระบบสามารถ "Scale-to-Zero" ได้เมื่อไม่มีผู้ใช้งาน (เช่น ช่วงกลางคืน) ทำให้เสียค่าใช้จ่ายเฉพาะวินาทีที่มีการประมวลผลจริงเท่านั้น (Pay-per-use) ประเมินค่าใช้จ่ายเพียง \~$10/เดือน แต่ยังคงความสามารถในการรองรับ Traffic Spikes ได้ทันที

2\. การกระจายศูนย์ข้อมูลแบบ Polyglot Persistence (Data Layer)

* เหตุผล: ในสถาปัตยกรรมสมัยใหม่ไม่มีฐานข้อมูลใดที่เหมาะสมกับทุกลักษณะงาน (No Silver Bullet) การฝืนเก็บข้อมูลทุกประเภทลงใน Relational Database เพียงอย่างเดียวจะก่อให้เกิดปัญหาคอขวด (Bottleneck) และค่าใช้จ่ายที่สูงเกินความจำเป็น
* ผลลัพธ์ (New Optimized Method): โครงการจึงเลือกใช้กลยุทธ์ Polyglot Persistence เพื่อกระจายภาระงานไปยังฐานข้อมูลที่เหมาะสมที่สุดตามคุณลักษณะของข้อมูล (Data Characteristics) ดังนี้:
  * Cloud SQL (PostgreSQL 16): ใช้สำหรับจัดเก็บข้อมูลที่มีความสัมพันธ์ซับซ้อน (Relational Data) และข้อมูลที่ต้องรักษาความถูกต้องเข้มงวด (Data Integrity)
  * Cloud Firestore (Serverless NoSQL): ใช้จัดเก็บข้อมูลกึ่งโครงสร้าง (Semi-structured) เช่น User Profiles, Real-time Community Feed และ Chat Sessions ซึ่งต้องการความเร็วในการเข้าถึงข้อมูล (Low Latency) และรองรับการทำ Real-time Sync โดยไม่ต้องเสียค่าใช้จ่ายคงที่แบบ Redis
  * In-Memory Local Caching (Efficiency Strategy): โครงการตัดสินใจใช้หน่วยความจำภายใน (Local RAM) ของ Cloud Run ทำหน้าที่เป็น Feed Cache แทนการใช้ External Cache (เช่น Redis) เพื่อบริหารจัดการงบประมาณให้มีประสิทธิภาพสูงสุด (Cost-Effective) โดยยังคงรักษาความเร็วในการแสดงผลหน้าฟีดไว้ในระดับมิลลิวินาที
  * BigQuery (Data Warehouse): ทำหน้าที่เป็นศูนย์กลางข้อมูลสำหรับงาน MLOps เพื่อจัดเก็บ Feature Store และพฤติกรรมการใช้งาน (Clickstream Logs) สำหรับใช้ในการฝึกสอนโมเดล AI ในระยะยาว

3\. การลงทุนที่จำเป็นใน Vertex AI Vector Search (AI & MLOps Layer)

* เหตุผล: ค่าใช้จ่ายที่สูงที่สุดในสถาปัตยกรรมนี้คือ Vertex AI Vector Search (\~$150/เดือน) ซึ่งอาจดูเป็นสัดส่วนที่สูง แต่มีความจำเป็นอย่างยิ่งสำหรับโครงสร้างพื้นฐานระดับองค์กร
* ผลลัพธ์: ระบบ RAG ทางการแพทย์และระบบแนะนำบทความ (Feed Recommendation) จำเป็นต้องใช้การค้นหาแบบ Approximate Nearest Neighbor (ANN) ที่มีความแม่นยำและความหน่วงต่ำระดับมิลลิวินาที การใช้ Managed Vector Search ของ GCP ช่วยรับประกันความเสถียร (SLA) และความเร็วในการดึง Medical Context ซึ่งหากให้วิศวกรสร้างและดูแล Vector Engine เองบนเซิร์ฟเวอร์ จะมีค่าใช้จ่ายแฝง (Hidden Costs) และความเสี่ยงเรื่องระบบล่มที่สูงกว่าค่าบริการ $150 นี้มาก

4\. ความปลอดภัยและการปกป้องข้อมูลระดับ Enterprise (Security & Connectivity Layer)

* เหตุผล: แอปพลิเคชันที่เกี่ยวกับคุณแม่ตั้งครรภ์คือเป้าหมายที่มีมูลค่าสูงสำหรับแฮกเกอร์
* ผลลัพธ์: การวาง Cloud Load Balancing คู่กับ Cloud Armor (WAF) ไว้ที่ด่านหน้า ช่วยป้องกันการโจมตีแบบ DDoS และ SQL Injection ก่อนที่ทราฟฟิกจะเข้าถึง Cloud Run การใช้ Secret Manager และ Cloud KMS รับประกันว่า API Keys จะถูกเข้ารหัส (CMEK) และไม่มีการ Hardcode ไว้ในซอร์สโค้ด นอกจากนี้ การลงทุนใน HA VPN (\~$72/เดือน) เพื่อเชื่อมต่อกับ On-Premise แบบ 2 Tunnels ถือเป็นการปฏิบัติตามข้อกำหนดการส่งผ่านข้อมูลสุขภาพอย่างเคร่งครัด
