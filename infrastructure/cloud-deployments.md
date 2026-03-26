---
icon: cloud
---

# Cloud Deployments

## Cloud provider and Services

#### Simplified

<figure><img src="../.gitbook/assets/diagram-export-3-25-2026-11_38_26-PM (1).png" alt=""><figcaption></figcaption></figure>

#### Full

<figure><img src="../.gitbook/assets/cloudfull.png" alt=""><figcaption></figcaption></figure>

### Provider: Google Cloud Platform

#### **1. Network Protection Layer**

| Service                      | Detail                                                            | Version or Spec                 | Estimated Monthly Cost (USD) |
| ---------------------------- | ----------------------------------------------------------------- | ------------------------------- | ---------------------------- |
| Cloud DNS                    | จัดการ Domain Name ของแอป                                         | Standard Zone                   | $1.00                        |
| Cloud Load Balancing & Armor | กระจายทราฟฟิกและป้องกันโจมตี (DDoS)                               | Global HTTP(S) LB + Basic Armor | $20.00                       |
| Cloud CDN                    | ทำหน้าที่เก็บสำเนาไฟล์ ไว้บน Server ที่กระจายอยู่ใกล้ตัวผู้ใช้งาน | Standard/Premium Tier           | \~$7.00                      |

#### 2. CI/CD Deployment Pipeline

| Service     | Detail                                               | Version or Spec                                         | Estimated Monthly Cost (USD) |
| ----------- | ---------------------------------------------------- | ------------------------------------------------------- | ---------------------------- |
| Cloud Build | Build, Test และ Deploy ซอฟต์แวร์แบบอัตโนมัติ (CI/CD) | Serverless CI/CD Platform (Managed Service) รุ่นปี 2026 | \~$0.10                      |

#### **3. Application and Compute Layer**

| Service          | Detail                                    | Version or Spec                            | Estimated Monthly Cost (USD) |
| ---------------- | ----------------------------------------- | ------------------------------------------ | ---------------------------- |
| Cloud Run & Jobs | รัน API Backend, RAG Pipeline และ ML Jobs | Serverless, 1 vCPU, 512MB RAM (Auto-scale) | \~$10.00                     |

#### **4. Hybrid Connectivity Layer**

| Service        | Detail                                 | Version or Spec                | Estimated Monthly Cost (USD) |
| -------------- | -------------------------------------- | ------------------------------ | ---------------------------- |
| HA VPN (IPsec) | เชื่อมต่อ On-premise Secure Datacenter | 2 Tunnels (เพื่อความเสถียร HA) | \~$72.00                     |

#### 5. Data Layer

| Service                | Detail                                       | Version or Spec                          | Estimated Monthly Cost (USD) |
| ---------------------- | -------------------------------------------- | ---------------------------------------- | ---------------------------- |
| Cloud Storage          | เก็บไฟล์ภาพ, บทความ, เอกสารทางการแพทย์       | Standard Tier (พื้นที่ประมาณ 50 GB)      | \~$1.00                      |
| Cloud SQL (PostgreSQL) | เก็บข้อมูลหลัก และทำ Vector Store (pgvector) | db-g1-small (1 vCPU, 1.7GB RAM) + 50GB   | \~$35.00                     |
| Cloud Firestore        | เก็บ Feature Store, Feedback และ Logs        | Standard (โควต้าอ่านเขียน 50k ครั้ง/วัน) | \~$5.00                      |
| Memorystore (Redis)    | ระบบ Cache ลดภาระ Database ให้แอปโหลดไว      | Basic Tier (1 GB capacity)               | \~$35.00                     |

#### 6. AI and MLOps Layer

