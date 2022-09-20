# Upstreams
Ref : https://blog.leapgio.cloud/api-gateway-kong-config/

ทำ Load Balance ให้กับ Services จริงๆแล้วมีพระเอกของงานคือ Health checks เพื่อตรวจสอบสถานะของ TCP, HTTP status หรือ Timeout ของ Targets ของเราตามที่เรากำหนดค่าเอาไว้ หากหนึ่งใน Targets ของเรามีสถานะไม่ปกติ มันก็จะ Set Target เป็น Unhealthy และหยุดการส่ง Request เข้าไป จนกว่าสถานะจะกลับมาเป็นปกติจึง Set เป็น Healthy

Health checks มีให้เลือก 2 โหมด คือ Active health checks ตัว Kong จะเข้าไปตรวจสอบ Targets ของเราให้เอง ส่วน Passive health checks จะเกิดการตรวจสอบสถานะของ Targets ก็ต่อเมื่อมีการเรียก Request จาก Clients เข้ามา โดยรวมๆก็ตั้ง Active เถอะครับเพื่อลดโอกาสของ Client Request ที่จะ Failure

![title](/images/kong/image-16.png)

![title](/images/kong/image-15.png)

ลองมา Stop container ตัวนึงที่อยู่ใน Targets Upstreams ของผมจะเห็นได้ว่า Kong พบความไม่ปกติของ Target จึง Set Unhealthy เอาไว้

![title](/images/kong/image-14.png)

เรียก Request ผ่าน Browser เข้าไปที่ตัว load เราจะถูกพาเข้าไปที่ Targets ที่ Healthy อยู่เท่านั้น

![title](/images/kong/image-18.png)