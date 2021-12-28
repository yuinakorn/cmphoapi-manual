# CMPHO api-client

ระบบเพื่อส่งข้อมูล

สำหรับติดตั้งใน Raspberry PI

---

### วิธีการติดตั้ง

เชื่อมต่อ Network ผ่าน LAN หรือ WIFI
- คลิกขวาที่ไอคอน network

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi001.png)

- เลือก Wireless & Wired Network Settings

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi003.png)

- เลือก interface เป็น eth0
- ตั้งค่า Private IP ที่สามารถเชื่อมต่อฐานข้อมูล HIS ได้
- เช่น

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi004.png)

เปิด ssh เพื่อ remote
- คลิกที่โลโก้ Raspberry->Preferences->Raspberry Pi Configuration

![image](https://phoenixnap.com/kb/wp-content/uploads/2021/04/raspberry-pi-configuration-preferences-gui.png)

- ไปที่ Interfaces -> SSH: Enable และ VNC: Enable

![image](https://phoenixnap.com/kb/wp-content/uploads/2021/04/raspberry-pi-configuration-interaface-gui.png)

ตั้งค่า เวลา Time Zone ดังนี้
- ไปที่ Localisation->Set Timezone เลือก Asia / Bangkok

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-timezone01.png)
### Remote ไปหา Raspberry Pi ด้วย SSH

- เปิด PowerShell หรือ Terminal สำหรับเครื่อง Mac
- พิมพ์ ssh pi@192.168.xxx.xxx

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi005.png)

- พิมพ์ 'yes' แล้วกด Enter
- พิมพ์รหัสผ่านค่าเริ่มต้น คือ 'raspberry'

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi006.png)

- เข้าสู่ prompt ของ Raspberry Pi ได้แล้ว

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi007.png)

### เปลี่ยน password จากค่าเริ่มต้น เป็นรหัสที่ปลอดภัย
- พิมพ์ ดังนี้
~~~
passwd pi
~~~
- พิมพ์รหัสผ่านปัจจุบัน (raspberry)
- พิมพ์รหัสใหม่ และพิมพ์รหัสผ่านอีกครั้งเพื่อยืนยัน

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi008.png)

### ติดตั้ง editor & git cli
- พิมพ์ ดังนี้
~~~
$ sudo apt install vim -y
$ sudo apt install nano -y
~~~

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-vim01.png)
~~~
$ sudo apt install git -y
$ git --version
~~~

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-git01.png)

---
### ติดตั้ง Web Server
Install Apache (HTTP Server)

update packages เพื่อปรับปรุงและอัพเดทแหล่งซอฟต์แวร์และการตรวจสอบเวอร์ชั่นใหม่
- พิมพ์ ดังนี้
~~~
$ sudo apt-get update
$ sudo apt update
~~~
![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi009.png)
- พิมพ์ ดังนี้
~~~
  $ sudo apt-get install apache2 -y
~~~
![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-apache01.png)
![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-apache02.png)

---
### ติดตั้ง PHP
- พิมพ์ ดังนี้
~~~
$ sudo apt install libapache2-mod-php -y
~~~
~~~
$ sudo apt install mariadb-server php-mysql -y
~~~
![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-php01.png)
![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-php02.png)

- ติดตั้ง php-curl
~~~
$ sudo apt-get install php-curl -y
~~~
![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-php-curl01.png)

- ติดตั้ง php-pgsql
~~~
sudo apt-get install php-pgsql -y
~~~
![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-pgsql01.png)

---
### ตั้งค่า PHP Web Server

- พิมพ์ ดังนี้
```
$ cd /var/www/
$ ls
$ sudo chown pi: html
```

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-config01.png)

- ตั้งค่า php
~~~
$ php --ini
$ sudo vim /etc/php/7.4/cli/php.ini
~~~
![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-php-con02.png)
- กด i เพื่อ แก้ไขไฟล์
- upload_max_filesize = 100M
- เลื่อนลงมาที่บรรทัด 913
- เอาเครื่องหมาย ; ออกจากหน้า extension ดังต่อไปนี้

~~~
extension=curl
extension=mysqli
extension=openssl
extension=pdo_mysql
extension=pdo_pgsql
extension=pgsql
~~~
![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-php-con05.png)
- กด :wq เพื่อ save แล้วออก

