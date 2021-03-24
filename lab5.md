#  การทดลองที่ 5 เรื่อง การเขียนโปรแกรมเชื่อมต่อไวไฟและเว็บเซอร์เวอร์

##  วัตถุประสงค์
1. เพื่อศึกษาโปรแกรมเชื่อมต่อไวไฟกับเว็บเซอร์เวอร์
2. เพื่อศึกษาการดูผลลัพธ์จากการเชื่อมไวไฟ

##  อุปกรณ์ที่ใช้ (รายการอุปกรณ์)
1. CPU
2. ไมโครคอนโทรเลอร์ ESP-01
3. อุปกรณ์เชื่อมต่อ USB เข้าไปยัง serial
4. wifi ที่ต้องการเชื่อมกับ microcontroller

##  ศึกษาข้อมูลเบื้องต้น (แหล่งข้อมูลเพื่อการศึกษา)
    https://youtu.be/VX-QNQcO-b4

##  วิธีการทำการทดลอง
1. พิมพ์ **pwd** เพื่อเข้าสู่โปรแกรมตัวอย่างที่ 5 และพิมพ์ **cd ../05_Wifi-Web-Server** กับพิมพ์ **cd ..**
2. พิมพ์ **cd 05_Wifi-Web-Server** กับพิมพ์ **1s**
3. พิมพ์ **vi src/main.cpp** เพื่อเข้าสู่โปรแกรม
	 ![image](https://user-images.githubusercontent.com/80879429/112274173-c5776100-8cb0-11eb-82f8-fc3b5fc989e1.png)

4. เนื่องจากต้องเชื่อมกับไวไฟ จึงต้องใส่ชื่อไวไฟ โดยโปรแกรมแบ่งเป็น 2 ส่วน
    - ส่วนที่ 1 **void set up** (บรรทัดที่15-22) การ connect กับไวไฟที่เลือกไว้
            	* set up webserver แสดงผลเป็น Hello cnt
           	* cnt++ หมายถึง การนับเพิ่มเรื่อยๆ
    - ส่วนที่ 2 **void loop** (บรรทัดที่39-41) 
    - ![image](https://user-images.githubusercontent.com/80879429/112274203-ce683280-8cb0-11eb-882c-be85b7128e0b.png)

    - ![image](https://user-images.githubusercontent.com/80879429/112274222-d32ce680-8cb0-11eb-8c5c-df39dd0e4793.png)

    - พิมพ์ **:q** เพื่อออก
    - *เนื้อหารายละเอียดของโปรแกรมที่แสดงใน platformio*

```javascript
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "HI_BMFWIFI_2.4G";
const char* password = "0819110933";

ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(115200);

	WiFi.mode(WIFI_STA);
	WiFi.begin(ssid, password);
	while (WiFi.status() != WL_CONNECTED) {
		delay(500);
		Serial.print(".");
	}
	Serial.print("\n\nIP address: ");
	Serial.println(WiFi.localIP());

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

5. อัปโหลดโปรแกรม 05_Wifi-Web-Server เข้าไปยังไมโครคอนโทรเลอร์ 
    - พิมพ์ **pio run -t upload** เพื่ออัปโหลด
    - ![image](https://user-images.githubusercontent.com/80879429/112274246-d9bb5e00-8cb0-11eb-8168-aefe0e1155b9.png)

    - เสียบไมโครคอนโทรเลอร์ ไปที่ USB to Serial
    - ![image](https://user-images.githubusercontent.com/80879429/112274269-dde77b80-8cb0-11eb-85e8-058a3d681de0.png)

    - ขณะที่โปรแกรมรัน ต้องกดปุ่มดำ(load)  และปุ่มแดง(reset) ที่ไมโครคอนโทรเลอร์ เพื่อรับโปรแกรมใหม่เข้าไป
6.เมื่ออัปโหลดเสร็จสิ้น 
    - พิมพ์ **pio device monitor** เพื่อเช็คผลลัพธ์
    - ผลลัพธ์ที่แสดง คือ ip address
    - copy ip adress ไปที่ browser สำหรับทดสอบ

##  การบันทึกผลการทดลอง
การใช้คำสั่ง pio device monitor เพื่อดูผลลัพธ์ของการรันโปรแกรมในไมโครคอนโทรเลอร์ พบว่า มีการแสดงที่อยู่ไอพีขึ้นมา และเมื่อทำการคัดลอกไปที่บราวเซอร์ ผลลัพธ์ที่แสดงจะขึ้นว่า Hello 1 โดย cnt เปลี่ยนตัวแปรไปเรื่อยๆ แสดงว่า WebServer ทำงานได้เรียบร้อย

##  อภิปรายผลการทดลอง
pio run -t upload นั้นใช้ในการอัพโหลดข้อมูลจาก 05_Wifi-Web-Server ไปยัง microcontroller โดยคำสั่ง pio device monitor ที่ใช้ดูผลลัพธ์ของโปรแกรมที่ถูกอัพโหลดเข้าไป ซึ่งในที่นี้คือ ip address แล้วจึงทำการคัดลอกเพื่อไปที่บราวเซอร์สำหรับทดสอบต่อไป

##  คำถามหลังการทดลอง
**คำถาม**   หากต้องการแก้ไขรายละเอียดโปรแกรม ต้องทำเช่นไร
* **คำตอบ** พิมพ์ **vi src/main.cpp** เพื่อเข้าสู่โปรแกรมใหม่ และแก้ไขเนื้อหาโค้ดภายในของโปรแกรม และทำขั้นตอนต่อไปซ้ำเพื่ออัปโหลดโปรแกรมใหม่ใส่ไมโครคอนโทรเลอร์
