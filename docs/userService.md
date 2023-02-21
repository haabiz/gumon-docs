# User Service

Service สำหรับจัดการข้อมูล User ทั้งเก็บรวบรวมข้อมูลต่างๆของ user

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
#### Get User

API สำหรับการเรียกข้อมูล User ที่มีในระบบ

    API name : getUsers

Response

- user[]

userSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| email     | EmailSchema    | email  |
| phone     | PhoneSchema    | phone  |
| username     | string    | username  |
| facebookId     | string    | facebookId  |
| googleId     | string    | googleId  |
| lineId     | string    | lineId  |
| appleId     | string    | appleId  |
| attribute     | object    |   |
| setting     | object    |  |

EmailSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

PhoneSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

---

#### Get User By ID

API สำหรับการเรียกข้อมูล User โดยอ้างอิงจาก ID
    API name : getUserByID

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | ID    | id ที่ใช้อ้างอิง  |

Response

userSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| email     | EmailSchema    | email  |
| phone     | PhoneSchema    | phone  |
| username     | string    | username  |
| facebookId     | string    | facebookId  |
| googleId     | string    | googleId  |
| lineId     | string    | lineId  |
| appleId     | string    | appleId  |
| attribute     | object    |   |
| setting     | object    |  |

EmailSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

PhoneSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

---

#### Get My User

API สำหรับการเรียกข้อมูล User ที่ login
    API name : getMyUser

Input Fields

Response

userSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appKey     | string    | appKey  |
| email     | EmailSchema    | email  |
| phone     | PhoneSchema    | phone  |
| username     | string    | username  |
| facebookId     | string    | facebookId  |
| googleId     | string    | googleId  |
| lineId     | string    | lineId  |
| appleId     | string    | appleId  |
| attribute     | object    |   |
| setting     | object    |  |

EmailSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

PhoneSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

---

### Mutation

เป็น API ที่ใช้สำหรับการแก้ไขข้อมูล

---

#### Register User

API สำหรับการสร้าง User ใหม่ในระบบ

    API name : registerUser

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| email     | EmailSchema    | email  |
| phone     | PhoneSchema    | phone  |
| username     | string    | username  |
| facebookId     | string    | facebookId  |
| googleId     | string    | googleId  |
| lineId     | string    | lineId  |
| appleId     | string    | appleId  |
| attribute     | object    |   |
| setting     | object    |  |

EmailSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

PhoneSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

Response

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
| attribute     | object    |   |
| setting     | object    |  |

EmailSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

PhoneSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

---

#### Update User

API สำหรับแก้ไข User ในระบบ

    API name : updateUser

Input Fields

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
| attribute     | object    |   |
| setting     | object    |  |

EmailSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

PhoneSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

Response

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
| attribute     | object    |   |
| setting     | object    |  |

EmailSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

PhoneSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

---

#### Delete User

API สำหรับลบ User ในระบบ

    API name : deleteUser

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |


Response

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
| attribute     | object    |   |
| setting     | object    |  |

EmailSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

PhoneSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

---

#### Ban User

API สำหรับban User ในระบบ

    API name : deleteUser

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |


Response

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
| attribute     | object    |   |
| setting     | object    |  |

EmailSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

PhoneSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

---

## CLI Reference

### Create User

command สำหรับการนำ service นั้นเข้าสู่ระบบระบบ

    > createUser [appKey] [username]

Response

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
| attribute     | object    |   |
| setting     | object    |  |

EmailSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

PhoneSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

---

### Delete User

command สำหรับการนำ service นั้นออกจากระบบ 

    > deleteUser [appKey] [username]

Response

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
| attribute     | object    |   |
| setting     | object    |  |

EmailSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

PhoneSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |


---

## kafka consum Reference

### Application
consum ข้อมูล application
    
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

## Kafka Produc Reference

### User

produc ข้อมูล user

    topic: sync-user

---

#### Add User

ส่งข้อมูล user เมื่่อมีการสร้าง user ใหม่

    Action: ADD

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appkey     | string    | appkey  |
| email     | EmailSchema    | email  |
| phone     | PhoneSchema    | phone  |
| username     | string    | username  |
| facebookId     | string    | facebookId  |
| googleId     | string    | googleId  |
| lineId     | string    | lineId  |
| appleId     | string    | appleId  |
| attribute     | object    |   |
| setting     | object    |  |

EmailSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

PhoneSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

---

#### Update User

ส่งข้อมูล User เมื่่อมีการแก้ไข

    Action: UPDATE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appkey     | string    | appkey  |
| email     | EmailSchema    | email  |
| phone     | PhoneSchema    | phone  |
| username     | string    | username  |
| facebookId     | string    | facebookId  |
| googleId     | string    | googleId  |
| lineId     | string    | lineId  |
| appleId     | string    | appleId  |
| attribute     | object    |   |
| setting     | object    |  |

EmailSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

PhoneSchema

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| value     | string    | value  |
| verifyStatus     | string    | VERIFIED - NOT_VERIFY |

---

#### Delete Service

ส่งข้อมูล User เมื่่อมีการลบ User

    Action: DELETE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| _id     | string    | id ที่ใช้อ้างอิง  |
| appkey     | string    | appkey  |

---