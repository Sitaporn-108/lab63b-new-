#  การทดลองที่ 6 เรื่อง การเขียนโปรแกรมสร้างไวไฟแอคเซสพอยต์ (Wifi AP)

##  วัตถุประสงค์ (อธิบายเป็นข้อๆ)

##  อุปกรณ์ที่ใช้ (รายการอุปกรณ์)

##  ศึกษาข้อมูลเบื้องต้น (แหล่งข้อมูลเพื่อการศึกษา)
    https://youtu.be/T26DVHePlTs

##  วิธีการทำการทดลอง (ทำเป็นขั้นตอนพร้อมภาพประกอบ)
  1. พิมพ์ **pwd** เพื่อเข้าสู่โปรแกรมตัวอย่างที่ 6 และพิมพ์ **vi src/main.cpp** เพื่อเข้าสู่โปรแกรม
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

##  การบันทึกผลการทดลอง (พร้อมตัวอย่าง)

##  อภิปรายผลการทดลอง (พร้อมตัวอย่าง)

##  คำถามหลังการทดลอง (พร้อมตัวอย่างคำตอบ)
**คำถาม**   
*  **คำตอบ**
