# สิ่งที่จะต้องมีใน Service

เงื่อนไข และความสามารถที่ต้องมี ในการสร้าง service ใหม่ โดยที่จะไม่นับพวกใน core set
<br>

1. [Sync Application](#sync-application)
2. [Refresh Data](#refresh-data)
3. [ระบบจัดการและตรวจสอบ Certificate System Key ](#certificate-system-key)
4. [ระบบจัดการและตรวจสอบ Certificate App Key ](#certificate-app-key)
5. [ระบบHealthCheck](#health-check)
6. [Sync User](#kafka-sync-user)
7. [Sync Label](#kafka-sync-label)
8. [ระบบ UserPolicy และ Permission](#user-policy)

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
### กระบวกการสร้าง Certificate App Key
เป็นระบบที่มีไว้สำหรับ ใช้ในการเข้ารหัสข้อมูลสำคัญที่ไว้คุยกันระหว่าง service แยกในแต่ล่ะ App โดยจะมีขั้นตอนดังนี้

1. Create Application (ไม่จำเป็น ถ้ามีอยู่แล้ว)
2. Add Service to App
    1. เลือก service ที่จะเพิ่มลงใน app ที่ระบุ
    2. ยิง api AddServiceToApp ที่ core service
    3. core ส่งข้อมูลผ่าน kafka ให้ service ที่เพิ่มมา ได้ข้อมูล app และ gen Certificate App Key ของ app นั้นให้ โดยที่ Certificate App Key จะเข้ารหัสผ่าน Certificate System Key 
    
ข้อมูล เป็นดังนี้

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| id     | ID    | id ของ app  |
| serviceKey     | string    | เป็น key ของ service  |
| appKey     | string    | เป็น appKey  |
| appName     | string    | เป็น appName  |
| status     | string    | เป็น status ของ app  |
| credentialDd     | ID    | credential App Id ที่ใช้ในการอ้างอิง  |
| publicKey     | string    | publicKey  |
| privateKey     | string    | privateKey ที่เอาไว้ถอดรหัส โดยจะเข้ารหัสไว้ด้วย Certificate System Key |

3. ทำการ refreshData 

---

### ระบบ sync Certificate App Key

เป็นระบบที่ syncCertificate publicKey Key ของทุกๆ service ใน app นั้นให้กับservice ใน app นั้นทั้งหมด โดยที่ ข้อมูลจะมีดังนี้

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| id     | ID    | id ของ Certificate App Key  |
| status     | string    | เป็น status ของ Certificate App Key  |
| serviceKey     | string    | key ของ service ที่เป็นเจ้าของ |
| appKey     | string    | appKey  |
| publicKey     | string    | เข้ารหัสด้วย publicKey นี้เมื่อจะเข้ารหัส  |
| privateKey     | string    | privateKey ที่เอาไว้ถอดรหัส ของ servicer นั้น โดยจะเข้ารหัสไว้ด้วย Certificate System Key ของ service นั้น|

---

## Health Check

เป็นระบบ ที่เอาไว้ Health Check ว่าตัวเองยังพร้อมใช้ในระบบอยู่




## Kafka Sync User
consume ข้อมูล User
    
    topic: sync-user

### ACTION

#### Add User
รับข้อมูล User เมื่อมีการสร้าง User ใหม่ขึ้นมาในระบบ

    Action: ADD

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| email     | EmailSchema    | email  |
| phone     | PhoneSchema    | phone  |
| username     | string    | username  |
| facebookId     | string    | facebookId  |
| googleId     | string    | googleId  |
| lineId     | string    | lineId  |
| appleId     | string    | appleId  |

---

#### Update User

รับข้อมูล User เมื่อมีการ update User

    Action: UPDATE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| email     | EmailSchema    | email  |
| phone     | PhoneSchema    | phone  |
| username     | string    | username  |
| facebookId     | string    | facebookId  |
| googleId     | string    | googleId  |
| lineId     | string    | lineId  |
| appleId     | string    | appleId  |

---

#### Delete User

รับข้อมูล User เมื่อมีการ DELETE User 

    Action: DELETE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |

---

<br>
<br>

## Kafka Sync Label
consume ข้อมูล Label
    
    topic: sync-label

### ACTION

#### Add Label
รับข้อมูล Label เมื่อมีการสร้าง Label ใหม่ขึ้นมาในระบบ

    Action: ADD

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| name     | String    | ชื่อ label ที่สร้าง  |
| description     | String    | คำอธิบาย  |

---

#### Update Label

รับข้อมูล Label เมื่อมีการ update Label

    Action: UPDATE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| name     | String    | ชื่อ label ที่สร้าง  |
| description     | String    | คำอธิบาย  |

---

#### Delete Label

รับข้อมูล Label เมื่อมีการลบ Label

    Action: DELETE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| name     | String    | ชื่อ label ที่สร้าง  |
| description     | String    | คำอธิบาย  |

---

<br>
<br>

## User Policy

---
เป็นระบบที่เอาไว้จัดการเครื่อง สิทธิ และ Permission ในระบบ
โดยที่ ACL service จะเป็น service ที่จัดการ Permission แล้ว sync User Policy (ได้แรงบันดาใจจาก AWS)ที่เป็นของพร้อมใช้มา

ข้อมูล Permission ที่ต้องส่งให้ ACL service ผ่าน kafka ดังนี้

    topic: sync-permission

โดยข้อมูลเป็นดังนี้

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| serviceKey     | string    | serviceKey ของ service |
| permissions     | Array< permissionSchema >    | array ของ permission ที่มีใน service ทั้งหมด |

โดยที่ permissionSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| key     | string    | key ของ permission ห้ามซ้ำ |
| name    | string    | key ของ permission |
| description    | string    | description ของ permission |

ซึ้้งเมื่อ ทำการส่ง sync-permission ไปแล้ว ACL service จะทำการ gen user Policy ของ user มาในรูปแบบ 

    policyKey: userId + ':' + LABEL + ':' +permissionKey

โดยจะส่งผ่าน kafka ดังนี้

    topic: sync-user-policy

โดยข้อมูลเป็นดังนี้

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| permissionKey     | string    | permissionKey ใช้สำหรับการจัดการเรื่องเพิ่มลบ permission |
| policyKey     | string    | เป็น Key ที่ตรงตามรูปแบบ ใช้ในการค้นหา |
| userId     | string    | userId ใช้ในการจัดการเพิ่มลบข้อมูล user  |

และมี action ดังนี้

| actionKey     |   ข้อมูลที่เพิ่มไปใน header (ถ้ามี)    |  คำอธิบาย     |
| ------  | ------    | ------       |
| ADD     |     | ใช้เพิ่ม data ตามที่ระบุ |
| REMOVE     |     | ใช้ลบ policyKey ตามที่ระบุ |
| REMOVE-PERMSSION    |  permissionKey: string   | ลบ user Policy ทั้งหมดที่มี permissionKey ตามที่ระบุ |
| REMOVE-USER    |  userId: string   | ลบ user Policy ทั้งหมดที่มี userId ตามที่ระบุ |
เป็น ระบบ ในการ แลก Key แบบ RSA แบบ SHA256
โดยที่ ตัว service จะเก็บ publicKey , privateKey ที่ไว้ใช้คุยกับ core service ไว้ โดยที่มีกระบวนการดังนี้