| Service                   | Detail                                        | Version or Spec                                  | Estimated Monthly Cost (USD) |
| ------------------------- | --------------------------------------------- | ------------------------------------------------ | ---------------------------- |
| Vertex AI Gemini          | LLM ประมวลผล Chatbot ให้คำปรึกษาคุณแม่        | Gemini 1.5 Flash (ประเมิน 1000 DAU)              | \~$20.00                     |
| Vertex AI Vector Search   | ค้นหาและแนะนำบทความ (Feed Recommendation)     | Standard Node (เป็น Fixed Cost ขั้นต่ำของระบบ)   | \~$150.00\*                  |
| Pub/Sub                   | Message Queue จัดการ Task หลังบ้าน            | Standard (ใช้งานไม่เกิน 10 GB/month)             | $0.00 (Free Tier)            |
| Vertex AI Model Registry  | จัดเก็บและจัดการเวอร์ชันของโมเดล ML           | Standard (คิดค่าพื้นที่จัดเก็บแบบ Cloud Storage) | $0.00 (โควต้าฟรีครอบคลุม)    |
| Vertex AI Text-Embeddings | แปลง Text ให้กลายเป็น Vectors หรือ Embeddings | Standard                                         | \~$0.825                     |

#### 7. Operations, Security and Governance Layer

| Service                       | Detail                                            | Version or Spec                         | Estimated Monthly Cost (USD)    |
| ----------------------------- | ------------------------------------------------- | --------------------------------------- | ------------------------------- |
| Secret Manager & KMS          | เก็บ API Keys และรหัสผ่านฐานข้อมูล                | Standard                                | \~$1.00                         |
| Cloud Monitoring & Audit Logs | ตรวจสอบระบบแจ้งเตือนเมื่อล่ม                      | Standard (ใช้งานโควต้าฟรี)              | $0.00 (Free Tier)               |
| Cloud Scheduler               | ทริกเกอร์งานอัปเดตข้อมูลแบบตั้งเวลา (Cron)        | Standard (<10 jobs)                     | \~$1.00                         |
| IAM (RBAC)                    | จัดการสิทธิ์และการเข้าถึงทรัพยากร                 | Standard Feature                        | $0.00 (ไม่มีค่าบริการเพิ่มเติม) |
| Cloud KMS (CMEK)              | จัดการคีย์สำหรับเข้ารหัสข้อมูล (Encryption Keys)  | Standard (เก็บไม่กี่ Key และใช้งานน้อย) | \~$1.00                         |
| Cloud Audit Logs              | เก็บบันทึกประวัติการเข้าถึงและการทำงาน (Security) | Standard (ฟรี 50GB แรก/เดือน)           | $0.00 (Free Tier)               |

## MLOps Architecture Diagram

### 1. AI Recommendation Feed

<figure><img src="../.gitbook/assets/ai feedback.png" alt=""><figcaption></figcaption></figure>

ระบบ AI สำหรับแนะนำบทความนี้ ถูกออกแบบมาเพื่อคัดเลือกบทความที่ตรงกับความสนใจและอายุครรภ์ของคุณแม่แต่ละท่านมากที่สุด โดยทำงานอัตโนมัติผ่าน 4 ขั้นตอนหลัก ดังนี้:

#### 1. Data Pipeline (กระบวนการเตรียมข้อมูล)

จุดเริ่มต้นของการรวบรวมและทำความสะอาดข้อมูล เพื่อเตรียมพร้อมสำหรับการสอนโมเดล

* แหล่งที่มาข้อมูล:
  * Cloud Storage: จัดเก็บข้อมูลทั่วไป เช่น เนื้อหาบทความ , หมวดหมู่ และอายุครรภ์ที่เหมาะสมของบทความ
  * Secure Data Center (On-premise): จัดเก็บข้อมูลสุขภาพที่ละเอียดอ่อน เช่น อายุครรภ์จริง และประวัติการอ่าน ข้อมูลส่วนนี้จะถูกส่งผ่านช่องทางที่ปลอดภัย (Fortinet FortiGate และ HA VPN IPSec Tunnel) เท่านั้น
* Preprocessing: ข้อมูลทั้งหมดจะถูกส่งมาพักไว้ที่ Cloud Firestore ก่อนที่ Cloud Run Jobs จะเริ่มทำงาน
  * Data Cleaning: ทำความสะอาดและจัดรูปแบบข้อมูลดิบ
  * Feature Engineering: แปลงข้อมูลให้อยู่ในรูปแบบที่โมเดลใช้งานได้ (Features) และนำไปจัดเก็บไว้ที่ Cloud Firestore (Feature Store) เพื่อรอการนำไปใช้งาน

