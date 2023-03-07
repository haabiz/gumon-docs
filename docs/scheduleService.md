# Schedule Service

Service สำหรับจัดการตั้งเวลาในการส่งข้อมูลหรือการตั้งรอบเวลาในการทำงานต่างๆ โดยเมื่อถึงเวลาที่กำหนด service นี้จะทำการส่งข้อมูลหรือการแจ้งเดือนกลับไปยัง service ที่ตั้งเวลานั้นไว้

 
<br>

- [API Reference](#api-reference)
- [kafka consum Reference](#kafka-consum-reference)
- [kafka produc Reference](#kafka-produc-reference)

---

<br>
<br>

## API Reference
---

### Query

เป็น API ที่ใช้สำหรับการ Query ข้อมูลออกมา ไม่มีการแก้ไข Data

#### Get Schedule

API สำหรับการเรียกดูรายการข้อมูล schedule

    API name : getSchedules


Response : scheduleSchema[]

scheduleSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| serviceKey     | string    | serviceKey ของ service ที่ส่งมา  |
| description     | String    | คำอธิบาย  |
| repeat     | ENUM    | ONCE,EVERY  |
| every     | string    | รอบการแจ้งเตือน เช่น 1D, 2M, EVERY MONDAY *หากเลือก repeat เป็น every  |

---

#### Get Schedule By ID

API สำหรับการเรียกดูข้อมูล Schedule โดยอ้างอิงจาก ID

    API name : getScheduleByID

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | String    | ID ของ schedule |


Response : scheduleSchema

scheduleSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| serviceKey     | string    | serviceKey ของ service ที่ส่งมา  |
| description     | String    | คำอธิบาย  |
| repeat     | ENUM    | ONCE,EVERY  |
| every     | string    | รอบการแจ้งเตือน เช่น 1D, 2M, EVERY MONDAY *หากเลือก repeat เป็น every  |

---

---

### Mutation

เป็น API ที่ใช้สำหรับการแก้ไขข้อมูล

#### Create Schedule

API สำหรับการสร้าง Schedule

    API name : createSchedule

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| serviceKey     | string    | serviceKey ที่ต้องการตั้งแจ้งเตือน  |
| description     | String    | คำอธิบาย  |
| repeat     | ENUM    | ONCE,EVERY  |
| every     | string    | รอบการแจ้งเตือน เช่น 1D, 2M, EVERY MONDAY *หากเลือก repeat เป็น every  |

Response : scheduleSchema

scheduleSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| serviceKey     | string    | serviceKey ของ service ที่ส่งมา  |
| description     | String    | คำอธิบาย  |
| repeat     | ENUM    | ONCE,EVERY  |
| every     | string    | รอบการแจ้งเตือน เช่น 1D, 2M, EVERY MONDAY *หากเลือก repeat เป็น every  |

---
#### Update Schedule

API สำหรับการแก้ไขข้อมูล Schedule

    API name : updateSchedule

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| description     | String    | คำอธิบาย  |
| repeat     | ENUM    | ONCE,EVERY  |
| every     | string    | รอบการแจ้งเตือน เช่น 1D, 2M, EVERY MONDAY *หากเลือก repeat เป็น every  |

Response : scheduleSchema

scheduleSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| serviceKey     | string    | serviceKey ของ service ที่ส่งมา  |
| description     | String    | คำอธิบาย  |
| repeat     | ENUM    | ONCE,EVERY  |
| every     | string    | รอบการแจ้งเตือน เช่น 1D, 2M, EVERY MONDAY *หากเลือก repeat เป็น every  |

---
#### delete Schedule

API สำหรับการลบ Schedule

    API name : deleteSchedules

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _ids     | String[]    | list ID ของ schedule ที่ต้องการลบ  |


Response : DeleteStatus

DeleteStatus
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| status     | ENUM    | SUCCESS, ERROR  |
| _id     | string[]    | list ของ ID  |

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

### Schedule
consume ข้อมูลที่ service อื่นส่งมา
    
    topic: schedule

---

#### create schedule

รับข้อมูลที่ service นั้นสร้างขึ้นมาเพื่อต้องการตั้งเวลาในการแจ้งเตือนหรือการทำงานบางสิ่งที่ต้องการรอบเวลาหรือเวลาในการแจ้งเตือน

    Action: ADD

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| serviceKey     | string    | serviceKey ของ service ที่ส่งมา  |
| description     | String    | คำอธิบาย  |
| repeat     | ENUM    | ONCE,EVERY  |
| every     | string    | รอบการแจ้งเตือน เช่น 1D, 2M, EVERY MONDAY *หากเลือก repeat เป็น every  |

---

consume ข้อมูลที่ service ที่รับ schedule แล้วหลังจากจบกระบวนการทำงานแล้วจะส่ง result กลับมาที่ schedule
    
    topic: schedule_alarm_result

---

#### update schedule

รับข้อมูลเวลา

    Action: ADD

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| sendTime     | Date    | timeStamp จากที่ schedule ส่งมาในตอนแรก |
| receiveTime     | Date    | ช่วงเวลาที่่ข้อมูลมาถึง service |
| sendTime2     | Date    | timeStamp ที่เริ่มส่งข้อมูลมายัง schedule |

---


#### Schedule

## Kafka Produce Reference

produce ข้อมูล schedule

    topic: schedule_alarm

---

#### Alarm Schedule

ส่งข้อมูล schedule เมื่่อถึงเวลาตามที่ตั้งไว้ใน schedule

    Action: UPDATE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| sendTime     | Date    | timeStamp ในช่วงเวลาที่ส่งข้อมูล  |

---