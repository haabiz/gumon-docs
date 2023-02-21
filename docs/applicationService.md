# Application Service

Service สำหรับจัดการ Application ทั้งเก็บรวบรวมข้อมูลต่างๆของ application รวมไปถึงการตั้งค่าต่างๆของ application

- [API Reference](#api-reference)
- [CLI Reference](#cli-reference)
- [kafka consume Reference](#kafka-consume-reference)
- [kafka produce Reference](#kafka-produce-reference)

---

<br>
<br>

## API REFERENCE
---
<br>

### Query

เป็น API ที่ใช้สำหรับการ Query ข้อมูลออกมา

#### Get Application

API สำหรับการเรียกข้อมูล Applications

    API name : getApplications

Response : Applications[]

Application
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| name     | string    | ชื่อ app  |

---

#### Get Application By ID

API สำหรับการเรียกข้อมูล Application โดยอ้างอิงจาก ID

    API name : getApplicationByID

Input Fields

- \_id : String!

Response : Application

Application
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| name     | string    | ชื่อ app  |

---

#### Get Theme

API สำหรับการเรียกดูข้อมูล Theme ของ Application

    API name : getThemes


Response: Theme[]

Theme
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appID     | string    | ID ของ Application  |
| name     | string    | ชื่อ theme  |

---

#### Get Theme By ID

API สำหรับการเรียกดูข้อมูล Theme ของ Application โดยอ้างอิงจาก ID

    API name : getThemeByID

Input Fields

- \_id : String!

Response: Theme

Theme
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appID     | string    | ID ของ Application  |
| name     | string    | ชื่อ theme  |

---

### Mutation

เป็น API ที่ใช้สำหรับการแก้ไขข้อมูล

#### Create Application

API สำหรับการสร้าง Application

    API name : createApplication

Input Fields

- appKey : String!
- name : String

Response: Application

Application
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| name     | string    | ชื่อ app  |

---



#### Update Application

API สำหรับการแก้ไขข้อมูล Application โดยอ้างอิงจาก ID ที่ส่งมา

    API name : updateApplication


Input Fields

- \_id : String!

Response: Application

Application
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| name     | string    | ชื่อ app  |

---

#### Delete Application

API สำหรับการลบ Application โดยอ้างอิงจาก ID ที่ส่งมาทั้งหมด

    API name : deleteApplications

Input Fields

- \_id : String[]!

Response : DeleteStatus

DeleteStatus
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| status     | ENUM    | SUCCESS, ERROR  |
| _id     | string[]    | list ของ ID  |

---

##### Create Theme

API สำหรับการสร้าง Theme ของ Application

    API name : createTheme

Input Fields

- appID : String!

Response: Theme

Theme
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appID     | string    | ID ของ Application  |
| name     | string    | ชื่อ theme  |

---

#### Update Theme

API สำหรับการแก้ไขข้อมูล Theme ของ Application

    API name : updateTheme

Input Fields

- \_id : String!

Response: Theme

Theme
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appID     | string    | ID ของ Application  |
| name     | string    | ชื่อ theme  |

---

#### Delete all Theme in Application

API สำหรับการลบ Theme ทั้งหมดของ Application

    API name : deleteAllTheme

Input Fields

- appID : String!

Response : DeleteStatus

DeleteStatus
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| status     | ENUM    | SUCCESS, ERROR  |
| _id     | string[]    | list ของ ID  |

---

#### Delete Theme

API สำหรับการลบ Theme ตาม ID ที่ส่งมา

    API name : deleteTheme

Input Fields

- \_id : String[]!

Response : DeleteStatus

DeleteStatus
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| status     | ENUM    | SUCCESS, ERROR  |
| _id     | string[]    | list ของ ID  |


<br>
<br>

## CLI Reference

### Create Application

command สำหรับการสร้าง Application

    > gumon exec applicationService create [applicationName]

หรือหลังจาก execute เข้าไปใน service แล้ว

    > create

```
application name : [your application]
appKey : [application Key]
```

## Kafka consume Reference

---

<br>
<br>

## Kafka produce Reference
---

produc ข้อมูล application 
    
    topic: sync-application

### ACTION

#### update Service

ส่งข้อมูล Application เมื่่อมีการ update ข้อมูลของ Application

    Action: ADD

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | serviceKey  |
| name     | string    | name  |

