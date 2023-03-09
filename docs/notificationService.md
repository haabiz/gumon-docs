# notification Service

Service สำหรับจัดการเกี่ยว notification ต่างๆโดยใช้วิธีการ subscription ของ GraphQL


<br>

- [API Reference](#api-reference)
- [CLI Reference](#cli-reference)
- [kafka consume Reference](#kafka-consume-reference)
- [kafka produce Reference](#kafka-produce-reference)

---

<br>
<br>

## API Reference
---

### Query

เป็น API ที่ใช้สำหรับการ Query ข้อมูลออกมา ไม่มีการแก้ไข Data

#### Get Notification

API สำหรับเรียกดูข้อมูล notification โดย Api นี้จะเป็นแบบ realtime API หากมีข้อมูลสร้างขึ้นมาใหม่ Api นี้จะนำข้อมูลนั้นออกมาแสดงโดยอัตโนมัติ

    API name : getNotification

Input Fields

Response : notificationSchema[]

notificationSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| topic     | String    | หัวข้อ notification  |
| content     | String    | เนื้อหา  |

#### Get Notification By ID

API สำหรับเรียกดูข้อมูล notification ตาม ID ที่รับมา

    API name : getNotificationByID

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |

Response : notificationSchema

notificationSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| topic     | String    | หัวข้อ notification  |
| content     | String    | เนื้อหา  |

---

### Mutation

เป็น API ที่ใช้สำหรับการแก้ไขข้อมูล

#### Create Notification

API สำหรับการสร้าง notification หลังจากการบันทึกข้อมูล จะทำการส่งข้อมูลไปยัง TOPIC ที่กำหนดไว้ตามฟังก์ชันของ subscription

    API name : createNotification

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| topic     | String    | หัวข้อ notification  |
| content     | String    | เนื้อหา  |

Response : notificationSchema

notificationSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| topic     | String    | หัวข้อ notification  |
| content     | String    | เนื้อหา  |

---

## kafka consume Reference

### Application
consume ข้อมูล application
    
    topic: sync-application

---

#### Add Application

รับข้อมูล Application เมื่อมีการสร้าง Application ใหม่ขึ้นมาในระบบ

    Action: ADD

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| name     | string    | ชื่อ app  |
| attribute     | object    |   |

---

#### Update Application

รับข้อมูล Application เมื่อมีการ update Application

    Action: UPDATE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| name     | string    | ชื่อ app  |
| attribute     | object    |   |

---

#### Delete Application

รับข้อมูล Application เมื่อมีการ DELETE Application 

    Action: DELETE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |

---

#### RefreshData

เมื่อมีคำสั่งนี้มา ให้ทำการส่งข้อมูลของตัวเอง อัตเดตขึ้น kafka

    Action: REFRESHDATA

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |

---

### Notification
consume ข้อมูลที่ต้องการใช้งาน noification
    
    topic: notification

---
#### Notification

เมื่อมีคำสั่งนี้มา ให้ทำการสร้างข้อมูล notification โดยไม่จำเป็นต้องผ่าน API "createNotification"

    Action: CREATE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| topic     | String    | หัวข้อ notification  |
| content     | String    | เนื้อหา  |
| type     | ENUM(NOTIFICATION_TYPE) | [NOMAL, EMAIL, SMS] ประเภท notification ที่ต้องการส่ง

---

## Kafka Produce Reference
