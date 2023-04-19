# Core Service

Service สำหรับจัดการ Application ทั้งเก็บรวบรวมข้อมูลต่างๆของ application รวมไปถึงการตั้งค่าต่างๆของ application

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

---
#### Get Service

API สำหรับการเรียกข้อมูล Service ที่มีในระบบ

    API name : getServices

Response

- services[]


| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | ID    | id ที่ใช้อ้างอิง  |
| name     | String    | ชื่อ Service  |
| serviceKey     | String    | serviceKey  |
| version     | String    | 1.1.1  |
| registerDate     | Date    | |
---

#### Get Service By ID

API สำหรับการเรียกข้อมูล Service โดยอ้างอิงจาก ID
    API name : getServiceByID

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | ID    | id ที่ใช้อ้างอิง  |

Response

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | ID    | id ที่ใช้อ้างอิง  |
| name     | String    | ชื่อ Service  |
| serviceKey     | String    | serviceKey  |
| version     | String    | 1.1.1  |
| registerDate     | Date    | |

---

#### Get Service By ServiceKey

API สำหรับการเรียกข้อมูล Service โดยอ้างอิงจาก ServiceKey

    API name : getServiceByServiceKey

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| ServiceKey     | String!    | id ที่ใช้อ้างอิง  |

Response

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | ID    | id ที่ใช้อ้างอิง  |
| name     | String    | ชื่อ Service  |
| serviceKey     | String    | serviceKey  |
| version     | String    | 1.1.1  |
| registerDate     | Date    | |

---

#### Get Setting Service

API สำหรับการเรียกข้อมูล Setting ของ Service โดยอ้างอิงจาก ServiceKey

    API name : getSettingService

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| ServiceKey     | String!    | id ที่ใช้อ้างอิง  |

Response

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | ID    | id ที่ใช้อ้างอิง  |
| name     | String    | ชื่อ Service  |
| serviceKey     | String    | serviceKey  |
| version     | String    | 1.1.1  |
| registerDate     | Date    | |
| Setting     | JSON    | |

---

#### Get Service Health Check

API สำหรับการเรียกข้อมูล Health Check ของ Serviceในระบบ

    API name : getHealthCheckService

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| ServiceKey     | String!    | id ที่ใช้อ้างอิง  |

Response

- services[]
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | ID    | id ที่ใช้อ้างอิง  |
| name     | String    | ชื่อ Service  |
| serviceKey     | String    | serviceKey  |
| version     | String    | 1.1.1  |
| registerDate     | Date    | |
| lastHealthCheck     | Date    | |

---


### Mutation

เป็น API ที่ใช้สำหรับการแก้ไขข้อมูล

---

#### Service Registration

API สำหรับการสร้าง Service ใหม่ในระบบ โดยที่มันจะไปเรียกใช้ gumon CLI ในการดาวโหลดและ run Service นั้น

    API name : registerService

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| name     | String    | |
| serviceKey     | String    |  |
| version     | String    | |

Response

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | ID    | id ที่ใช้อ้างอิง  |
| name     | String    | ชื่อ Service  |
| serviceKey     | String    | serviceKey  |
| version     | String    | 1.1.1  |
| registerDate     | Date    | |

---

#### ServiceUpdate

API สำหรับการเปลียนversion ของ serviceName นั้นโดยที่มันจะไปเรียกใช้ gumon CLI และ run Service นั้น

    API name : ServiceUpdate

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| name     | String    | |
| serviceKey     | String    |  |
| version     | String    | |


Response

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | ID    | id ที่ใช้อ้างอิง  |
| name     | String    | ชื่อ Service  |
| serviceKey     | String    | serviceKey  |
| version     | String    | 1.1.1  |
| registerDate     | Date    | |

---

#### Service Resignation

API สำหรับการนำ service นั้นออกจากระบบ โดยที่จะไปเรียกใช้ gumon CLI

    API name : ServiceResignation

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| name     | String    | |
| serviceKey     | String    |  |
| version     | String    | |


Response

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | ID    | id ที่ใช้อ้างอิง  |
| name     | String    | ชื่อ Service  |
| serviceKey     | String    | serviceKey  |
| version     | String    | 1.1.1  |
| registerDate     | Date    | |

---


## CLI Reference

### add service

command สำหรับการนำ service นั้นเข้าสู่ระบบระบบ

    > addService [serviceName] [serviceKey] [version]

Response

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | ID    | id ที่ใช้อ้างอิง  |
| name     | String    | ชื่อ Service  |
| serviceKey     | String    | serviceKey  |
| version     | String    | 1.1.1  |
| registerDate     | Date    | |


---

### remove service

command สำหรับการนำ service นั้นออกจากระบบ 

    > removeService [serviceName] [serviceKey] [version]

Response

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | ID    | id ที่ใช้อ้างอิง  |
| name     | String    | ชื่อ Service  |
| serviceKey     | String    | serviceKey  |
| version     | String    | 1.1.1  |
| registerDate     | Date    | |


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

---

#### Update Application

รับข้อมูล Application เมื่อมีการ update Application

    Action: UPDATE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| name     | string    | ชื่อ app  |

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

    Action: REFRESH_DATA

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |

---

## Kafka Produce Reference

### Service

produce ข้อมูล application 
    
    topic: sync-service

---

#### Add Service

ส่งข้อมูล Service เมื่่อมีการสร้าง Service ใหม่

    Action: ADD

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| serviceKey     | string    | serviceKey  |
| name     | string    | name  |
| version     | String    |   |
| registerDate     | Date    |   |

---

#### Update Service

ส่งข้อมูล Service เมื่่อมีการแก้ไขService ใหม่

    Action: UPDATE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| serviceKey     | string    | serviceKey  |
| name     | string    | name  |
| version     | String    |   |
| registerDate     | Date    |   |

---

#### Delete Service

ส่งข้อมูล Service เมื่่อมีการลบ Service

    Action: DELETE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| serviceKey     | string    | serviceKey  |
| name     | string    | name  |
| version     | String    |   |

---