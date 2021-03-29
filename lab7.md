```javascript
#include <Arduino.h>
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

//ปรับเปลี่ยน ssid และ password

const char* ssid = "MY-ESP8266";
const char* password = "6210610108";

//ปรับเปลี่ยน IPAddress ให้ตรงกับ Wifiของบ้านตัวเอง
IPAddress local_ip(192, 168, 1, 33);    
IPAddress gateway(192, 168, 1, 1);
IPAddress subnet(255, 255, 255, 0);

unsigned char status_led = 0;   //กำหนดตัวแปร ที่เก็บค่าสถานะของ LED
ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(230400);   //เพิ่มความเร็ว
	pinMode(LED, OUTPUT);   //lab3 และปรับเปลี่ยน Pin ที่ต่อกับ LED เป็น Output

	WiFi.mode(WIFI_STA);  //lab2
	WiFi.disconnect();    //lab2
  
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
}

void loop(void){
  server.handleClient();
}

© 2021 GitHub, Inc.
```
