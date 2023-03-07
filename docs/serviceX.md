# สิ่งที่จะต้องมีใน Service

เงื่อนไข และความสามารถที่ต้องมี ในการสร้าง service ใหม่ โดยที่จะไม่นับพวกใน core set
<br>

1. [Sync Application](#sync-application)
2. [Refresh Data](#refresh-data)
3. [ระบบจัดการและตรวจสอบ Certificate System Key ](#certificate-system-key)
4. [ระบบจัดการและตรวจสอบ Certificate App Key ](#certificate-app-key)
5. [ระบบHealthCheck](#kafka-produc-reference)
6. [Sync User](#api-reference)
7. [Sync Label](#api-reference)
8. [ระบบ UserPolicy และ Permission](#api-reference)

---

<br>
<br>

## Sync Application
---

เป็นระบบที่รับการ sync ข้อมูลของ Application จาก Application Service จาก kafka

    topic: sync-application

โดยมีความสามารถดังนี้

---
### Add Application

รับข้อมูล Application เมื่อมีการสร้าง Application ใหม่ขึ้นมาในระบบ

    Action: ADD

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| name     | string    | ชื่อ app  |
| attribute     | object    |   |

---

### Update Application

รับข้อมูล Application เมื่อมีการ update Application

    Action: UPDATE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| name     | string    | ชื่อ app  |
| attribute     | object    |   |

---

### Delete Application

รับข้อมูล Application เมื่อมีการ DELETE Application 

    Action: DELETE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |

---

## Refresh Data
---

เป็นระบบที่เมื่อ ได้รับคำสั่ง kafka 

    topic: sync-application
    Action: REFRESHDATA

ข้อมูล เป็นดังนี้

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |

ให้ทำการ ส่งข้อมูลตัวเอง ที่จำเป็นต้อง Produc ขึ้น kafka ใหม่ทั้งหมดใน app นั้น


## Certificate System Key
---

เป็น ระบบ ในการ แลก Key แบบ RSA แบบ SHA256
โดยที่ ตัว service จะเก็บpublicKey , privateKey ที่ไว้ใช้คุยกับ core service ไว้ โดยที่มีกระบวนการดังนี้

1. ยิง API "Register Service to Core Syetem" ไปที่ core service