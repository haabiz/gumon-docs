# Core Service

Service สำหรับจัดการ Application ทั้งเก็บรวบรวมข้อมูลต่างๆของ application รวมไปถึงการตั้งค่าต่างๆของ application

- [API Reference](#api-reference)
- [CLI Reference](#cli-reference)
- [kafka consum Reference](#kafka-consum-reference)
- [kafka produc Reference](#kafka-produc-reference)

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
  - \_id : ID
  - name : String
  - serviceKey : String
  - version : String
  - registerDate : Date

---

#### Get Service By ID

API สำหรับการเรียกข้อมูล Service โดยอ้างอิงจาก ID
    API name : getServiceByID

Input Fields

- \_id : String!

Response

- \_id : ID
- name : String
- serviceKey : String
- version : String
- registerDate : Date

---

#### Get Service By ServiceKey

API สำหรับการเรียกข้อมูล Service โดยอ้างอิงจาก ServiceKey

    API name : getServiceByServiceKey

Input Fields

- ServiceKey : String!

Response

- \_id : ID
- name : String
- serviceKey : String
- version : String
- registerDate : Date

---

#### Get Setting Service

API สำหรับการเรียกข้อมูล Setting ของ Service โดยอ้างอิงจาก ServiceKey

    API name : getSettingService

Input Fields

- ServiceKey : String!

Response

- \_id : ID
- name : String
- serviceKey : String
- version : String
- registerDate : Date
- Setting : JSON

---

#### Get Service Health Check

API สำหรับการเรียกข้อมูล Health Check ของ Serviceในระบบ

    API name : getHealthCheckService

Input Fields

- ServiceKey : String

Response

- services[]
  - \_id : ID
  - name : String
  - serviceKey : String
  - version : String
  - registerDate : Date
  - lastHealthCheck : Date

---


### Mutation

เป็น API ที่ใช้สำหรับการแก้ไขข้อมูล

---

#### Service Registration

API สำหรับการสร้าง Service ใหม่ในระบบ โดยที่มันจะไปเรียกใช้ gumon CLI ในการดาวโหลดและ run Service นั้น

    API name : ServiceRegistration

Input Fields

- name : String
- serviceKey : String
- version : String

Response

- \_id : ID
- name : String
- serviceKey : String
- version : String
- registerDate : Date

---

#### ServiceUpdate

API สำหรับการเปลียนversion ของ serviceName นั้นโดยที่มันจะไปเรียกใช้ gumon CLI และ run Service นั้น

    API name : ServiceUpdate

Input Fields

- name : String
- serviceKey : String
- version : String

Response

- \_id : ID
- name : String
- serviceKey : String
- version : String
- registerDate : Date

---

#### Service Resignation

API สำหรับการนำ service นั้นออกจากระบบ โดยที่จะไปเรียกใช้ gumon CLI

    API name : ServiceResignation

Input Fields

- name : String
- serviceKey : String
- version : String

Response

- \_id : ID
- name : String
- serviceKey : String
- version : String
- registerDate : Date

---


## CLI Reference

### add service

command สำหรับการนำ service นั้นเข้าสู่ระบบระบบ

    > addService [serviceName] [serviceKey] [version]

Response

- \_id : ID
- name : String
- serviceKey : String
- version : String
- registerDate : Date

---

### remove service

command สำหรับการนำ service นั้นออกจากระบบ 

    > removeService [serviceName] [serviceKey] [version]


Response

- \_id : ID
- name : String
- serviceKey : String
- version : String
- registerDate : Date

---

## kafka consum Reference

## kafka produc Reference

