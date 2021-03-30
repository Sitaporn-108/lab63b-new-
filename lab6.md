#  การทดลองที่ 6 เรื่อง การเขียนโปรแกรมสร้างไวไฟแอคเซสพอยต์ (Wifi AP)

##  วัตถุประสงค์
  1. เพื่อศึกษาการอ่านข้อมูลโปรแกรมสร้างไวไฟ
  2. เพื่อศึกษาการติดตั้งไวไฟ การแก้ไขข้อมูล
  3. เพื่อศึกษาการทำงานของไวไฟในตัวไมโครคอนโทรเลอร์

##  อุปกรณ์ที่ใช้ (รายการอุปกรณ์)
  1. ไมโครคอนโทรเลอร์ ชื่อ ESP-01 ประกอบด้วย CPU และเสาอากาศสำหรับ WIFI
  2. อุปกรณ์ต่อ USB เพื่อไปยัง serial
  3. CPU

##  ศึกษาข้อมูลเบื้องต้น (แหล่งข้อมูลเพื่อการศึกษา)
    https://youtu.be/T26DVHePlTs

##  วิธีการทำการทดลอง
 1. พิมพ์ **pwd** เพื่อเข้าสู่โปรแกรม **06_Wifi-AP-Web-Server**  และพิมพ์ **vi src/main.cpp** เพื่อเข้าสู่โปรแกรม
  	- ![image](https://user-images.githubusercontent.com/80879429/112264336-52b3b900-8ca3-11eb-91c4-ce4d915188b9.png)
  	- เมื่อเข้าสู่โปรแกรม ต้องทำการกำหนดชื่อของWIFI รวมถึงตั้งรหัสผ่าน
  	- เตรียม WebServer และสร้าง Access point 
  	- พิมพ์ **:q** เพื่อออก
	- *เนื้อหารายละเอียดของโปรแกรมที่แสดงใน platformio*

```javascript
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "MY-ESP8266";
const char* password = "choompol";

IPAddress local_ip(192, 168, 1, 1);
IPAddress gateway(192, 168, 1, 1);
IPAddress subnet(255, 255, 255, 0);

ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(115200);

	WiFi.softAP(ssid, password);
	WiFi.softAPConfig(local_ip, gateway, subnet);
	delay(100);

	server.onNotFound([]() {
		server.send(404, "text/plain", "Path Not Found");
	});

	server.on("/", []() {
		cnt++;
		String msg = "Hello cnt: ";
		msg += cnt;
		server.send(200, "text/plain", msg);
	});

	server.begin();
	Serial.println("HTTP server started");
}

void loop(void){
  server.handleClient();
}

© 2021 GitHub, Inc.
```
![image](https://user-images.githubusercontent.com/80879429/112264368-5e9f7b00-8ca3-11eb-8c77-d5405d5af6d2.png)

  2. การอัพโหลดโปรแกรมเข้าสู่ ไมโครคอนโทรเลอร์
        - พิมพ์ **pio run -t upload** เพื่ออัพโหลด
        - ![image](https://user-images.githubusercontent.com/80879429/112264393-66f7b600-8ca3-11eb-9426-4496b84436aa.png)

        - กดปุ่ม upload(ดำ) ค้างไว้ และกดปุ่ม reset(แดง) เพื่อให้โปรแกรมอัพโหลดได้
        - เมื่ออัพโหลดเสร็จเรียบร้อย  พิมพ์ **pio device monitor** เพื่อดูผลลัพธ์
        - ![image](https://user-images.githubusercontent.com/80879429/112264412-7119b480-8ca3-11eb-9ff7-923438252768.png)

##  การบันทึกผลการทดลอง
เมื่ออัพโหลดโปรแกรมเรียบร้อย ทำการมอนิเตอร์ และ WebServer เริ่มดำเนินการ พบว่า สามารถใช้อุปกรณ์ เช่น โทรศัพท์ในการค้นหาครือข่ายหรือ WIFI ของไมโครคอนโทรเลอร์ได้
![image](https://user-images.githubusercontent.com/80879429/112264428-7840c280-8ca3-11eb-9c96-ccc8b082a24a.png)

##  อภิปรายผลการทดลอง
คำสั่ง pio device monitor ใช้ในการดูผลลัพธ์ของการรันโปรแกรมใน microcontroller ซึ่่งแสดงผลออกมาว่า ver started และ ตรวจสอบการแสดงผลของ wifi โดยทดลองค้นหาด้วยโทรศัพท์มือถือ ทำให้พบกับ wifi
##  คำถามหลังการทดลอง
**คำถาม**   โปรแกรมนี้สามารถแก้ไขชื่อไวไฟได้หรือไม่ หากก้ได้ แก้อย่างไร
*  **คำตอบ**	โปรแกรมสามารถแก้ไขชื่อไวไฟได้ แก้โค้ดได้ที่ const char* ssid = "**MY-ESP8266**"
