# Install Ubuntu Server 20.04 LTS
## Step 1 Install redis
```
sudo apt update
sudo apt install redis-server
```

## Step 2 Configure Redis
```
sudo nano /etc/redis/redis.conf
```
- Edit
  -  bind 127.0.0.1 ::1 -> bind * -::* #All Ip Address
  -  peotected-mode yes -> peotected-mode no #Enable Lan

## Step 3 Restart Redis Service
```
sudo systemctl restart redis
sudo systemctl status redis
```


หากคุณมีบริการไฟร์วอลล์ UFW ทำงานอยู่คุณต้องเปิดพอร์ตที่ Redis กำลังฟังอยู่ในไฟร์วอลล์ สิ่งนี้จะเปิดใช้งานคำขอภายนอกเพื่อส่งผ่านไฟร์วอลล์ไปยังเซิร์ฟเวอร์ Redis
sudo ufw allow 6379/tcp
sudo ufw reload

ทดสอบการเชื่อมต่อกับเซิร์ฟเวอร์ Redis
```
redis-cli
-> ping 
ระบบจะตอบ pong
```

### Redis Client On Error: Error: write ECONNABORTED Config right?
update redis.conf
peotected-mode yes -> peotected-mode no
