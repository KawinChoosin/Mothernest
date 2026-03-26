---
icon: gear-code
---

# CI/CD and DevSecOps

<figure><img src="../.gitbook/assets/diagram-export-3-26-2026-9_14_41-PM (1).png" alt=""><figcaption></figcaption></figure>

กระบวนการนำส่งซอฟต์แวร์ (Software Delivery) ของโครงการ MotherNest ถูกออกแบบภายใต้มาตรฐาน DevSecOps เพื่อรับประกันความปลอดภัยของระบบ Health-Tech และความสามารถในการทำงาน Hybrid Cloud ระหว่างระบบ On-Premise และ GCP โดยกระบวนการทั้งหมดถูกควบคุมผ่าน Cloud Build แบ่งออกเป็น 3 ระยะหลัก:

1\. Continuous Integration & Security Pipeline (กระบวนการผสานโค้ดและตรวจสอบความปลอดภัย)

* เมื่อวิศวกรทำการ Push โค้ดไปยัง GitHub จะเกิด Webhook ไปกระตุ้นการทำงานของ Cloud Build โดยอัตโนมัติ
* ระบบจะดึงข้อมูลที่ละเอียดอ่อน (เช่น API Keys) จาก Secret Manager อย่างปลอดภัย และเริ่มทำ SAST (Static Application Security Testing) รวมถึงการทำ Linting เพื่อตรวจจับข้อผิดพลาดและช่องโหว่ของโค้ดตั้งแต่เนิ่นๆ (Shift-left Security)
* เมื่อผ่านการทดสอบ โค้ดจะถูกสร้างเป็น Docker Image และจัดเก็บที่ Artifact Registry ซึ่งมีระบบสแกนหาช่องโหว่ระดับคอนเทนเนอร์ (CVEs Vulnerability Scanner) ทำงานเป็นด่านสุดท้าย

2\. Staging Deployment (การนำขึ้นระบบทดสอบบน On-Premise)

* หากการอัปเดตเกิดขึ้นใน `staging branch` ระบบจะวิ่งผ่าน HA VPN Tunnel (IPsec) เข้าสู่ Datacenter ขององค์กร
* สคริปต์ DB Schema Migration จะทำงานเพื่อปรับปรุงโครงสร้างฐานข้อมูลจำลอง ก่อนที่ระบบจะนำ Image ไปรันบน Kubernetes Cluster
* หลังจาก Deploy สำเร็จ ระบบจะรันชุดทดสอบ Automated UAT แบบ End-to-End เพื่อยืนยันความถูกต้อง และส่งผลการทดสอบแจ้งเตือนไปยังทีมพัฒนาผ่าน Pub/Sub (Slack/Discord)

3\. Production Deployment & Canary Release (การนำขึ้นระบบจริงบน GCP)

* เมื่อได้รับการยืนยันโค้ดลง `main branch` Cloud Build จะดึง Production Image มาติดตั้งลงบน Serverless Services ทั้ง 3 ส่วน ได้แก่ RAG Orchestrator, RecSys Engine, และ Cloud Run Jobs (Batch)
* Traffic Splitting & Canary Deploy: สำหรับบริการที่กระทบผู้ใช้โดยตรง (RAG และ RecSys) ระบบจะใช้กลยุทธ์ Canary Release โดยแบ่งทราฟฟิกเพียง 10% ไปยังเวอร์ชันใหม่
* Automated Rollback: Cloud Monitoring จะทำหน้าที่ตรวจจับ Error Rate และ Latency ของเวอร์ชันใหม่ หากพบความผิดปกติ ระบบจะทำการย้ายทราฟฟิก 100% กลับไปยังเวอร์ชันเก่าโดยอัตโนมัติ (Rollback) เพื่อป้องกันไม่ให้ระบบล่มหรือ AI ตอบคำถามผิดพลาดเป็นวงกว้าง

#### 💡 การตัดสินใจเชิงวิศวกรรมสถาปัตยกรรม (Architectural Justifications)

ในการออกแบบระบบ CI/CD สำหรับโครงการ MotherNest ซึ่งเป็นแอปพลิเคชันที่จัดการกับข้อมูลสุขภาพ (Health-Tech) โครงการได้ตัดสินใจเชิงวิศวกรรมเพื่อรักษาสมดุลระหว่าง "ความรวดเร็วในการส่งมอบซอฟต์แวร์ (Agility)" และ "ความปลอดภัยสูงสุด (Maximum Security)" ดังนี้:

