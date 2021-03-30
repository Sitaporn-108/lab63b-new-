#  การทดลองที่่ 7 เรื่อง Share and Scan Network

##  วัตถุประสงค์
 1. เพื่อศึกษาการอ่านและทำความเข้าใจรายละเอียด code ในภาษา C หรือ C+++
 2. เพื่อศึกษาการเขียน Code รวมถึงการปรับเปลี่ยนและแก้ไขให้เกิดประสิทธิภาพที่ดีขึ้น
 3. เพื่อศึกษาการประยุกต์และพัฒนาโปรแกรมให้สอดคล้องกับการทำงานของไมโครคอนโทรเลอร์

##  อุปกรณ์ที่ใช้
   1. ไมโครคอนโทรเลอร์ ชื่อ ESP-01 ประกอบด้วย WIFI ภายในตัว
   2. อุปกรณ์ต่อ USB เพื่อไปยัง serial
   3. Adaptor ที่มีสาย Port 0(สีขาว) และ Port 1(สีเหลือง) ซึ่งสายทั้งหมดต่อกับ LED เปล่งแสง

##  แหล่งข้อมูลเพื่อการศึกษา
 1. Source code แต่ละแลป (เฉพาะ แลป2, 3,และ 6)
 	- https://github.com/choompol-boonmee/lab63b/tree/master/examples
 2. Clips อธิบายรายละเอียดของแต่ละแลป (เฉพาะ แลป2, 3,และ 6)
	- 02 run example 2 https://youtu.be/yBjab0UNuB8
	- 03 run example 3 https://youtu.be/CCnN1WJsXQY
	- 06 run wiri AP https://youtu.be/T26DVHePlTs
 3. แหล่งรวบรวม Code ที่นำมาใช้
  	- การใช้งานเริ่มต้น ESP8266 https://blog.thaieasyelec.com/getting-started-with-esp8266-nodemcu-ch5/
  	- การใช้งาน Arduino https://www.allnewstep.com/article/205/5-arduino-
 	- C Programming Part1 https://benzneststudios.com/blog/c-programming/c-programming-basic-1/
 	- C Programming Part2 https://benzneststudios.com/blog/c-programming/c-programming-basic-2/
 4. ตรวจสอบ IP Address ของ computer
 	- https://www.sony.co.th/th/electronics/support/articles/00022321
 
##  วิธีการทำการทดลอง
 1. นำไมโครคอนโทรเลอร์ เสียบเข้าไปใน USB serial port
 2. เข้าไปในโฟลเดอร์ pattani
	- พิมพ์ **cd pattani** เพื่อเข้าสู่โฟลเดอร์
