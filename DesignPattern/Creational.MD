# Creational Patterns
กลุ่มนี้จะช่วยให้ developer ไม่ต้องเสียเวลาในการ สร้าง object ซึ่งแม้ว่ามันจะสร้างง่ายๆด้วยคำสั่ง `new` เท่านั้นก็ตาม เพราะการสร้าง object ด้วยตัวเองมันคือ Hardcode แบบหนึ่งนั่นเอง ดังนั้นถ้าเรามีปัญหาเรื่อง การสร้าง object ให้เรามองมาที่ Patterns ต่างๆที่อยู่ในกลุ่มนี้ได้เลย

![title](/images/designPattern/image_1.png)

## Factory Method
Define an interface for creating a single object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

## Abstract Factory
Provide an interface for creating families of related or dependent objects without specifying their concrete classes.

## Builder
Separate the construction of a complex object from its representation, allowing the same construction process to create various representations.

## Prototype
Specify the kinds of objects to create using a prototypical instance, and create new objects from the 'skeleton' of an existing object, thus boosting performance and keeping memory footprints to a minimum.

## Singleton
Ensure a class has only one instance, and provide a global point of access to it.
