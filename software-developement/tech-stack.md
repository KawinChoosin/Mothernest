---
icon: layer-group
---

# Tech Stack

### Overview

ระบบ MotherNest ถูกพัฒนาด้วยสถาปัตยกรรมแบบแยกส่วนระหว่าง Frontend, Backend, Database และ Deployment เพื่อให้ดูแลง่าย ขยายต่อได้ และรองรับการพัฒนาเป็นทีม

Tech stack หลักของระบบนี้ประกอบด้วย

* Frontend: `Next.js`
* Backend: `Nest.js`
* Containerization: `Docker`
* ORM: `Prisma ORM`
* Database: `PostgreSQL`
* Notification: `LINE Notification`

{% hint style="info" %}
แนวทางนี้ช่วยให้ระบบพัฒนาได้เร็วในช่วงเริ่มต้น แต่ยังรองรับการขยายฟีเจอร์ในอนาคต เช่น AI, real-time service, notification workflow และการแยก service เพิ่มเติม
{% endhint %}



### Frontend: Next.j

<figure><img src="../.gitbook/assets/image (17).png" alt="" width="563"><figcaption></figcaption></figure>

`Next.js` ถูกใช้สำหรับพัฒนา Web Application ของระบบ

#### เหตุผลที่เลือกใช้

* รองรับการพัฒนา UI แบบ component-based
* จัดการ routing ได้เป็นระบบ
* เหมาะกับการทำ Web Application ที่ต้องมีหลายหน้า
* รองรับการเชื่อมต่อ API ได้สะดวก
* ดูแลโครงสร้างโปรเจกต์ได้ง่ายเมื่อทีมพัฒนาขยายขึ้น

#### หน้าที่ในระบบ

* แสดงหน้าเว็บสำหรับผู้ใช้งาน
* จัดการแบบฟอร์มและการโต้ตอบกับผู้ใช้
* เชื่อมต่อกับ Backend API
* แสดงข้อมูลสุขภาพ คอนเทนต์ และสถานะการใช้งานต่าง ๆ

### Backend: Nest.js

<figure><img src="../.gitbook/assets/image (18).png" alt="" width="563"><figcaption></figcaption></figure>

`Nest.js` ถูกใช้เป็นแกนหลักของฝั่งเซิร์ฟเวอร์

#### เหตุผลที่เลือกใช้

* โครงสร้างชัดเจน เหมาะกับระบบขนาดกลางถึงใหญ่
* รองรับการแยก module ตามโดเมนงาน
* ใช้ `TypeScript` เป็นหลัก ทำให้ดูแลโค้ดง่าย
* เหมาะกับการพัฒนา API ที่มี business logic หลายชั้น
* ขยายต่อไปสู่ microservices ได้หากระบบเติบโต

#### หน้าที่ในระบบ

* ให้บริการ REST API
* จัดการ authentication และ authorization
* ประมวลผล business logic
* เชื่อมต่อฐานข้อมูลผ่าน Prisma ORM
* เรียกใช้งานระบบแจ้งเตือนผ่าน LINE

### Database Access Layer: Prisma ORM

<figure><img src="../.gitbook/assets/image (19).png" alt="" width="563"><figcaption></figcaption></figure>

`Prisma ORM` ใช้เป็นตัวกลางระหว่าง Backend และฐานข้อมูล

#### เหตุผลที่เลือกใช้

* เขียน query ได้ชัดเจนและปลอดภัยขึ้น
* ลดความซับซ้อนของการจัดการ SQL โดยตรง
* รองรับ schema migration ได้ดี
* ทำงานร่วมกับ `TypeScript` ได้เหมาะสม
* ช่วยให้ model ของข้อมูลสอดคล้องกับฝั่งแอป

#### หน้าที่ในระบบ

* จัดการ schema ของฐานข้อมูล
* ใช้ query ข้อมูลจาก PostgreSQL
* จัดการ migration และ version ของโครงสร้างข้อมูล
* ลด error จากการเข้าถึงข้อมูลแบบไม่ตรงชนิด

