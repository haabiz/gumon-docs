# Storage Service

Service สำหรับจัดการข้อมูล storage ต่างๆ ซึ้งมีทั้ง

1. Organization
2. Business Unit
3. Position
4. Role
5. Custom
 
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

#### Get Storage

API สำหรับการเรียกดูรายการข้อมูล storage

    API name : getStorages


Response : storageSchema[]

storageSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| acl     | enum    | PRIVATE, PUBLIC (สิทธิ์ในการเข้าถึงข้อมูลของไฟล์)  |
| path     | string    | category ที่ต้องการเก็บข้อมูลใน S3  |
| signedUrl     | String    | URL ที่ได้ทำการ signed ที่ S3 เอาไว้ (ใช้ในการ upload ไฟล์ที่ได้ทำการ signed เอาไว้)  |
| publicUrl     | String    | url ของไฟล์ที่ทำการบันทึกไว้ที่ s3 (กรณี acl เป็น private จะไม่สามารถดูข้อมูลได้ หากต้องการดูข้อมูล ต้องเรียกใช้ api [Get Object SignedURL](#get-object-signedurl))  |
| filename     | String    | ชื่อของไฟล์ที่ทำการ upload  |
| fileKey     | String    | key ของ ไฟล์ที่ได้จากการ signed ที่ s3  |
| userID     | String    | id ของ user ที่ทำการ upload  |
| expired     | int    | ระยะเวลาหมดอายุการ signed (max 604800 = 7 days)  |
| status     | enum    | UNKNOW, SUCCESS, FAILED (สถานะในการ upload)  |

---

#### Get Storage By ID

API สำหรับการเรียกดูข้อมูล Storage โดยอ้างอิงจาก ID

    API name : getStorageByID

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | String    | ID ของ storage |


Response : storageSchema

storageSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| acl     | enum    | PRIVATE, PUBLIC (สิทธิ์ในการเข้าถึงข้อมูลของไฟล์)  |
| path     | string    | category ที่ต้องการเก็บข้อมูลใน S3  |
| signedUrl     | String    | URL ที่ได้ทำการ signed ที่ S3 เอาไว้ (ใช้ในการ upload ไฟล์ที่ได้ทำการ signed เอาไว้)  |
| publicUrl     | String    | url ของไฟล์ที่ทำการบันทึกไว้ที่ s3 (กรณี acl เป็น private จะไม่สามารถดูข้อมูลได้ หากต้องการดูข้อมูล ต้องเรียกใช้ api [Get Object SignedURL](#get-object-signedurl))  |
| filename     | String    | ชื่อของไฟล์ที่ทำการ upload  |
| fileKey     | String    | key ของ ไฟล์ที่ได้จากการ signed ที่ s3  |
| userID     | String    | id ของ user ที่ทำการ upload  |
| expired     | int    | ระยะเวลาหมดอายุการ signed (max 604800 = 7 days)  |
| status     | enum    | UNKNOW, SUCCESS, FAILED (สถานะในการ upload)  |

---

#### Get Object SignedURL

API สำหรับการเรียกข้อมูล url ของไฟล์ที่ทำการ upload เอาไว้

(กรณี acl เป็น PRIVATE หากต้องการเรียกข้อมูลไฟล์ จะต้องเรียกใช้ API นี้เพื่อรับ publicUrl เท่านั้น)

    API name : getSignedUploadFille

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| acl     | enum    | PRIVATE, PUBLIC (สิทธิ์ในการเข้าถึงข้อมูลของไฟล์)  |
| path     | string    | category ที่ต้องการเก็บข้อมูลใน S3  |
| filename     | String    | ชื่อของไฟล์ที่ทำการ upload  |
| contentType     | String    | ประเภทของไฟล์  |


Response : storageSchema

storageSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| acl     | enum    | PRIVATE, PUBLIC (สิทธิ์ในการเข้าถึงข้อมูลของไฟล์)  |
| signedUrl     | String    | URL ที่ได้ทำการ signed ที่ S3 เอาไว้ (ใช้ในการ upload ไฟล์ที่ได้ทำการ signed เอาไว้)  |
| publicUrl     | String    | url ของไฟล์ที่ทำการบันทึกไว้ที่ s3 (กรณี acl เป็น private จะไม่สามารถดูข้อมูลได้ หากต้องการดูข้อมูล ต้องเรียกใช้ api [Get Object SignedURL](#get-object-signedurl))  |
| filename     | String    | ชื่อของไฟล์ที่ทำการ upload  |
| fileKey     | String    | key ของ ไฟล์ที่ได้จากการ signed ที่ s3  |
| expired     | int    | ระยะเวลาหมดอายุการ signed (max 604800 = 7 days)  |
| status     | enum    | UNKNOW, SUCCESS, FAILED (สถานะในการ upload)  |



---
<br>

### Mutation

เป็น API ที่ใช้สำหรับการแก้ไขข้อมูล

#### Put Object SignedURL

API สำหรับการ signed ที่ S3 ก่อนที่จะทำการ upload ไฟล์ไปยัง url ที่ทำการ signed เอาไว้

โดยเมื่อทำการ signed ที่ S3 เรียบร้อยแล้ว จะทำการบันทึกข้อมูลลง database โดยบันทึก status ว่า UNKNOW และส่งข้อมูลไปยัง schedule service เพื่อเช็คข้อมูลอีกครั้งตาม expired ว่าข้อมูลที่ได้ทำการ signed เอาไว้ได้ทำการ upload แล้วหรือยัง

    API name : putSignedUploadFille

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| acl     | enum    | PRIVATE, PUBLIC (สิทธิ์ในการเข้าถึงข้อมูลของไฟล์)  |
| path     | string    | category ที่ต้องการเก็บข้อมูลใน S3  |
| filename     | String    | ชื่อของไฟล์ที่ทำการ upload  |
| contentType     | String    | ประเภทของไฟล์  |


Response : storageSchema

storageSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| acl     | enum    | PRIVATE, PUBLIC (สิทธิ์ในการเข้าถึงข้อมูลของไฟล์)  |
| signedUrl     | String    | URL ที่ได้ทำการ signed ที่ S3 เอาไว้ (ใช้ในการ upload ไฟล์ที่ได้ทำการ signed เอาไว้)  |
| publicUrl     | String    | url ของไฟล์ที่ทำการบันทึกไว้ที่ s3 (กรณี acl เป็น private จะไม่สามารถดูข้อมูลได้ หากต้องการดูข้อมูล ต้องเรียกใช้ api [Get Object SignedURL](#get-object-signedurl))  |
| filename     | String    | ชื่อของไฟล์ที่ทำการ upload  |
| fileKey     | String    | key ของ ไฟล์ที่ได้จากการ signed ที่ s3  |
| expired     | int    | ระยะเวลาหมดอายุการ signed (max 604800 = 7 days)  |
| status     | enum    | UNKNOW, SUCCESS, FAILED (สถานะในการ upload)  |

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

### shedule
consume ข้อมูล schedule
    
    topic: schedule_alarm

---

#### Alarm Schedule

รับข้อมูล schedule เมื่่อถึงเวลาตามที่ตั้งไว้ใน schedule

โดยเมื่อได้รับข้อมูลจาก schedule แล้วจะทำการนำ _id ที่ได้รับมาเช็คที่ database และนำข้อมูลนั้นตรวจสอบอีกทีที่ S3 หากข้อมูลทำการ upload เรียบร้อยแล้ว จะอัพเดต status เป็น SUCCESS แล้วบันทึกลงใน database แต่หากยังไม่มีการ upload จะทำการอัพเดต status เป็น FAILED

    Action: UPDATE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ของไฟล์ที่ทำการ signed ไว้  |
| sendTime     | Date    | timeStamp ในช่วงเวลาที่ส่งข้อมูลมาจาก schedule  |

---

## Kafka Produce Reference

### Schedule

produce เวลาที่ต้องการแจ้งเตือน

    topic: schedule

---

#### Set alarm time

ส่งข้อมูล storage เมื่อมีการ signed ที่ S3 โดยอิงเวลาแจ้งเตือนจากตัวแปร expired ที่เก็บไว้ใน database

    Action: ADD

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| serviceKey     | string    | serviceKey ของ service ที่ส่งมา  |
| description     | String    | คำอธิบาย  |
| repeat     | ENUM    | ONCE,EVERY  |
| every     | string    | รอบการแจ้งเตือน เช่น 1D, 2M, EVERY MONDAY *หากเลือก repeat เป็น every  |

รายระเอียดในการตั้งเวลาแจ้งเตือน

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id จาก database ที่ได้หลักจากการ signed ที่ S3 เรียบร้อยแล้ว  |
| serviceKey     | string    | serviceKey  |
| description     | String    | คำอธิบาย  |
| repeat     | ENUM    | ONCE  |
| every     | string    | undefined  |

---

produce ข้อมูลหลังจากจบกระบวนการทำงานแล้วจะส่ง result กลับไปยังที่ schedule service
    
    topic: schedule_alarm_result

---

#### update schedule

รับข้อมูลเวลา

    Action: UPDATE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| sendTime     | Date    | timeStamp จากที่ schedule ส่งมาในตอนแรก |
| receiveTime     | Date    | ช่วงเวลาที่่ข้อมูลมาถึง service |
| sendTime2     | Date    | timeStamp ที่เริ่มส่งข้อมูลไปยัง schedule |

---