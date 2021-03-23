#  การทดลองที่ 2 เรื่อง การเขียนโปรแกรมค้นหาไวไฟ

##  วัตถุประสงค์ (อธิบายเป็นข้อๆ)
   1. เพื่อศึกษาการทำงานของไมโครคอนโทรเลอร์
   2. เพื่อศึกษาข้อมูลพื้นฐานของโปรแกรมด้าน WIFI
   3. เพื่อศึกษาการค้นหา WIFI จากไมโครคอนโทรเลอร์
   4. เพื่อศึกษาการอัพโหลดโปรแกรมเข้าไมโครคอนโทรเลอร์
##  อุปกรณ์ที่ใช้ (รายการอุปกรณ์)
   1. ไมโครคอนโทรเลอร์ ชื่อ ESP-01 ภายในประกอบด้วย WIFI 
   2. อุปกรณ์ต่อ USB เพื่อไปยัง serial
##  ศึกษาข้อมูลเบื้องต้น (แหล่งข้อมูลเพื่อการศึกษา)
    https://youtu.be/yBjab0UNuB8

##  วิธีการทำการทดลอง (ทำเป็นขั้นตอนพร้อมภาพประกอบ)
   1. นำไมโครคอนโทรเลอร์ เสียบเข้าไปใน USB 
   2. พิมพ์ **1s** เพื่อเข้าสู่ platformio.ini src
   3. พิมพ์ **vi src/main.cpp** เพื่อเข้าสู่โปรแกรม
   ![image](https://user-images.githubusercontent.com/80879429/112137741-36f8d600-8c03-11eb-82c6-998286d2f52a.png)
   4. เตรียมการอัพโหลดโปรแกรม โดยโปรแกรมมี 2 ส่วน 
        - ส่วนที่ 1 **void setup**  เตรียมการ set WIFI ให้พร้อมทำงาน
        - ส่วนที่ 2 **void loop**   วนลูป เริ่มต้นค้นหา จนพบ และแสดงผลการค้นหา WIFI รอบตัว
        - พิมพ์ **:q** เพื่อออก
        * *เนื้อหารายละเอียดของโปรแกรมที่แสดงใน platformio*

```javascript
#include <Arduino.h>
#include <ESP8266WiFi.h>

int cnt = 0;

void setup()
{
	Serial.begin(115200);
	WiFi.mode(WIFI_STA);
	WiFi.disconnect();
	delay(100);
	Serial.println("\n\n\n");
}

void loop()
{
	Serial.println("========== เริ่มต้นแสกนหา Wifi ===========");
	int n = WiFi.scanNetworks();
	if(n == 0) {
		Serial.println("NO NETWORK FOUND");
	} else {
		for(int i=0; i<n; i++) {
			Serial.print(i + 1);
			Serial.print(": ");
			Serial.print(WiFi.SSID(i));
			Serial.print(" (");
			Serial.print(WiFi.RSSI(i));
			Serial.println(")");
			delay(10);
		}
	}
	Serial.println("\n\n");
}

© 2021 GitHub, Inc.
```

   ![image](https://user-images.githubusercontent.com/80879429/112137785-44ae5b80-8c03-11eb-8b6c-442282d5e1ed.png)
   5. อัพโหลดโปรแกรมเข้าสู่ ไมโครคอนโทรเลอร์
        - พิมพ์ **pio run -t upload** เพื่ออัพโหลด
        - ทำการกดปุ่ม upload(ดำ) ค้างไว้ และกดปุ่ม reset(แดง) เพื่อให้โปรแกรมอัพโหลดได้ 
   ![image](https://user-images.githubusercontent.com/80879429/112137817-51cb4a80-8c03-11eb-8451-ca698cc56d90.png)
   ![image](https://user-images.githubusercontent.com/80879429/112137832-57289500-8c03-11eb-9f52-3d0694655f86.png)
   6. พิมพ์ **pio device monitor** เพื่อดูผลลัพธ์ การสแกนหา WIFI 
   ![image](https://user-images.githubusercontent.com/80879429/112137853-5e4fa300-8c03-11eb-8941-5f4695e068a7.png)

##  การบันทึกผลการทดลอง (พร้อมตัวอย่าง)

##  อภิปรายผลการทดลอง (พร้อมตัวอย่าง)

##  คำถามหลังการทดลอง (พร้อมตัวอย่างคำตอบ)
**คำถาม**   หากต้องการ ทำการค้นหา WIFI ใหม่ จะต้องปฏิบัติเช่นไร
*  **คำตอบ**   กดปุ่ม reset (ปุ่มแดง) คอมพิวเตอร์จะเริ่มทำการค้นหาใหม่
