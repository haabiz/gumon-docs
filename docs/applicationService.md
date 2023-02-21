# Application Service

Service สำหรับจัดการ Application ทั้งเก็บรวบรวมข้อมูลต่างๆของ application รวมไปถึงการตั้งค่าต่างๆของ application

- [API Reference](#api-reference)
- [Function](#function)

---

<br>
<br>

## API REFERENCE
---
<br>

### Create Application

API สำหรับการสร้าง Application

    API name : createApplication

Input Fields

- appKey : String!
- name : String

Response

- \_id : ID
- appKey : String
- name : String

---

### Get Application

API สำหรับการเรียกข้อมูล Applications

    API name : getApplications

Response

- applications[]
  - \_id : ID
  - name : String
  - appKey : String

---

### Get Application By ID

API สำหรับการเรียกข้อมูล Application โดยอ้างอิงจาก ID

    API name : getApplicationByID

Input Fields

- \_id : String!

Response

- \_id : ID
- appKey : String
- name : String

---

### Update Application

API สำหรับการแก้ไขข้อมูล Application โดยอ้างอิงจาก ID ที่ส่งมา

    API name : updateApplication


Input Fields

- \_id : String!

Response

- \_id : ID
- appKey : String
- name : String

---

### Delete Application

API สำหรับการลบ Application โดยอ้างอิงจาก ID ที่ส่งมาทั้งหมด

    API name : deleteApplications

Input Fields

- \_id : String[]!

Response

- status : ENUM_STATUS (SUCCESS, ERROR)
- \_id : ID[]

---

### Create Theme

API สำหรับการสร้าง Theme ของ Application

    API name : createTheme

Input Fields

- appID : String!

Response

- \_id : ID
- appID: ID
- name : String

---

### Get Theme

API สำหรับการเรียกดูข้อมูล Theme ของ Application

    API name : getThemes


Response

- themes[]
  - \_id : ID
  - appID : String
  - name : String

---

### Get Theme By ID

API สำหรับการเรียกดูข้อมูล Theme ของ Application โดยอ้างอิงจาก ID

    API name : getThemeByID

Input Fields

- \_id : String!

Response

- themes[]
  - \_id : ID
  - appID : String
  - name : String

---

### Update Theme

API สำหรับการแก้ไขข้อมูล Theme ของ Application

    API name : updateTheme

Input Fields

- \_id : String!

Response

- \_id : ID
- appID: ID
- name : String

---

### Delete all Theme in Application

API สำหรับการลบ Theme ทั้งหมดของ Application

    API name : deleteAllTheme

Input Fields

- appID : String!

Response

- status : ENUM_STATUS (SUCCESS, ERROR)
- appID : ID
- \_id : ID[]

---

### Delete Theme

API สำหรับการลบ Theme ตาม ID ที่ส่งมา

    API name : deleteTheme

Input Fields

- \_id : String[]!

Response

- status : ENUM_STATUS (SUCCESS, ERROR)
- \_id : ID[]

---

## FUNCTION

### Create Application

    createApplication()

---