### Database: PostgreSQL

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

`PostgreSQL` เป็นฐานข้อมูลหลักของระบบ

#### เหตุผลที่เลือกใช้

* เป็น relational database ที่เสถียร
* รองรับข้อมูลที่มีความสัมพันธ์ซับซ้อน
* เหมาะกับระบบที่ต้องการความถูกต้องของข้อมูลสูง
* รองรับ transaction และ constraint ได้ครบ
* ขยายต่อได้ดีเมื่อข้อมูลเพิ่มขึ้น

#### ตัวอย่างข้อมูลที่จัดเก็บ

* ข้อมูลผู้ใช้งาน
* ประวัติการตั้งครรภ์และข้อมูลเด็ก
* ข้อมูลนัดหมายและการปรึกษา
* บทความและคอนเทนต์ในระบบ
* ประวัติการแจ้งเตือน

### Containerization: Docker

<figure><img src="../.gitbook/assets/image (22).png" alt="" width="563"><figcaption></figcaption></figure>

`Docker` ใช้สำหรับจัดการ environment ของระบบให้สม่ำเสมอ

#### เหตุผลที่เลือกใช้

* ลดปัญหา environment ไม่ตรงกันระหว่างเครื่อง
* ทำให้ deployment ง่ายขึ้น
* ช่วยแยก service ออกจากกันชัดเจน
* เหมาะกับการพัฒนาและทดสอบแบบทีม
* รองรับการย้ายระบบไปยัง server หรือ cloud ได้สะดวก

#### การใช้งานในระบบ

* รัน frontend และ backend ใน container
* รันฐานข้อมูลใน container สำหรับ development หรือ testing
* จัดการ dependency ให้พร้อมใช้งานในทุก environment

### Notification: LINE Notification

<figure><img src="../.gitbook/assets/image (23).png" alt="" width="509"><figcaption></figcaption></figure>

ระบบใช้ `LINE Notification` เป็นหนึ่งในช่องทางแจ้งเตือนหลัก

#### เหตุผลที่เลือกใช้

* ผู้ใช้ในบริบทของระบบคุ้นเคยกับ LINE อยู่แล้ว
* เข้าถึงผู้ใช้งานได้รวดเร็ว
* เหมาะกับการแจ้งเตือนที่ต้องการให้เห็นทันที
* ลดแรงเสียดทานในการรับข้อมูลสำคัญจากระบบ

#### ตัวอย่างการใช้งาน

* แจ้งเตือนนัดหมาย
* แจ้งเตือนกิจกรรมสำคัญ
* แจ้งเตือนเกี่ยวกับสุขภาพหรือกำหนดการ
* แจ้งเตือนสถานะบางอย่างของบัญชีผู้ใช้

### จุดเด่นของชุดเทคโนโลยีนี้

การเลือกใช้ stack นี้มีข้อดีหลักดังนี้

* ใช้ `TypeScript` ได้ทั้งฝั่ง Frontend และ Backend
* โครงสร้างระบบชัดเจน แยกหน้าที่แต่ละส่วนได้ดี
* ดูแลง่ายเมื่อฟีเจอร์เพิ่มขึ้น
* รองรับการทำงานร่วมกับฐานข้อมูลเชิงสัมพันธ์ได้เหมาะสม
* พร้อมต่อยอดสู่ production deployment ได้

### สรุป

ระบบนี้ใช้ `Next.js` สำหรับพัฒนา frontend, `Nest.js` สำหรับ backend, `Prisma ORM` สำหรับจัดการการเข้าถึงข้อมูล, `PostgreSQL` สำหรับฐานข้อมูล, `Docker` สำหรับจัดการ deployment environment และ `LINE Notification` สำหรับการแจ้งเตือนผู้ใช้งาน

ชุดเทคโนโลยีนี้เหมาะกับระบบที่ต้องการทั้งความยืดหยุ่น ความเป็นระเบียบของโครงสร้าง และความพร้อมในการขยายต่อในอนาคต
