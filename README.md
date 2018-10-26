# Connected Things Workshop 

In this workshop we will learn about how to control things using your voice, for this we are using Google Assistant as Interface and NodeMCU as Controller and Adafruit.io as Cloud, The NodeMCU is an opensource development board using esp8266.  and this is step by step guide complete the workshop. 

## Pre-requirements 

- Laptop
- Arduino IDE 
- NodeMCU
- USB micro to Mini Cable
- Internet Connectivity 
- Beginner level knowledge in programming 

## 1. Setup Arduino IDE

You can download and Install Arduino IDE from here [Arduino IDE](https://www.arduino.cc/en/Main/Software)
In default Arduino IDE only Support Native boards like UNO,Nano.. etc , so we need to install NodeMCU Board  and MQTT Libraries .

 **NodeMCU Board Definition**

Open Arduino IDE and add additional Board URL ` File => Preference (Ctrl + Comma) `

![additional Board URL File](https://github.com/KeralaHardwareCommunity/MFK18_Workshop/blob/master/img/001.jfif)

In Additional Boards Manager, click add and paste the URL there ` http://arduino.esp8266.com/stable/package_esp8266com_index.json `
And click "OK".

![nodemcu](https://github.com/KeralaHardwareCommunity/MFK18_Workshop/blob/master/img/002.jfif)

![nodemcu](https://github.com/KeralaHardwareCommunity/MFK18_Workshop/blob/master/img/003.jfif)


**Download Board Definitions**

Open Board Manager by going to ` Tools => Board => Boards Manger `

![nodemcu](https://github.com/KeralaHardwareCommunity/MFK18_Workshop/blob/master/img/004.jfif)

Open Boards Manager and search for NodeMCU:

![nodemcu](https://github.com/KeralaHardwareCommunity/MFK18_Workshop/blob/master/img/005.jfif)

`Note: It only appears if you set the NodeMCU Board URL (1.1)`

**Download MQTT library**

For MQTT we are using Adafruit.io broker and for that we need to install Adafruit MQTT library . `Sketch => Include library => Manage Libraries`

![mqtt](https://github.com/KeralaHardwareCommunity/MFK18_Workshop/blob/master/img/006.jfif)

In the following window type `mqtt` and just wait a sec , it will load mqtt related lib .

![mqtt](https://github.com/KeralaHardwareCommunity/MFK18_Workshop/blob/master/img/007.JPG)

Now we can see the Adafruit MQTT Lib on the Third one , select the latest verison and click install . that's all.


## Code 

```

#include <ESP8266WiFi.h>
#include "Adafruit_MQTT.h"
#include "Adafruit_MQTT_Client.h"

#define WIFI_SSID "WiFi Name"
#define WIFI_PASS "WiFi Password"

#define MQTT_SERV "io.adafruit.com"
#define MQTT_PORT 1883
#define MQTT_NAME "Adafruit.io User Name"
#define MQTT_PASS "AIO key"

//Set up MQTT and WiFi clients
WiFiClient client;
Adafruit_MQTT_Client mqtt(&client, MQTT_SERV, MQTT_PORT, MQTT_NAME, MQTT_PASS);

//Set up the feed you're subscribing to
Adafruit_MQTT_Subscribe onoff = Adafruit_MQTT_Subscribe(&mqtt, MQTT_NAME "/f/onoff");


void setup()
{
  Serial.begin(9600);

  //Connect to WiFi
  Serial.print("\n\nConnecting Wifi... ");
  WiFi.begin(WIFI_SSID, WIFI_PASS);
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
  }

  Serial.println("OK!");

  //Subscribe to the onoff feed
  mqtt.subscribe(&onoff);

  pinMode(LED_BUILTIN, OUTPUT);
  digitalWrite(LED_BUILTIN, HIGH);
}

void loop()
{
  MQTT_connect();
  
  //Read from our subscription queue until we run out, or
  //wait up to 5 seconds for subscription to update
  Adafruit_MQTT_Subscribe * subscription;
  while ((subscription = mqtt.readSubscription(5000)))
  {
    //If we're in here, a subscription updated...
    if (subscription == &onoff)
    {
      //Print the new value to the serial monitor
      Serial.print("onoff: ");
      Serial.println((char*) onoff.lastread);
      
      //If the new value is  "ON", turn the light on.
      //Otherwise, turn it off.
      if (!strcmp((char*) onoff.lastread, "ON"))
      {
        //Active low logic
        digitalWrite(LED_BUILTIN, LOW);
      }
      else
      {
        digitalWrite(LED_BUILTIN, HIGH);
      }
    }
  }

  // ping the server to keep the mqtt connection alive
  if (!mqtt.ping())
  {
    mqtt.disconnect();
  }
}


/***************************************************
  Adafruit MQTT Library ESP8266 Example

  Must use ESP8266 Arduino from:
    https://github.com/esp8266/Arduino

  Works great with Adafruit's Huzzah ESP board & Feather
  ----> https://www.adafruit.com/product/2471
  ----> https://www.adafruit.com/products/2821

  Adafruit invests time and resources providing this open source code,
  please support Adafruit and open-source hardware by purchasing
  products from Adafruit!

  Written by Tony DiCola for Adafruit Industries.
  MIT license, all text above must be included in any redistribution
 ****************************************************/

void MQTT_connect() 
{
  int8_t ret;

  // Stop if already connected.
  if (mqtt.connected()) 
  {
    return;
  }

  Serial.print("Connecting to MQTT... ");

  uint8_t retries = 3;
  while ((ret = mqtt.connect()) != 0) // connect will return 0 for connected
  { 
       Serial.println(mqtt.connectErrorString(ret));
       Serial.println("Retrying MQTT connection in 5 seconds...");
       mqtt.disconnect();
       delay(5000);  // wait 5 seconds
       retries--;
       if (retries == 0) 
       {
         // basically die and wait for WDT to reset me
         while (1);
       }
  }
  Serial.println("MQTT Connected!");
}


```