#### 2. ML Pipeline (กระบวนการเทรนโมเดล)

นำ Features ที่เตรียมไว้มาสอนให้ AI เรียนรู้พฤติกรรมและความสนใจของผู้ใช้งานผ่าน Cloud Run Jobs (Training)

* Item Representation:
  * SBERT Embedding Job: แปลงเนื้อหาบทความ หมวดหมู่ และ อายุครรภ์ที่เหมาะสมของบทความ ให้เป็นชุดตัวเลข (Embedding Vector)
  * MLP Item Tower: รับค่าที่ได้มาบีบอัดขนาดลงจนได้เป็น `item_vector` ซึ่งเป็นตัวแทนความหมายของบทความแต่ละชิ้น
* User Representation: MLP User Tower: นำ อายุครรภ์จริง และประวัติการอ่านมาแปลงเป็น `user_vector` ของผู้ใช้แต่ละคน
* BCE Joint Training: นำ `item_vector` และ `user_vector` มาจับคู่กันเพื่อคำนวณคะแนน (Dot Product) แล้วเทียบกับประวัติการคลิกจริงด้วย BCE Loss เพื่อสอนให้โมเดลรู้ว่า บทความแบบไหนที่ผู้ใช้คนนี้มีแนวโน้มจะสนใจ
* Model Registration: ตัวชี้วัดความแม่นยำ (เช่น Loss, AUC-ROC) จะถูกบันทึกเพื่อวัดผล และตัวโมเดลจะถูกจัดเก็บเวอร์ชันไว้ที่ Vertex AI Model Registry (รองรับการ Rollback หากจำเป็น)
  * `item_vectors` ทั้งหมดของระบบจะถูกทำดัชนีส่งเข้าไปเก็บที่ Vertex AI Vector Search เพื่อเตรียมพร้อมสำหรับการค้นหา

#### 3. Deployment Pipeline (กระบวนการใช้งานจริง)

เมื่อผู้ใช้งานเปิดแอปพลิเคชัน ระบบจะทำการประมวลผลเพื่อดึงบทความมาแสดงผลแบบ Real-time ที่ Cloud Run Backend (Serving)

* User Vector Inference: เมื่อแอปส่ง Request เข้ามาพร้อม `user_id` ระบบจะดึงข้อมูล อายุครรภ์จริง และประวัติการอ่านจาก Data Center มาเข้าโมเดล User Tower เพื่อคำนวณ `user_vector` ล่าสุด ณ ขณะนั้น
* Candidate Retrieval: ส่ง `user_vector` ไปที่ Vertex AI Vector Search เพื่อค้นหาบทความ (ANN Query) ที่มีความใกล้เคียงกับความสนใจที่สุด (Candidate Articles)
* Score and Rank: นำ Candidate ที่ได้มาคำนวณคะแนน Preference Score (จาก `user_vector` x `item_vector`) รวมกับ Time Relevance Score (ความเหมาะสมกับอายุครรภ์ปัจจุบัน) แล้วจัดเรียงลำดับบทความจากคะแนนมากไปน้อย
* Filter Module: คัดกรองบทความที่ผู้ใช้งาน "เคยอ่านไปแล้ว" ออกจากรายการ เพื่อส่งมอบ JSON ผลลัพธ์บทความใหม่ๆ กลับไปที่ Mobile App

#### 4. Monitoring & Feedback Loop (กระบวนการตรวจสอบและเรียนรู้อัตโนมัติ)

ระบบถูกออกแบบให้สามารถเรียนรู้และเก่งขึ้นได้เอง โดยไม่ต้องใช้คนเข้าไปสั่ง Retrain