![image](https://user-images.githubusercontent.com/80879429/113065867-bf6b0e00-91e3-11eb-9243-a90494902fbb.png)

 3. เข้าไปในโปรแกรมทั้งสาม เพื่ออ่านและทำความเข้าใจรายละเอียด
	- พิมพ์ **cd 02_Scan-Wifi** *Enter*
	- พิมพ์ **cd 03_Output-Port** *Enter*
	- พิมพ์ **cd 06_Wifi-AP-Web-Server** *Enter*
 4. นำทุกตัวอย่างโปรแกรมมาพัฒนา  เป็นโปรแกรมตัวอย่างที่ 7 
	- พิมพ์ **cd 07_Share-Scan-Wifi** *Enter*
	- พิมพ์ **vi src/main.cpp** เพื่อเข้าสู่โปรแกรม
 5. อ่านข้อมูลและรายละเอียดของ Source code โปรแกรมที่ 7
	- ส่วนที่ 1 ข้อมูลเบื้องต้น 
		- #include <name of header file> : การบอกให้นำเฮดเดอร์ไฟล์ทั้งสาม มาร่วมในการแปลโปรแกรม
		- const char* ssid : ชื่อไวไฟ
		- const char* password : รหัสผ่านของไวไฟ
		- IPAddress local_ip(...,...,...,...) : IP Address
		- IPAddress gateway(...,...,...,...) : Default Gateway
		- IPAddress subnet(...,...,...,...) : Subnet Mask
		- unsigned char status_led : กำหนดตัวแปรที่เก็บค่าสถานะของ LED
		- ESP8266WebServer server(...) : กำหนดให้งาน server ที่ port ...
	- ส่วนที่ 2 Set up
		- Serial.begin(...) : กำหนดความเร็วของการ Set up 
		- pinMode(0, OUTPUT) : กำหนด Port 0 ของ Output หรือ Port ... ของ Input
		- WiFi.mode(WIFI_STA) : การเปิดไวไฟภายในตัวไมโครคอนโทรเลอร์
		- WiFi.softAP(ssid, password) : การรันค่า ssid, password
		- WiFi.softAPConfig(local_ip, gateway, subnet) : การรันค่า local_ip, gateway, subnet
		- delay(...) : ความหน่วงเวลาของการ Set up

	- ส่วนที่ 3 loop
		- WiFi.scanNetworks() : จำนวน Network หรือผลของการสแกนไวไฟรอบๆ
		- digitalWrite(0,...) : อ่านค่าของ Port 0 ผลที่ได้จะมีค่าแค่ Low or High
		- delay(...) : ความหน่วงเวลาของการสแกนหาไวไฟและความหน่วงเวลาของการแชร์ไวไฟ
	- พิมพ์ **:q** เพื่อออกจากโปรแกรมที่ 7
	- *เนื้อหารายละเอียดของโปรแกรมที่แสดงใน platformio*

```javascript
#include <Arduino.h>
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "SITA-EE108";		//ปรับเปลี่ยน ssid
const char* password = "6210610108";		//ปรับเปลี่ยน password

//ปรับเปลี่ยน IPAddress ให้ตรงกับ Wifiของบ้านตัวเอง
IPAddress local_ip(192, 168, 1, 33);    
IPAddress gateway(192, 168, 1, 1);
IPAddress subnet(255, 255, 255, 0);

unsigned char status_led = 0;   //กำหนดตัวแปร ที่เก็บค่าสถานะของ LED
ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(230400);   //เพิ่มความเร็ว
	pinMode(0, OUTPUT);   //lab3

	WiFi.mode(WIFI_STA);  //lab2
  
	WiFi.softAP(ssid, password);
	WiFi.softAPConfig(local_ip, gateway, subnet);
	delay(1000);    //เพิ่มความหน่วง

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
	Serial.println(WiFi.localIP());           // แสดงหมายเลข IP ของ Server
	Serial.println("\n\n\n");
}

void loop(void){
  //lab6
  server.handleClient();
  //lab2
  Serial.println("========== Start Scan Wifi ===========");
	int n = WiFi.scanNetworks();
	if(n == 0) {
		Serial.println("========== OFF ===========");		//lab3
		status_led=0;                   			//กำหนดค่า ตัวแปรใน status_led=0
		digitalWrite(0, LOW);					//lab3
		Serial.println("NO NETWORK FOUND");
	} else {
		Serial.println("========== ON ===========");	//lab3
		status_led=1;                   		//กำหนดค่า ตัวแปรใน status_led=1
		digitalWrite(0, HIGH);				//lab3
		for(int i=0; i<n; i++) {
			Serial.print(i + 1);
			Serial.print(": ");
			Serial.print(WiFi.SSID(i));
			Serial.print(" (");
			Serial.print(WiFi.RSSI(i));
			Serial.println(")");
			int d = i*1000
			delay(d);		//กำหนดให้ความหน่วงเพิ่มขึ้นตามจำนวนไวไฟที่พบ
		}
	}
	Serial.println("NOW! FOUND %d NETWORK",n);		//บอกจำนวนไวไฟที่เจอรอบสถานที่นั้น
  
  delay(1000)
  Serial.println("\n\n\n");
}

© 2021 GitHub, Inc.
```
 6. อัปโหลดโปรแกรม 07_Share-Scan-Wifi เข้าไปยังไมโครคอนโทรเลอร์
      - พิมพ์ **pio run -t upload** เพื่ออัพโหลด
      ![image](https://user-images.githubusercontent.com/80879429/112097793-3c3e2c80-8bd3-11eb-996e-32ca2630c4d9.jpg)
      - ขณะที่โปรแกรมรัน ต้องกดปุ่มดำ(load)  และปุ่มแดง(reset) ที่ไมโครคอนโทรเลอร์ เพื่อรับโปรแกรมใหม่เข้าไป
      ![image](https://user-images.githubusercontent.com/80879429/112098051-b53d8400-8bd3-11eb-81a0-603bb97c49a6.png)
      - เมื่อรันโปรแกรมเสร็จเรียบร้อย
      ![image](https://user-images.githubusercontent.com/80879429/112096308-9db0cc00-8bd0-11eb-8e18-ad50c46ef244.png)
 7. ตรวจสอบผลลัพธ์ของโปรแกรม    
      - พิมพ์ **pio device monitor** เพื่อดูผลลัพธ์
      - กดปุ่มแดง เพื่อ reset โปรแกรมหยุดทำงานและเริ่มรันโปรแกรมใหม่
      - นำโทรศัพท์มาตรวจสอบไวไฟของไมโครคอนโทรเลอร์

##  การบันทึกผลการทดลอง
 * เมื่ออัพโหลดโปรแกรมเรียบร้อย ทำการมอนิเตอร์ และ WebServer เริ่มดำเนินการ พบว่า สามารถใช้อุปกรณ์ เช่น โทรศัพท์ในการค้นหาครือข่ายหรือ WIFI ของไมโครคอนโทรเลอร์ได้
	* ![image](https://user-images.githubusercontent.com/80879429/112264428-7840c280-8ca3-11eb-9c96-ccc8b082a24a.png)
 * เมื่อเริ่มต้นการวนลูปโปรแกรม พบว่า สามารถค้นหา WIFI จากรอบๆได้ 
	* ![image](https://user-images.githubusercontent.com/80879429/112289756-29a22100-8cc1-11eb-82ca-e945cb5c5e8c.png)
 * เมื่อไม่สามารถสแกนหาไวไฟรอบๆได้ พบว่า LED ไม่เปล่งแสง
 	* ![image](https://user-images.githubusercontent.com/80879429/113069065-c4cb5700-91e9-11eb-871b-07da0ec425f7.png)
 * เมื่อสามารถสแกนหาไวไฟรอบๆได้อย่างน้อย 1 network พบว่า LED เปล่งแสงออกมา
 	* ![image](https://user-images.githubusercontent.com/80879429/113069052-bc731c00-91e9-11eb-9b57-5adbe7c3310c.png)


##  อภิปรายผลการทดลอง
 * จากการทำการทดลอง การพัฒนา Source Code ของโปรแกรม โดยการนำ Source Code ของ 3 โปรแกรม มาดัดแปลงและแก้ไขให้เกิดโปรแกรมในรูปแบบใหม่ พบว่า โปรแกรมใหม่ที่รันใส่ลงในไมโครคอนโทรเลอร์ สามารถที่จะแชร์ไวไฟภายในตัวไมโครคอนโทรเลอร์ ให้อุปกรณ์อื่นๆได้ และสามารถสแกนหาไวไฟรอบๆข้างขณะรันโปรแกรมได้ โดยหากพบไวไฟรอบข้าง หลอดไฟ LED นั้นจะเปล่งแสงสว่างออกมา เป็นการบ่งบอกผลลัพธ์ของการสแกน
##  คำถามหลังการทดลอง
**คำถาม**   หากการสแกนไวไฟ ทำให้พบไวไฟรอบๆ จำนวน 3 ไวไฟ จะมี delay กี่วินาที
*  **คำตอบ** การนลูปแต่ละครั้งมี delay 3 วินาที (3000 มิลลิวินาที) วนลูป 3 รอบ จึงมี delay ทั้งหมด 9  วินาที

