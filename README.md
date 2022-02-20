# Phichaikorn_Lekwattana
Blynk WIFI temperature
/*************************************************************
วิธีการใช้งานระบบของ blynk Iot ก่อนอื่นๆ
1. โหลด libraries ที่จะใช้งานระบบของ Blynk Iot บน Cloud ของเชิฟเวอร์
2. ใช้งานลิงค์แปะไว้ 
 libraries
    https://github.com/adafruit/Adafruit_Sensor
    https://github.com/adafruit/DHT-sensor-library
 เว็บไซต์ที่ใช้งาน
    Blynk Create : https://examples.blynk.cc/?board=ESP8266&shield=ESP8266%20WiFi&example=GettingStarted%2FBlynkBlink&auth=vDcDFN403lRbAZeozrHtzfS1YAzic06e
    Blynk iot    : https://blynk.cloud
*************************************************************/

/*************************************************************
  App project setup:
    Value Display widget attached to V0 คือระบบที่จะส่ง
    Value Display widget attached to V1 คือระบบที่จะส่ง
 *************************************************************/
 
#define BLYNK_TEMPLATE_ID "TMPLGNyuM0Dn"
#define BLYNK_DEVICE_NAME "Temp"
#define BLYNK_AUTH_TOKEN "HXxyQz0Gl_R3ABc-qMb8L-b7jIf9QpfH"


// Comment this out to disable prints and save space
#define BLYNK_PRINT Serial


#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <DHT.h>

char auth[] = BLYNK_AUTH_TOKEN;

// Your WiFi credentials.
// Set password to "" for open networks.
char ssid[] = "-"; //ชื่อ WIFI
char pass[] = "-";     //รหัส

#define DHTPIN D2          // What digital pin we're connected to

// Uncomment whatever type you're using!
#define DHTTYPE DHT11     // DHT 11
//#define DHTTYPE DHT22   // DHT 22, AM2302, AM2321
//#define DHTTYPE DHT21   // DHT 21, AM2301

DHT dht(DHTPIN, DHTTYPE);
BlynkTimer timer;

// This function sends Arduino's up time every second to Virtual Pin (5).
// In the app, Widget's reading frequency should be set to PUSH. This means
// that you define how often to send data to Blynk App.
void sendSensor()
{
  float h = dht.readHumidity();
  float t = dht.read (); // or dht.readTemperature(true) for Fahrenheit

  if (isnan(h) || isnan(t)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }
  // You can send any value at any time.
  // Please don't send more that 10 values per second.
  Blynk.virtualWrite(V0, h);
  Blynk.virtualWrite(V1, t);
}

void setup()
{
  // Debug console
  Serial.begin(115200);

  Blynk.begin(auth, ssid, pass);
  // You can also specify server:
  //Blynk.begin(auth, ssid, pass, "blynk.cloud", 80);
  //Blynk.begin(auth, ssid, pass, IPAddress(192,168,1,100), 8080);

  dht.begin();

  // Setup a function to be called every second
  timer.setInterval(1000L, sendSensor);
}

void loop()
{
  Blynk.run();
  timer.run();
}
