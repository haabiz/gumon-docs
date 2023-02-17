# Commands

  เป็นการรวมคำสั่งที่มีใน gumon cli ทั้งหมด

## Init

## Config

## list

## run

## add

  `gumon add [serviceName/url] [version]`

  เป็นการที่จะเพิ่ม service เข้าไปในระบบของ gumon โดยระบุจากชื่อหรือ url ของ docker image ที่จะติดตั้ง โดยที่ถ้าไม่ระบุ version จะเลือก version ล่าสุด
  การทำงานเป็นดังนี้

  1. ค้นหา service ด้วย ชื่อ หรือ url ตามที่ระบุ
  2. download docker image 
  3. ทำการเพิ่มข้อมูล service ใหม่ลงใน .gumon
  4. ทำการตั้งค่า config ของ service
  5. ทำการยิงคำสั่งเพิ่ม service ลงใน core service
  6. run docker image ของ service นั้น

## remove

  `gumon remove [serviceName]`

  เป็นการที่จะลบ service นั้นในระบบ gumon
  การทำงานเป็นดังนี้

  1. ค้นหา service ด้วยชื่อที่ระบุใน .gumon
  2. stop run docker image 
  3. ทำการลบข้อมูล service ใหม่ลงใน .gumon
  4. ทำการยิงคำสั่งลบ service ลงใน core service
  5. ลบ docker image ของ service นั้น

## update

  `gumon update [serviceName] [version]`

  เป็นการที่จะเพิ่ม หรือเปลียน version ของ serviceName นั้น ตาม version ที่ระบุ

  1. ค้นหา service ด้วย ชื่อ และ version
  2. stop run docker image เดิม
  3. ลบ docker image ของ service เดิม
  4. download docker image 
  5. ทำการเพิ่ม/แก้ไขข้อมูล service ใหม่ลงใน .gumon
  6. ทำการตั้งค่า config ของ service ถ้าเป็นการเพิ่มใหม่
  7. ทำการยิงคำสั่งเพิ่ม/แก้ไข service ลงใน core service
  8. run docker image ของ service นั้น

## store

  `gumon store [functionName] `

  เป็นคำสั่งที่เอาไว้ใช้ที่เกี๋ยวข้องก้บ store มี function ดังนี้

  1. `list` ใช้แสดง service ทั้งหมดที่ user ที่ login นั้นสามารถเข้าถึงได้(ถ้าไม่ได้ login จะแสดงแค่อันที่ไม่ต้องการ login )
  2. `login [username] [password]` ไว้ใช้ login
  3. `logout` ไว้ใช้ logout

## install

## help

## execute

  `gumon exec [servicename] [command]`
  
  เป็นคำสั่งที่เอาไว้ส่ง command ไปที่ service นั้นโดยตรง