1\. กลยุทธ์ความปลอดภัยแบบเลื่อนซ้าย (Shift-Left Security & DevSecOps)

* เหตุผล: ในระบบที่จัดการกับข้อมูลส่วนบุคคล (PDPA) ความปลอดภัยไม่สามารถรอไปตรวจในขั้นตอนสุดท้ายได้ โครงการจึงตัดสินใจผนวกการสแกนโค้ด (SAST) และการสแกนช่องโหว่ของคอนเทนเนอร์ (Vulnerability Scanning บน Artifact Registry) เข้าไปใน CI Pipeline โดยตรง
* ผลลัพธ์: การตรวจจับช่องโหว่ระดับ OS และ Dependencies ตั้งแต่ขั้นตอนการ Build ช่วยลดความเสี่ยงที่ระบบจะถูกโจมตีแบบ Zero-day vulnerability และป้องกันไม่ให้โค้ดที่มีข้อผิดพลาดหลุดไปถึง Production อย่างเด็ดขาด

2\. การจำกัดรัศมีความเสียหายด้วย Canary Release (Blast Radius Reduction)

* เหตุผล: ระบบ AI (RAG และ RecSys) มีความเสี่ยงที่โมเดลจะให้คำตอบที่ผิดปกติ (Hallucination) หรือเกิดข้อผิดพลาดจาก Logic ใหม่ๆ การเปลี่ยนทราฟฟิก 100% ทันที (Big Bang Deployment) ถือว่ามีความเสี่ยงสูงเกินไปสำหรับแอปพลิเคชันสุขภาพ
* ผลลัพธ์: การใช้ Canary Deployment (10%) บน Cloud Run ร่วมกับ Automated Rollback ผ่าน Cloud Monitoring ทำให้หากเกิดข้อผิดพลาด จะมีผู้ใช้งานเพียง 10% เท่านั้นที่ได้รับผลกระทบ และระบบสามารถดึงเวอร์ชันเก่ากลับมาให้บริการต่อได้ทันทีโดยไม่ต้องรอวิศวกรสั่งการ (Zero-Downtime)

3\. การบริหารจัดการสภาพแวดล้อมแบบแยกส่วน (Hybrid Environment Isolation)

* เหตุผล: โครงการต้องการทดสอบระบบในสภาพแวดล้อมที่ใกล้เคียงความจริงที่สุด แต่ไม่ต้องการนำข้อมูลทดสอบไปปะปนกับฐานข้อมูลหลักบน GCP
* ผลลัพธ์: การออกแบบให้ `staging branch` นำไป Deploy บน On-Premise Kubernetes Cluster ผ่าน VPN ช่วยให้ทีม QA สามารถจำลองการยิง API และทดสอบโมเดล AI กับฐานข้อมูลจำลอง (Staging DB) ได้อย่างเต็มที่โดยไม่มีค่าใช้จ่ายคลาวด์เพิ่มเติม และเมื่อโค้ดเสถียรแล้วจึงนำไป Deploy จริงที่ฝั่ง GCP (Production) เป็นการใช้ประโยชน์จาก Hybrid Cloud อย่างคุ้มค่าที่สุด

4\. การจัดการความลับแบบรวมศูนย์ (Centralized Secret Management)

* เหตุผล: การเก็บ API Keys (เช่น เครดิต Vertex AI) หรือรหัสผ่านฐานข้อมูลไว้ในซอร์สโค้ด (Hardcoding) หรือ Environment Variables แบบปกติ เป็นช่องโหว่ความปลอดภัยที่ร้ายแรง
* ผลลัพธ์: โครงการบังคับให้ Cloud Build ดึงข้อมูลที่ละเอียดอ่อนจาก Google Secret Manager เท่านั้น ทำให้ทีมพัฒนาสามารถหมุนเวียนรหัสผ่าน (Key Rotation) ได้ตลอดเวลาโดยไม่ต้องแก้โค้ด และไม่มีใครสามารถเข้าถึงรหัสผ่านฐานข้อมูล Production ได้โดยตรง
