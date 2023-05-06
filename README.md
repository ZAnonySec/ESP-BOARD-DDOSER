# ESP-BOARD-DDOSER
ITS AND ESP BOARD DDOSER ITS ESP-12S DDOSER 
HERE IS THE SKETCH CODE

#include <Arduino.h>
#include <Wire.h>
#include <U8g2lib.h>
#include <ESP8266WiFi.h>
#include <WiFiClient.h>

#define LED_PIN 2

U8G2_SSD1306_128X64_NONAME_F_SW_I2C u8g2(U8G2_R0, /* clock=*/ 14, /* data=*/ 12, /* reset=*/ U8X8_PIN_NONE);

void setup() {
  Serial.begin(115200);

  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, HIGH); // turn on the LED

  u8g2.begin();
  u8g2.clearBuffer();
  u8g2.setFont(u8g2_font_7x14B_tr);
  u8g2.drawStr(0, 10, "Made by Zer0Sec");
  u8g2.setFont(u8g2_font_6x12_tr);
  u8g2.drawStr(0, 25, "DDOS Starting!!");
  u8g2.drawStr(0, 40, "Target IP:");
  u8g2.drawStr(0, 51, "10.0.0.1");
  u8g2.drawStr(0, 62, "Target Port: 80");
  u8g2.sendBuffer();

  WiFi.mode(WIFI_OFF);
}

void loop() {
  bool fiveSecondsPassed = false;
  unsigned long startTime = millis();

  IPAddress targetIP(10, 0, 0, 1); // replace with your target IP address
  int targetPort = 80; // replace with your target port number

  WiFiClient client;
  client.connect(targetIP, targetPort);

  u8g2.setFont(u8g2_font_7x14B_tr);
  u8g2.drawStr(0, 10, "Made by Zer0Sec");
  u8g2.setFont(u8g2_font_6x12_tr);
  u8g2.drawStr(0, 25, "DDOS Starting!!");
  u8g2.drawStr(0, 40, "Target IP:");
  u8g2.drawStr(0, 51, "10.0.0.1");
  u8g2.drawStr(0, 62, "Target Port: 80");
  u8g2.sendBuffer();

  delay(5000); // wait for 5 seconds before starting to send packets

  unsigned long currentTime = startTime;
  String packetData = "";
  int packetsSent = 0;
  int totalBytesSent = 0;
  int totalPacketsSent = 0; // add this line to keep track of total packets sent

  while (currentTime - startTime < 25000) { // send packets for 10 seconds
    static unsigned long lastSecond = 0;
    static int packetsSentThisSecond = 0;

    if (client.connected()) {
        // ...
        packetsSent++;
        totalBytesSent += packetData.length();
        delay(50); // wait for 50 milliseconds between each packet

        // update live packet count on OLED every second
if (millis() - lastSecond >= 1000) {
    lastSecond = millis();
    u8g2.clearBuffer();
    u8g2.setFont(u8g2_font_7x14B_tr);
    u8g2.drawStr(0, 10, "Made by Zer0Sec");
    u8g2.setFont(u8g2_font_6x12_tr);
    u8g2.drawStr(0, 25, "DDOS in progress");
    u8g2.drawStr(0, 40, "Packets sent:");
    totalPacketsSent += packetsSentThisSecond;
    u8g2.setCursor(75, 40);
    u8g2.print(totalPacketsSent);
    u8g2.sendBuffer();
    packetsSentThisSecond = 0;
}

// end the loop after 10 seconds
if (currentTime - startTime >= 10000) {
    client.stop();

    u8g2.clearBuffer();
    u8g2.setFont(u8g2_font_7x14B_tr);
    u8g2.drawStr(0, 10, "Made by Zer0Sec");
    u8g2.setFont(u8g2_font_6x12_tr);
    u8g2.drawStr(0, 25, "DDOS finished");
    u8g2.drawStr(0, 40, "Total packets sent:");
    u8g2.setCursor(95, 40);
    u8g2.print(totalPacketsSent);
    u8g2.sendBuffer();

    while (1) { // infinite loop to prevent the program from continuing to execute
        digitalWrite(LED_PIN, HIGH); // turn on the LED to indicate that the program has finished executing
        delay(500);
        digitalWrite(LED_PIN, LOW);
        delay(500);
    }
}

packetsSentThisSecond++;
   }
 }
}

