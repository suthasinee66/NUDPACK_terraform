# Deploy Web Application บน AWS ด้วย Terraform

## ภาพรวมโครงการ

โครงการนี้ออกแบบเพื่อ **Deploy แอปพลิเคชันเว็บ** ที่พัฒนาโดยใช้ **Python FastAPI** พร้อม **HTML Frontend** บน **AWS EC2** โดยใช้ **Terraform** ในการจัดการ Infrastructure แบบอัตโนมัติ  

ระบบสามารถทำงานดังนี้:

- สร้าง EC2 Instance สำหรับรัน Application  
- สร้าง Security Group และ SSH Key Pair  
- Clone Source Code จาก GitHub  
- ติดตั้ง Dependencies ของ Python  
- รัน FastAPI Server  
- เชื่อมต่อกับฐานข้อมูล **Neon PostgreSQL**  

---

## โครงสร้างโปรเจค


```
NUDPACK/
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── terraform.tfstate
│   └── terraform.tfstate.backup
│
├── deploy.sh
└── README.md
```

**ไฟล์สำคัญ**

- **main.tf** – กำหนด Resource บน AWS เช่น EC2, Security Group, Key Pair  
- **variables.tf** – กำหนดค่าตัวแปร เช่น Region, Instance Type  
- **outputs.tf** – แสดงผลลัพธ์ เช่น Public URL ของ Application  

---

## เทคโนโลยีที่ใช้

- Terraform  
- AWS EC2  
- Python + FastAPI  
- Uvicorn  
- PostgreSQL (Neon)  
- Git / GitHub  

---

## ขั้นตอนการใช้งาน

### 1. ติดตั้งเครื่องมือพื้นฐาน

ติดตั้งโปรแกรมต่อไปนี้:

- Terraform  
- AWS CLI  
- Git  
- Node.js / NPM (npm install)  

ตรวจสอบเวอร์ชัน:

```bash
terraform -version
aws --version
git --version
node -v
npm -v

# 2. Login AWS CLI

ก่อนใช้งาน Terraform ต้อง Login AWS CLI ก่อน

ใช้คำสั่ง

```
aws configure
```

จากนั้นกรอกข้อมูล

```
AWS Access Key ID
AWS Secret Access Key
Region (เช่น ap-southeast-1)
Output format (json)
```

---

# 3. Clone Repository

```
git clone <repository-url>
cd NUDPACK/terraform
```

---

# 4. Initialize Terraform

ใช้คำสั่ง

```
terraform init
```

Terraform จะทำการติดตั้ง Provider และเตรียม Environment สำหรับการทำงาน

---

# 5. ตรวจสอบ Infrastructure Plan

ใช้คำสั่ง

```
terraform plan
```

คำสั่งนี้จะแสดงรายการ Resource ที่ Terraform จะสร้างบน AWS

เช่น

* EC2 Instance
* Security Group
* Key Pair

---

# 6. Deploy Infrastructure และ Application

ใช้คำสั่ง

```
terraform apply
```

เมื่อระบบถามยืนยัน ให้พิมพ์

```
yes
```

Terraform จะทำการ

1. สร้าง EC2 Instance บน AWS
2. สร้าง Security Group
3. Clone Source Code จาก GitHub
4. ติดตั้ง Python และ Dependencies
5. รัน FastAPI Server

หลังจาก Deploy สำเร็จ Terraform จะแสดง URL สำหรับเข้าใช้งานระบบ เช่น

```
app_public_url = http://<EC2_PUBLIC_IP>:8000
```

สามารถเปิด URL นี้ใน Web Browser เพื่อใช้งานระบบได้

---

# 7. ตรวจสอบการทำงานของระบบ

เปิด Web Browser และเข้าไปที่


http://<EC2_PUBLIC_IP>:8000/admin การเข้าใช้งานหน้า เจ้าหน้าที่
http://<EC2_PUBLIC_IP>:8000/client การเข้าใช้งานหน้า พนักงานขนส่ง
http://<EC2_PUBLIC_IP>:8000/recipient การเข้าใช้งานหน้า ผู้รับพัสดุ


ระบบจะทำการเชื่อมต่อกับฐานข้อมูล Neon PostgreSQL และสามารถใช้งานระบบ Login และฟังก์ชันต่าง ๆ ได้

---

# 8. การลบ Infrastructure

หลังจากใช้งานเสร็จ ควรลบ Infrastructure เพื่อป้องกันค่าใช้จ่ายจาก Cloud

ใช้คำสั่ง

```
terraform destroy
```

เมื่อระบบถามยืนยัน ให้พิมพ์

```
yes
```

Terraform จะทำการลบ Resource ทั้งหมดที่สร้างไว้ เช่น

* EC2 Instance
* Security Group
* Key Pair

---

# สรุปการทำงานของระบบ

ขั้นตอนการทำงานของระบบมีดังนี้

```
Terraform
   │
   ▼
AWS EC2 Instance
   │
   ├── Install Python
   ├── Clone GitHub Repository
   ├── Install Dependencies
   └── Run FastAPI Server
            │
            ▼
      Neon PostgreSQL Database
```

ผู้ใช้สามารถเข้าใช้งานระบบผ่าน Web Browser โดยเชื่อมต่อไปยัง EC2 Instance ที่ถูกสร้างโดย Terraform

---

# หมายเหตุ

ก่อนการใช้งาน Terraform จำเป็นต้อง Login AWS CLI ก่อนทุกครั้ง หากยังไม่ได้ตั้งค่า AWS CLI ระบบจะไม่สามารถสร้าง Infrastructure บน AWS ได้
