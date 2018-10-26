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

Note: It only appears if you set the NodeMCU Board URL (1.1)


