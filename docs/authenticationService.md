# Authorization Service

Service สำหรับจัดการระบบ Authorization ต่างๆ

- [API Reference](#api-reference)
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

### Mutation

เป็น API ที่ใช้สำหรับการแก้ไขข้อมูล

#### Login with Username

API สำหรับการเข้าสู่ระบบ

    API name : loginWithUsername

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| username | string | ชื่อผู้ใช้งาน |
| password | string | รหัสผู้ใช้งาน |

Response: LOGINED

LOGINED
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| token     | Token    | ข้อมูล Token  |
| user     | User    | ข้อมูลผู้ใช้งาน  |
| name     | string    | ชื่อ app  |

TOKEN
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| accessToken | string | token สำหรับเข้าสู่ระบบ |
| refreshToken | string | token สำหรับขอ accessToken ใหม่ |

---

#### Login with Email

API สำหรับการเข้าสู่ระบบ

    API name : loginwithEmail

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| email | string | อีเมลของผู้ใช้งาน |
| password | string | รหัสผู้ใช้งาน |

Response: LOGINED

LOGINED
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| token     | Token    | ข้อมูล Token  |
| user     | User    | ข้อมูลผู้ใช้งาน  |
| name     | string    | ชื่อ app  |

TOKEN
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| accessToken | string | token สำหรับเข้าสู่ระบบ |
| refreshToken | string | token สำหรับขอ accessToken ใหม่ |

---

#### Login with Phone number

API สำหรับการเข้าสู่ระบบด้วย Phone number

    API name : loginWithPhoneNumber

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| phoneNumber | string | เบอร์โทรศัพท์ |

Response: OTP_LOGINED

OTP_LOGINED

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| otpToken     | string    | token จากทางผู้ให้บริการ OTP  |
| expireDate     | Date    | วันหมดอายุของ token  |
| user     | User    | ข้อมูลผู้ใช้งาน  |
---

#### Confirm OTP for Login

API สำหรับการส่ง OTP เพื่อทำการ login ด้วย phone number

    API name : confirmOTPforLogin

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| phoneNumber | string | เบอร์โทรศัพท์ |
| otp | string | otp code |

Response: OTP_LOGINED

OTP_LOGINED

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| token     | Token    | ข้อมูล Token  |
| user     | User    | ข้อมูลผู้ใช้งาน  |
---

#### Login with Facebook

API สำหรับการเข้าสู่ระบบด้วย Facebook

    API name : loginWithFacebok

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| facebookToken | string | token จากทาง facebook |

Response: LOGINED

LOGINED
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| token     | Token    | ข้อมูล Token  |
| user     | User    | ข้อมูลผู้ใช้งาน  |
| name     | string    | ชื่อ  |
---

#### Login with Google

API สำหรับการเข้าสู่ระบบด้วย Google

    API name : loginWithGoogle

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| googleToken | string | token จากทาง google |

Response: LOGINED

LOGINED
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| token     | Token    | ข้อมูล Token  |
| user     | User    | ข้อมูลผู้ใช้งาน  |
| name     | string    | ชื่อ  |
---

#### Login with Line

API สำหรับการเข้าสู่ระบบด้วย Line

    API name : loginWithLine

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| lineToken | string | token จากทาง line |

Response: LOGINED

LOGINED
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| token     | Token    | ข้อมูล Token  |
| user     | User    | ข้อมูลผู้ใช้งาน  |
| name     | string    | ชื่อ  |
---

#### Login with appleID

API สำหรับการเข้าสู่ระบบด้วย AppleID

    API name : loginWithAppleID

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| appleToken | string | token จากทาง apple |
| userID | string | userID     จากทาง apple |

Response: LOGINED

LOGINED
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| token     | Token    | ข้อมูล Token  |
| user     | User    | ข้อมูลผู้ใช้งาน  |
| name     | string    | ชื่อ  |

#### Change Password

API สำหรับการเปลี่ยน password

    API name : changePassword

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| password | string | รหัสผ่านปัจจุบัน |
| newPassword | string | รหัสผ่านใหม่ |

Response: RESET_STATUS

RESET_STATUS
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| status     | ENUM    | SUCCESS, FAILED  |
---

#### Forgot Password

API เกี่ยวกับการลืมรหัสผ่าน เพื่อร้องขอการเปลี่ยนรหัส

    API name : forgotPassword

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| email | string | email ในระบบสำหรับขอเปลี่ยนรหัสผ่าน |

Response: RESET_STATUS

RESET_STATUS
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| status     | ENUM    | SUCCESS, FAILED  |
---

#### Reset Password

API สำหรับเปลี่ยนรหัสผ่านหลังจากได้ Token ใน email ที่ถูกส่งไปผ่าน API:[forgotPassword](#forgot-password)

    API name : resetPassword

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| token | string | token ที่ได้มาจาก API:[forgotPassword](#forgot-password) |
| newPassword | string | รหัสผ่านใหม่ |

Response: RESET_STATUS

RESET_STATUS
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| status     | ENUM    | SUCCESS, FAILED  |
---

#### Refresh Token

API สำหรับร้องขอ token ใหม่เมื่อ accessToken หมดอายุ

    API name : refreshtoken

Input Fields

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| refreshToken | string | refreshToken ที่ได้รับตอน generate token |

Response: RESET_STATUS

RESET_STATUS
| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| accessToken | string | token สำหรับเข้าสู่ระบบ |
| refreshToken | string | token สำหรับขอ accessToken ใหม่ |
---

<br>

## Kafka consume Reference

### Authrntication
consume คำสั่งที่ต้องการข้อมูลของ Authentication Service
    
    topic: sync-auth

### ACTION
#### RefreshData

เมื่อมีคำสั่งนี้มา ให้ทำการส่งข้อมูลของตัวเอง อัตเดตขึ้น kafka

    Action: REFRESHDATA

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| appKey     | string    | appKey  |
| serviceKey     | string    | serviceKey  |

---

### User
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

## Kafka produce Reference
 
produce ข้อมูล authentication Service 
    
    topic: sync-auth

### ACTION
#### RefreshData

ส่งข้อมูลของตัวเองขึ้น Kafka เมื่อมีการร้องขอข้อมูลเข้ามา โดยส่งข้อมูลกลับไปตาม Service ที่ส่ง publicKey ของตัวเองมา

    Action: REFRESHDATA

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| secretKey     | string    | appKey  |
---


### Notification
product ข้อมูลที่ต้องการใช้งาน noification (ex. forgot password)
    
    topic: notification

---
#### Notification

เมื่อมีคำสั่งนี้มา ให้ทำการสร้างข้อมูล notification โดยไม่จำเป็นต้องผ่าน API "createNotification"

    Action: CREATE

| key     |   Type    |  คำอธิบาย     |
| ------  | ------    | ------       |
| topic     | String    | หัวข้อ notification  |
| content     | String    | เนื้อหา  |
| type     | ENUM(NOTIFICATION_TYPE) | [NOMAL, EMAIL, SMS] ประเภท notification ที่ต้องการส่ง |

---