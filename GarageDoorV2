#include <Blynk.h>

/*************************************************************
This is the latest and likely final rev of my Smartphone Garage Door Opener sketch
using the Adafruit Feather Huzzah board and the Blynk framework. It's pretty robust 
and yells at you when you leave the garage door open too long. I hope it serves you well.

Tyler Winegarner, 2017

/* Comment this out to disable prints and save space */
#define BLYNK_PRINT Serial

#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

// You should get Auth Token in the Blynk App.
// Go to the Project Settings (nut icon).
char auth[] = "AUTH CODE GOES HERE";

// Your WiFi credentials.
// Set password to "" for open networks.
char ssid[] = "WIFI";
char pass[] = "PASSWORD";

// Select your pin with physical button
const int doorPin = 2;
int warnPin;
int warnThreshold = 2000;
int tick = 0;
WidgetLCD lcd(V3);



BlynkTimer timer;

BLYNK_WRITE(V1) {
  warnPin = param.asInt();
}


// V3 LED Widget represents the physical button state
boolean btnState = false;
void buttonLedWidget()
{
  // Read button
  
  // If state has changed...
  if (digitalRead(doorPin) == HIGH) {
   lcd.print(0,0, "Door Open  ");
   tick++;
  }  else {
   lcd.print(0,0, "Door Closed");
    
   tick = 0;
   Blynk.virtualWrite(warnPin, LOW);
  }
  if (tick > warnThreshold) {
    if (warnPin == LOW) {
      Blynk.notify("Garage door is open!");
      tick = (tick - 500);
    }
  }
  Blynk.virtualWrite(V2, tick);
}

void setup()
{
  // Debug console
  Serial.begin(9600);

  Blynk.begin(auth, ssid, pass);
  // You can also specify server:
  //Blynk.begin(auth, ssid, pass, "blynk-cloud.com", 8442);
  //Blynk.begin(auth, ssid, pass, IPAddress(192,168,1,100), 8442);

  // Setup physical button pin (active low)
  pinMode(doorPin, INPUT);

  timer.setInterval(10, buttonLedWidget);
}

void loop()
{
  Blynk.run();
  timer.run();
  
}
