# ASP.NET WEB API Pipeline

## หลักการทำงานของ ASP.NET Web API
ในการทำงาน ของ Web API 2 นั้นจะมีการทำงานตามหัวข้อใหญ่ๆดังนี้
- Host คือ สถานที่ที่เว็บ Service ของเราได้ทำการไว้เช่น IIS                           
- Message Handlers คือ เป็นตัวตรวจสอบข้อมูลชั้นแรกก่อนเข้าไปที่ Controller
- Controller คือ ตัว Controller ที่สามารถตรวจสอบข้อมูลที่รับมาว่าถูก Format หรือไม่เช่น ค่า API Key ถูกต้องและเป็นตัวเก็บ Action Method
- Model Biding คือ การแปลงข้อมูลที่รับมาจากผู้ใช้ให้เป็นตัวแปร Object แล้วส่งให้ Action Filers
- Action Filter คือ ส่วนทำการตรวจสอบข้อมูลที่รับเข้ามาอีกรอบหนึ่งว่าถูกหรือไม่ เช่น ข้อมูลต้องเป็นตัวเลข, ต้องมีค่าไม่เกิน 10 เป็นต้น ก่อนที่จะส่งข้อมูลให้ Action Method
- Action Method คือ ส่วนประมวลผลข้อมูล
- Result Conversion คือ การแปลงข้อมูลให้อยู่ในรูปแบบที่ตกลงกันไว้เช่น JSON, XML, BSON

![Overview Pipeline](https://user-images.githubusercontent.com/10595696/203119297-bc32442b-bb98-41b2-98e1-abc942aa0632.jpg)

จากรูปจะเห็นการทำงานเป็นลำดับคือ 
- เมื่อ Host มีการ Request เข้ามา Message Handler จะทำการตรวจสอบเป็นครั้งแรกก่อนว่าข้อมูลที่เข้ามานั้นตรงตาม Format ที่กำหนดหรือไม่ เช่น Header มี Authentication หรือไม่, รูปแบบการ Authentication ถูกต้องหรือเปล่า เป็นต้น 
- ถ้าตรวจสอบถูกต้องจึงจะเข้าไปที่ Controller (ถ้าไม่ถูกให้ response Status Code Error กลับไปให้ผู้ใช้งาน) หลังจากนั้น Controller ก็จะส่งข้อมูลที่ได้ให้ Modelฺ Binding เพื่อทำการแปลงข้อมูลที่รับจากผู้ใช้งานให้เป็นตัวแปรที่สามารถใช้งานได้
- แล้วทำการให้ Action Filter เพื่อทำการตรวจสอบค่าของข้อมูลว่าเป็นไปตามที่กำหนดหรือไม่
- เมื่อตรวจสอบผ่านจึงส่งไปให้ Action Method ทำการประมวล
- แล้วส่งข้อมูลที่ประมวลผลเสร็จแล้วไปให้ Action Filter อีกที 
- แล้วค่อย Result Conversion เพื่อทำการแปลงข้อมูลที่คำนวณได้เป็นรูปแบบที่ผู้ใช้ต้องการเช่น JSON, XML, BSON 

จากหลักการทำงานตามลำดับดังกล่าวมา แต่ละส่วนก็จะมีการทำงานย่อยลงไปซึ่งผมจะแตกการทำงานย่อยแบ่งเป็น 2 ส่วนใหญ่คือ
1. ส่วน Message Handler (บางที่อาจจะเรียกว่า Delegating Handler)
2. ส่วน Controller ไปถึง Method Action

1. ส่วน Message Handler (บางที่อาจจะเรียกว่า Delegating Handler)
   เป็นส่วนของการตรวจสอบข้อมูลชั้นแรกก่อนส่งไปยัง Controller ซึ่งมีการทำงานย่อยลงไปดังรูป ด้านล่าง
- HTTP Request Message คือ ข้อมูลรับจาก Host
- Message Handler คือ ตัวตรวจสอบข้อมูลที่ใช้ในการขอว่า รูปแบบข้อมูลถูกต้องหรือไม่
- Routing Dispatcher คือ ตัวจัดการรูปแบบของเส้นทางที่จะไปเรียก Web Service
- Controller Dispatcher คือ ตัวจัดการเส้นทางว่าควรใช้ Controller ตัวใหน
- HTTP Response Message คือ ข้อมูลที่จะส่งผ่าน HTTP ให้ Host
![Delegating Handler](https://user-images.githubusercontent.com/10595696/203119732-e6ca8c2b-1ed5-43b9-95c2-4050fd075775.jpg)

2. ส่วน Controller ไปถึง Method Action
เป็นส่วนของการแปลงและตรวจสอบข้อมูลภายในเพื่อนำเอาข้อมูลที่ได้ไปประมวลผลดังรูปด้านล่าง
- Controller คือ ตัวเก็บ Action Method
- Authentication Filter คือ การตรวจสอบว่าใครเป็นคนขอข้อมูล
- Authorization  Filter คือ การตรวจสอบสิทธิ์การการเรียกใช้งาน
- Model Binding คือ การแปลงข้อมูลที่รับมาจากผู้ใช้ให้เป็นตัวแปร Object แล้วส่งให้ Action Filers
- Action Filter และ Action Invoke คือ ส่วนทำการตรวจสอบข้อมูลที่รับเข้ามาอีกรอบหนึ่งว่าถูกหรือไม่ เช่น ข้อมูลต้องเป็นตัวเลข, ต้องมีค่าไม่เกิน 10 เป็นต้น 
- Action Method คือ ส่วนประมวลผลข้อมูล
- Exception Filter คือ ตัวตรวจสอบ Error ว่าถ้าเกิด Error ขึ้นเแล้วเป็น Error ประเภทใหน
![Controller to Action Method](https://user-images.githubusercontent.com/10595696/203119923-c76a7aa3-7bbd-4571-8b47-39b4ea98e670.jpg)

Referrence
- https://www.slideshare.net/rjovic/aspnet-webapi-going-deep-into-the-pipeline
- https://www.asp.net/media/4071077/aspnet-web-api-poster.pdf
- https://channel9.msdn.com/Events/TechEd/Europe/2014/DEV-B410