* Feedback Collection: ทุกครั้งที่ผู้ใช้กดอ่านหรือเลื่อนผ่านบทความ แอปจะส่ง Click Event (`user_id`, `article_id`, สถานะการคลิก) ผ่าน Cloud Audit Logs ไปสะสมเป็น Labeled Data ชุดใหม่ที่ Cloud Firestore (Label Store)
* Drift Detection: ระบบ Cloud Monitoring จะคอยตรวจสอบความแม่นยำของโมเดล หากพบว่าพฤติกรรมผู้ใช้เปลี่ยนไปจนเกินเกณฑ์ที่กำหนด (Data Drift) ระบบจะส่งการแจ้งเตือน
* Auto-Retraining: เมื่อถึงรอบเวลาที่กำหนด หรือได้รับการแจ้งเตือน Drift Cloud Scheduler จะกระตุ้น (Trigger) ให้ระบบดึงข้อมูลพฤติกรรมชุดใหม่ล่าสุดเข้าสู่กระบวนการ ML Pipeline อีกครั้ง ทำให้ Recommendation Feed แม่นยำและอัปเดตอยู่เสมอ

### 2. AI Chatbot

<figure><img src="../.gitbook/assets/ai chatbot (1).png" alt=""><figcaption></figcaption></figure>

ระบบแชทบอทอัจฉริยะนี้ออกแบบมาเพื่อตอบคำถามและให้คำปรึกษาที่อ้างอิงจากข้อมูลทางการแพทย์ที่ถูกต้องและสอดคล้องกับข้อมูลสุขภาพส่วนบุคคลของคุณแม่แต่ละท่าน โดยทำงานผ่าน 4 ขั้นตอนหลัก ดังนี้:

#### 1. Data Pipeline (กระบวนการสร้างและจัดการคลังความรู้)

จุดเริ่มต้นของการรวบรวมและจัดการเอกสารทางการแพทย์ เพื่อสร้างฐานข้อมูล (Vector Store) ที่มีคุณภาพสูงสำหรับให้ AI ใช้อ้างอิง (แชทบอทจะไม่อาศัยการจดจำจากโมเดล แต่จะอ้างอิงจากคลังข้อมูลนี้เป็นหลัก)

* แหล่งที่มาข้อมูล:
  * Cloud Storage: จัดเก็บข้อมูลเอกสารทางการแพทย์ที่ทีมงานรวบรวมมา และบทความทั้งหมดที่มีในแอปพลิเคชัน
  * Secure Data Center (On-premise): จัดเก็บข้อมูลส่วนบุคคลของผู้ใช้งาน (เช่น อายุครรภ์ ประวัติสุขภาพ) ซึ่งจะถูกส่งผ่านช่องทางที่ปลอดภัย (VPN) เสมอ
* Indexing Processing: ข้อมูลจะถูกนำมาเข้ากระบวนการจัดการผ่าน Cloud Run Jobs
  * Chunking Job: ตัดแบ่งเอกสารที่มีความยาวออกเป็นส่วนย่อยๆ (ประมาณ 300-500 Tokens ต่อท่อน) โดยให้เนื้อหามีการเหลื่อมซ้อนกันเล็กน้อย (Overlap) เพื่อรักษาใจความสำคัญไม่ให้ขาดหายระหว่างท่อน
  * Embedding Job: แปลงข้อความแต่ละท่อนที่ตัดแล้วให้เป็นชุดตัวเลข (Vector) และนำไปบันทึก (Upsert) ลงใน Cloud SQL

#### 2. ML Pipeline (กระบวนการควบคุมคุณภาพและจัดการคำสั่ง)

แชทบอทไม่มีการเทรนโมเดลเหมือนระบบแนะนําบทความ แต่จะเน้นไปที่การควบคุมคุณภาพของการดึงข้อมูล และการจัดการชุดคำสั่ง (Prompt) อย่างรัดกุม

* Retrieval Validation:
  * Retrieval QA Test: ทดสอบระบบด้วยชุดคำถามตัวอย่าง เพื่อตรวจสอบว่าระบบสามารถดึงข้อมูลส่วนย่อย (Chunks) ที่ถูกต้องและเกี่ยวข้องกลับมาได้จริงหรือไม่ (หากระบบดึงข้อมูลผิด คำตอบของ AI ก็จะผิดพลาดตามไปด้วย)
