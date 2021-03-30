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
		- IPAddress local_ip : IP Address
		- IPAddress gateway : Default Gateway
		- IPAddress subnet : Subnet Mask
		- unsigned char status_led : กำหนดตัวแปรที่เก็บค่าสถานะของ LED
		- ESP8266WebServer server(80) : กำหนดให้งาน server ที่ port 80
	- ส่วนที่ 2 Set up
		- Serial.begin() : กำหนดความเร็วของการ Set up 
		- pinMode(0, OUTPUT) : กำหนด Port 0 ของ Output หรือ Port ... ของ Input
		- WiFi.mode(WIFI_STA) : การเปิดไวไฟภายในตัวไมโครคอนโทรเลอร์
		- WiFi.softAP(ssid, password) : การรันค่า ssid, password
		- WiFi.softAPConfig(local_ip, gateway, subnet) : การรันค่า local_ip, gateway, subnet
		- delay() : ความหน่วงเวลาของการ Set up

	- ส่วนที่ 3 loop
		- WiFi.scanNetworks() : จำนวน Network หรือผลของการสแกนไวไฟรอบๆ
		- digitalWrite() : อ่านค่าของ Port 0 ผลที่ได้จะมีค่าแค่ Low or High
		- delay() : ความหน่วงเวลาของการสแกนหาไวไฟและความหน่วงเวลาของการแชร์ไวไฟ
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
##  การบันทึกผลการทดลอง

##  อภิปรายผลการทดลอง

##  คำถามหลังการทดลอง
**คำถาม**   
*  **คำตอบ** 

