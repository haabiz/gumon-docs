# Commands

  เป็นการรวมคำสั่งที่มีใน gumon cli ทั้งหมด

---
## Init
 เริ่มต้นสร้าง project และตั้งค่า default ค่าต่างๆของ project
 
    > gumon init [template]
    
   Project name : [project_name]
   database URL (mongodb) : [cluster_url]
   kafka produce URL : [kafka_product_url]
   kafka consume URL : [kafka_consume_url]
   Redis URL (not required) :
   default service list :
- core service
- application service
- user service
- authorization service
- ACL service
- label service

การทำงาน

1. ตั้งค่า project ผ่าน command line
2. สร้าง file gumon.yml ขึ้นมา
3. เก็บข้อมูลการตั้งค่า project
4. เก็บข้อมูล service ที่มีอยู่ใน peoject (ชื่อ service และ image name ของ service)

```
    projectName : gumonExample
    databaseURL : mongodb:gumon:1234/first_collection
    kafkaProductURL : 
    kafkaConsumeURL : 
    redisURL : 
    service: 
        core : CORE_SERVICE@v1.0.0
        application : APPLICATION_SERVICE@v1.0.0
        user : USER_SERVICE@v1.0.0
        authorization : AUTHORIZATION_SERVICE@v1.0.0
        acl : ACL_SERVICE@v1.0.0
        label : LABEL_SERVICE@v1.0.0

```

---
## Config
 แก้ไขการตั้งค่า Project
 
    > gumon config [service_name]

หลังจากพิมพ์คำสั่งระบบจะไปหาไฟล์ตั้งค่า service ที่ต้องการและเปิดหน้า แก้ไขข้อมูล

---
## list
แสดงรายการ service ทั้งหมดใน project

    > gumon list
    
---
## up
เป็นคำสั่งในการ push project ขึ้น docker และทำการ run container ทั้งหมด

    > gumon up [service_name]

*`gumon up` only, ทุก service จะถูก push ขึ้นไป

## down
 เป็นคำสั่งหยุดการทำงาน container ที่ต้องการใน docker
    
    > gumon down [container_name]
    
---
## add
เป็นการที่จะเพิ่ม service เข้าไปในระบบของ gumon โดยระบุจากชื่อหรือ url ของ docker image ที่จะติดตั้ง โดยที่ถ้าไม่ระบุ version จะเลือก version ล่าสุด

    > gumon add [serviceName/url] [version]

  
  การทำงานเป็นดังนี้

  1. ค้นหา service ด้วย ชื่อ หรือ url ตามที่ระบุ
  2. download docker image 
  3. ทำการเพิ่มข้อมูล service ใหม่ลงใน .gumon
  4. ทำการตั้งค่า config ของ service
  5. ทำการยิงคำสั่งเพิ่ม service ลงใน core service
  6. run docker image ของ service นั้น
  
---
## remove
เป็นการที่จะลบ service นั้นในระบบ gumon

    > gumon remove [serviceName]

  
  การทำงานเป็นดังนี้

  1. ค้นหา service ด้วยชื่อที่ระบุใน .gumon
  2. stop run docker image 
  3. ทำการลบข้อมูล service ใหม่ลงใน .gumon
  4. ทำการยิงคำสั่งลบ service ลงใน core service
  5. ลบ docker image ของ service นั้น

---
## update

    > gumon update [serviceName] [version]

  เป็นการที่จะเพิ่ม หรือเปลียน version ของ serviceName นั้น ตาม version ที่ระบุ

  1. ค้นหา service ด้วย ชื่อ และ version
  2. stop run docker image เดิม
  3. ลบ docker image ของ service เดิม
  4. download docker image 
  5. ทำการเพิ่ม/แก้ไขข้อมูล service ใหม่ลงใน .gumon
  6. ทำการตั้งค่า config ของ service ถ้าเป็นการเพิ่มใหม่
  7. ทำการยิงคำสั่งเพิ่ม/แก้ไข service ลงใน core service
  8. run docker image ของ service นั้น

---
## store

    > gumon store [functionName] 

  เป็นคำสั่งที่เอาไว้ใช้ที่เกี๋ยวข้องก้บ store มี function ดังนี้

  1. `list` ใช้แสดง service ทั้งหมดที่ user ที่ login นั้นสามารถเข้าถึงได้(ถ้าไม่ได้ login จะแสดงแค่อันที่ไม่ต้องการ login )
  2. `login [username] [password]` ไว้ใช้ login
  3. `logout` ไว้ใช้ logout

---
## install
เพิ่ม service จาก gumon store เข้าใน project

    > gumon install [service_name/image] [version]

การทำงาน
1. ส่งข้อมูลไปที่ gumon store ว่ามี service นี้และ version ที่ระบุไว้หรือไม่
2. ติดตั้ง service ตาม version ที่ได้ระบุไว้

---
## help
 แสดง command ของ gumon ทั้งหมด
    
    > gumon help [command]

หากต้องการดูรายละเอียดของคำสั่งนั้น ให้พิมพ์คำสั่งนั้นต่อท้าย help

---
## execute

    > gumon exec [servicename] [command]
  
  เป็นคำสั่งที่เอาไว้ส่ง command ไปที่ service นั้นโดยตรง