* Prompt Management:
  * Prompt Template Versioning: ควบคุมและจัดการเวอร์ชันของ System Prompt ที่ใช้กำหนดบทบาทและข้อจำกัดของ AI
  * Prompt Registry: จัดเก็บเวอร์ชันของ Prompt ไว้ใน Cloud Storage อย่างเป็นระบบ เพื่อรองรับการย้อนกลับ (Rollback) ไปใช้ Prompt เดิมในกรณีที่เวอร์ชันใหม่ให้ผลลัพธ์ที่ไม่ดีพอ

#### 3. Deployment Pipeline (กระบวนการประมวลผลและตอบคำถาม)

เมื่อผู้ใช้งานพิมพ์คำถามเข้ามา ระบบจะทำงานแบบ Real-time ผ่าน Cloud Run Backend (Serving) โดยแบ่งการทำงานออกเป็น 3 ส่วนพร้อมกัน

* Query Embedding: แปลงคำถามของผู้ใช้งานให้เป็น Vector ด้วยโมเดลเดียวกับที่ใช้ในตอน Index เอกสาร เพื่อให้อยู่ในพื้นที่ข้อมูล (Vector Space) เดียวกัน
* RAG Retrieval: นำ Vector ของคำถามไปค้นหาความคล้ายคลึง (Similarity Search) ใน `pgvector` เพื่อดึงข้อมูลชิ้นส่วน (Chunks) ที่มีความเกี่ยวข้องกับคำถามมากที่สุดกลับมา (Top-K)
* Answer Generation:
  * Prompt Builder: นำข้อมูลทั้งหมดมาประกอบเป็น Prompt เดียว โดยมีโครงสร้างชัดเจนคือ: System Prompt (บทบาท AI) -> ข้อมูลส่วนบุคคลของคุณแม่ (เช่น อายุครรภ์ ดึงจาก Data Center) -> บริบททางการแพทย์ (Chunks ที่ค้นเจอ) -> คำถามของผู้ใช้
  * Vertex AI Gemini: รับ Prompt ที่ประกอบเสร็จแล้วไปประมวลผล เพื่อสร้างคำตอบที่ถูกต้องตามหลักการแพทย์และเฉพาะเจาะจงกับสถานการณ์ของคุณแม่ท่านนั้นอย่างแท้จริง

#### 4. Monitoring & Feedback Loop (กระบวนการติดตามผลและพัฒนาอย่างต่อเนื่อง)

ระบบแชทบอทใช้ Feedback Loop สองรูปแบบที่แตกต่างจากระบบแนะนำบทความ เพื่ออัปเดตข้อมูลและพัฒนาความแม่นยำในการตอบคำถาม

* Content Loop (อัปเดตความรู้): เมื่อมีการเพิ่มเอกสารทางการแพทย์หรือบทความใหม่เข้าสู่ Cloud Storage ตัว Cloud Scheduler จะสั่งการ (Trigger) ให้ Cloud Run Indexing Jobs เริ่มหั่นข้อมูลและอัปเดตเข้าสู่ `pgvector` อัตโนมัติ เพื่อให้ AI มีความรู้ที่สดใหม่เสมอ
* Quality Loop (ปรับปรุงคุณภาพคำตอบ):
  * Feedback Collection: ทุกครั้งที่ผู้ใช้กดถูกใจหรือไม่ถูกใจคำตอบ แอปจะส่งข้อมูล Log ผ่าน Cloud Logging ไปสะสมไว้ที่ Cloud Firestore Feedback
  * Prompt Optimization: ทีมงานสามารถเปิดดูประวัติและวิเคราะห์กรณีที่ผู้ใช้รู้สึกไม่พึงพอใจ เพื่อนำข้อมูลไปปรับปรุงเนื้อหาใน Prompt Template (Zone 2) ให้แชทบอทตอบคำถามได้ตรงใจมากขึ้นในอนาคต
