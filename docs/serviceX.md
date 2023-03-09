# สิ่งที่จะต้องมีใน Service

เงื่อนไข และความสามารถที่ต้องมี ในการสร้าง service ใหม่ โดยที่จะไม่นับพวกใน core set
<br>

1. [Sync Application](#sync-application)
2. [Refresh Data](#refresh-data)
3. [ระบบจัดการและตรวจสอบ Certificate System Key ](#certificate-system-key)
4. [ระบบจัดการและตรวจสอบ Certificate App Key ](#certificate-app-key)
5. [ระบบHealthCheck](#kafka-produce-reference)
6. [Sync User](#kafka-consume-user)
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

ให้ทำการ ส่งข้อมูลตัวเอง ที่จำเป็นต้อง Produce ขึ้น kafka ใหม่ทั้งหมดใน app นั้น


## Certificate System Key
---

เป็น ระบบ ในการ แลก Key แบบ RSA แบบ SHA256
โดยที่ ตัว service จะเก็บ publicKey , privateKey ที่ไว้ใช้คุยกับ core service ไว้ โดยที่มีกระบวนการดังนี้


1. ยิง API "Register Service to Core Syetem" ไปที่ core service โดยจะส่งข้อมูลดังนี้

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| serviceKey     | string    | เป็น key ของ service  |
| serviceName     | string    | ตั้งชื่อเพื่อใช้ในการแสดงผล  |

โดยจะได้ผลลัพธ์เป็นดังนี้

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| serviceKey     | string    | เป็น key ของ service  |
| serviceName     | string    | ตั้งชื่อเพื่อใช้ในการแสดงผล  |
| id     | ID    | credential Id ที่ใช้ในการอ้างอิง  |
| publicKey     | string    | publicKey ที่เอาไว้ใช้เข้ารหัสเพื่อคุยกับ core service (key ที่เอาไว้เข้ารหัส)  |
| privateKey     | string    | privateKey ที่เอาไว้ถอดรหัส เวลาคุยข้อมูลกับ core service (key เอาไว้ถอดรหัส ห้ามหลุด!!!)  |

2. เดพ เอา id และ publicKey , privateKey ไปใส่ใน env ที่จะทำงาน และ run service
3. service เมื่อ run ให้ทำการเข้ารหัส credential Id ด้วย publicKey (เช็กว่า publicKey ถูกต้อง) ขึ้น kafka ดังนี้

    topic: register-service

ข้อมูล เป็นดังนี้

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| id     | ID    | credential Id ที่ใช้ในการอ้างอิง  |
| serviceKey     | string    | เป็น key ของ service  |
| encryptdata     | string    | เข้ารหัส id ด้วย publicKey |

4. core service ทำการรับ register-service แล้วเริ่มทำการ Haud Check เพื่อเช็กว่า ระบบสามารถถอดรหัสข้อมูลได้ถูกต้อง 
5. core service ทำการส่ง ข้อมูลขึ้น kafka ดังนี้

    topic: haud-check

ข้อมูล เป็นดังนี้

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| id     | ID    | credential Id ที่ใช้ในการอ้างอิง  |
| serviceKey     | string    | เป็น key ของ service  |
| refId     | ID    | เป็น Id ที่ core gen ขึ้นเพื่อใช้อ้างอิง การ haud-check|
| encryptData     | string    | เข้ารหัส refId ด้วย publicKey |

6. service รับ kafka topic: haud-check มาแล้ว ส่งข้อมูล ให้ทำการถอด encryptdata ด้วย privateKey แล้วนำค่าที่ได้ใส่ resultData แล้ว ทำการส่ง ข้อมูลขึ้น kafka ดังนี้

    topic: haud-check-result

ข้อมูล เป็นดังนี้

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| id     | ID    | credential Id ที่ใช้ในการอ้างอิง  |
| serviceKey     | string    | เป็น key ของ service  |
| refId     | ID    | เป็น Id ที่ core gen ขึ้นเพื่อใช้อ้างอิง การ haud-check|
| encryptData     | string    | เป็นข้อมูลเข้ารหัสที่ส่งมาจาก core |
| resultData     | string    | ผลลัพธ์การถอดรหัส ถ้าไม่สามารถถอดได้ ใส่ตอบกลับเป็น "" |

7. core service รับผลลัพธ์ topic: haud-check-result แล้วเทียบค่า refId กับ resultData แล้วsave ผลลัพธ์ลง database


## Certificate App Key
---

เป็น ระบบ ในการ แลก Key แบบ RSA แบบ SHA256
โดยที่ ตัว service จะเก็บ publicKey , privateKey ที่ไว้ใช้คุยกับ core service ไว้ โดยที่มีกระบวนการดังนี้