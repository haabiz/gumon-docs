# ACL Service

Service ที่เอาไว้จัดการ Permission และ User Policy โดยจะรับ Permission จาก serviceอื่น และเจน User Policy ไป

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

#### Get Permission

API สำหรับการเรียกดูรายการข้อมูล Permission ที่มีในระบบ

    API name :  getPermission

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| userId     | String    | ค้นหาด้วย ID ของ user |
| serviceKey     | string    | ค้นหาด้วย serviceKey เจ้าของ permission  |
| key     | string    | ค้นหาด้วย key ของ permission |
| name    | string    | ค้นหาด้วย key ของ permission |
| description    | string    | ค้นหาด้วย description ของ permission |

Response : permissionSchema[]

permissionSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| serviceKey     | string    | serviceKey เจ้าของ permission  |
| key     | string    | key ของ permission |
| name    | string    | key ของ permission |
| description    | string    | description ของ permission |

---

#### Get My Permission

API สำหรับการเรียกดูรายการข้อมูล Permission ของ user ที่ login

    API name :  getMyPermission

Response : permissionSchema[]

permissionSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| serviceKey     | string    | serviceKey เจ้าของ permission  |
| key     | string    | key ของ permission |
| name    | string    | key ของ permission |
| description    | string    | description ของ permission |

---

#### Get User Permission

API สำหรับการเรียกดูรายการข้อมูล Permission ของ user ที่ระบุ

    API name :  getMyPermission

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| userId     | String    | ID ของ user |

Response : permissionSchema[]

permissionSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| serviceKey     | string    | serviceKey เจ้าของ permission  |
| key     | string    | key ของ permission |
| name    | string    | key ของ permission |
| description    | string    | description ของ permission |

---

### Mutation

เป็น API ที่ใช้สำหรับการแก้ไขข้อมูล

#### set Permission

API สำหรับการสร้าง Permission

    API name : createPermission

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| serviceKey     | string    | serviceKey ของ service |
| permissions     | Array< permissionSchema >    | array ของ permission ที่มีใน service ทั้งหมด |

permissionSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| key     | string    | key ของ permission ห้ามซ้ำ |
| name    | string    | key ของ permission |
| description    | string    | description ของ permission |

Response : permissionSchema[]

permissionSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| serviceKey     | string    | serviceKey เจ้าของ permission  |
| key     | string    | key ของ permission |
| name    | string    | key ของ permission |
| description    | string    | description ของ permission |

---

#### Update Permission

API สำหรับการแก้ไขข้อมูล Permission

    API name : updatePermission

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| id     | String    | id ของ Permission ที่ต้องหารแก้ไข  |
| key     | string    | key ของ permission |
| name    | string    | key ของ permission |
| description    | string    | description ของ permission |

Response : permissionSchema[]

permissionSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| serviceKey     | string    | serviceKey เจ้าของ permission  |
| key     | string    | key ของ permission |
| name    | string    | key ของ permission |
| description    | string    | description ของ permission |

---

#### Delete Permission

API สำหรับการลบ Permission

    API name : deletePermission

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _ids     | String[]    | list ID ของ Permission ที่ต้องการลบ  |

Response : DeleteStatus

DeleteStatus
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| status     | ENUM    | SUCCESS, ERROR  |
| _id     | string[]    | list ของ ID  |

---

#### add Permission in label

API สำหรับการลบ Permission

    API name : deletePermission

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

## Kafka Produce Reference