restart service เว็บเซิร์ฟเวอร์ 1 รอบ
~~~
$ sudo systemctl restart apache2
~~~

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-config03.png)

หรือจะ restart เครื่อง Raspberry Pi เลยก็ได้ ด้วยคำสั่ง

~~~
$ sudo init 6
~~~

---
### โหลดโปรเจคจาก github
- เข้าไปที่ path ของ web server
~~~
$ cd /var/www/html/ 
~~~

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-git02.png)

- clone โปรเจคลงมา

~~~
$ git clone https://github.com/yuinakorn/cmphoapi.git
~~~
- ใส่ username github ของท่านเอง หากไม่มี ใส่ yuinakorn ได้
- ในช่อง password ให้ copy token นี้ไปวาง

~~~
Username for : $ yuinakorn
Password for : $ ส่ง Token ให้ใน zoom
~~~

- จะได้โฟลเดอร์ชื่อ cmphoapi ทำการ เข้าไปในโฟลเดอร์โปรเจค พิมพ์

~~~
$ cd cmphoapi
$ ls -l
~~~

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-git03.png)

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-git05.png)

ลองเข้า google chrome ในเครื่อง พิมพ์ ip เครื่อง Raspberry Pi เช่น http://192.168.xxx.xxx/cmphoapi/i.php

จะต้องแสดงหน้า php info

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-phpinfo.png)

---
### เชื่อมต่อ Database HIS

~~~
$ vim connection.php
~~~

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-vim-con00.png)

ตั้งค่า connection ต่าง ๆ

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-vim-con01.png)

~~~
เสร็จแล้วกดปุ่ม Esc 

พิมพ์ :wq แล้ว enter เพื่อ save ไฟล์
~~~

ทดสอบส่งข้อมูล
- เปิด google chrome แล้วพิมพ์ http://192.168.xxx.xxx/cmphoapi/api-sent.php
- หากส่งสำเร็จจะมี message : OK คืนกลับมา

---

### ตั้งเวลาส่งข้อมูลอัตโนมัติ

ตั้งเวลาด้วย cron

คำสั่งตรวจสอบ cron ในเครื่องว่ามีการตั้งเวลาทำงานอะไรไว้หรือไม่

~~~
$ crontab -l
~~~

ทำการตั้งเวลาให้ส่งข้อมูลอัตโนมัติ ดังนี้

~~~
$ crontab -e
~~~

เมื่อเข้ามาครั้งแรกระบบจะให้เลือก text editor ที่จะใช้เขียน crontab

- เลือกตัวเลือกที่ 2 vim.basic

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-crontab01.png)
- จากนั้นจะเปิดไฟล์ crontab ขึ้นมา ให้ทำการลบคำอธิบายทุกบรรทัดออกให้หมดโดยกดปุ่ม dd เพื่อลบทีละบรรทัด

เข้าเว็บ https://crontab-generator.org เพื่อสร้างตารางเวลาให้โปรแกรมทำงาน

กำหนดตารางเวลาตามที่ สสจ.กำหนด เช่น ส่งข้อมูลอัตโนมัติทุกๆ 06.02 น. และ 18.02 น.
ให้เลือก ดังนี้
- เลือกหลักนาทีเป็น 2
- เลือกหลักชั่วโมงเป็น 6am และ 18pm โดยกดปุ่ม Ctrl+คลิกเพื่อเลือกหลายรายการ

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-crontab02.png)

- กำหนดให้โปรแกรมทำอะไร
- พิมพ์ในช่อง Command To Execute ดังนี้

~~~
/usr/bin/php /var/www/html/cmphoapi/api-sent.php
~~~

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-crontab04.png)

จากนั้นเว็บฯจะสร้างคำสั่งให้อัตโนมัติ เช่น

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-crontab03.png)

~~~
2 6,18 * * * /usr/bin/php /var/www/html/cmphoapi/api-sent.php
~~~

คัดลอกคำสั่งนี้ไปวางใน Crontab -e ของ Raspberry Pi

ดังนี้

![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-crontab05.png)

~~~
- กด Esc พิมพ์ :wq เพื่อบันทึกตารางงานอัตโนมัติ
~~~


รีสตาร์ท cron

~~~
$ /etc/init.d/cron reload
~~~
![image](https://www.chiangmaihealth.go.th/cmpho_web/images/yui/pi-cron-restart01.png)
