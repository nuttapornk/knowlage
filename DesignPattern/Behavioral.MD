# Behavioral patterns
![title](/images/designPattern/image_3.png)

กลุ่มนี้จะช่วยให้คลาสต่างๆ ทำงานร่วมกัน ได้ง่ายๆ เพราะทุกครั้งที่เราเพิ่มความสามารถใหม่ๆเข้าไปในโค้ด นั่นหมายถึง คลาสต่างๆที่เรามีอยู่ ไม่ว่าจะเป็นตัวใหม่ หรือ ตัวเก่า มันก็จะต้องทำงานร่วมกันได้อย่างเหมาะสม ดังนั้นเจ้ากลุ่มนี้จะเป็นตัวชี้ทางสว่างว่า ถ้าเจอสถานะการณ์แบบนี้ เราควรจะออกแบบยังไงนั่นเอง

## Chain of Responsibility
## Command
## Iterator
## Mediator
## Memento
* wiki: Save and restore the previous state of an object without revealing the details of its implementation.
## Observer

## State สถานะ
* wiki: Allow an object to alter its behavior when its internal state changes. It appears as if the object changed its class.
* อนุญาตให้ Object สามารถปรับเปลี่ยนพฤติกรรม เมื่อสถานะภายในเปลี่ยนแปลง มันจะปรากฏขึ้นถ้า object ถูกเปลลี่ยน class
* ex: การวาดภาพบน Canvas เมาส์สามารถเปลี่ยน State ได้หลาย State เช่น Brush,Pen,Selection และ cursor จะเปลี่ยนแปลงไปตามสถานะนั้น
   
## Strategy กลยุทธ์
* wiki: Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.
* กำหนดกลุ่มของ Algorithms, ทำการ Encapsulate ในแต่ละ Algorithm ,และทำให้มันสามารถใช้ทดแทนกันได้ Strategy ช่วยให้ใช้ algorithm ที่แตกต่างกันไปตาม Client
* ex: การ Upload image สามารถแยกส่วนของ algorithms imageCompressor(jpg,png),algorithms image filter(BlackWhite,HighContrast) หรือแม้กระทั้ง image storage(Local,Cloud)
  
## Template Method แม่แบบ
## Visitor