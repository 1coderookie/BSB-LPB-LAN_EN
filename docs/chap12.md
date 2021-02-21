[Back to TOC](toc.md)  
[Back to chapter 11](chap11.md)    
   
---      
    

# 12. Hardware in Conjunction with the BSB-LPB-LAN Adapter
    
    
---
    
## 12.1 The Arduino Due
*In general, the use of an [original Arduino Due](https://store.arduino.cc/arduino-due) is recommended.*  
From experience, however, cheap replicas ("clones") of the Arduino Due can also be used, the use of these clones is usually possible without any problems. But: It should be paid attention if a modified board layout (e.g. changed pin assignments) is described in the prduct description. If this is the case and you still want to buy it, you may need to make specific adjustments in the file *BSB_lan_config.h*.
   
*A pinout diagram of the Arduino Due is available in [appendix b](appendix_b.md).*   
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/due_clone_pp.jpg">  
   
*A compatible clone of the Arduino Due.*  
   
***Notes:***  
- Regarding to the [tech specs of the Arduino Due](https://store.arduino.cc/arduino-due), it is recommended to use an external power source at the intended connection of the Arduino (e.g. 9V/1000mA).  
- If you want to power the Due via USB, please use the "Programming Port".  
- It's possible to power the Due via the DC-IN and use USB connection at the programming port for connecting it to the computer at the same time.  
- You can let the adapter be connected to the controller bus of the heater when flashing the Due.   
- *Make sure that you are using a high-quality USB cable!* This applies to the case that you want to power the Due via USB as well as to the case that you want to connect the Due to your PC for flashing. Especially cheap and thin cables (e.g. accessories of smartphones) can cause problems with the power supply and thus the stability of the Due and/or are not always fully wired, so that a use for data transfer is not possible.  
- With some Due models/clones it can happen that they do not seem to work properly after an initial start (e.g. after a power failure) and only work correctly after pressing the reset button. A possible solution for this problem could be to [add a capacitor](https://forum.arduino.cc/index.php?topic=256771.msg2512504#msg2512504).   
 
  
  
***ATTENTION: The GPIOs of the Arduino Due are only 3.3v compatible!***    
    
    
---
    
### 12.1.1 Due + LAN: The LAN Shield
*In general, the use of an [original Arduino LAN shield (v2)](https://store.arduino.cc/arduino-ethernet-shield-2) is recommended.*  
From experience, however, cheap replicas ("clones") of these LAN shields can also be used, the use of these clones is usually possible without any problems. But: It should be paid attention if a modified board layout (e.g. changed pin assignments) is described in the product description. If this is the case and you still want to buy it, you may need to make specific adjustments in the file *BSB_lan_config.h*.  
   
There are / have been two different versions of LAN shields available on the market: one with a WIZnet W5100 chip (v1) and one with a W5500 chip (v2). The usage of a v2-shield is recommended, it's also available at the official [Arduino store](https://store.arduino.cc/arduino-ethernet-shield-2).  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/lanshield_clone.jpg">  
   
*A compatible clone of a LAN shield with a W5100 chip.*  
       
***Notes:***     
After the installation of the Arduino IDE it should be checked that the current version of the Ethernet Library (min. v2) is installed.   
As a LAN cable one should preferably use a S/FTP type with a minimum length of one metre.  
   
---  
   
### 12.1.2 Due + WLAN: The ESP8266-WiFi-Solution
Another option for integrating the adapter setup into your WLAN is connecting an ESP8266 (NodeMCU or Wemos D1) additionally to the Arduino Due via the six-pole SPI header.  
The ESP8266 is supplied with power (+5V) by the Due and basically serves instead of the LAN shield only as an interface to access the Due via the network. The ESP8266 has to be flashed with a special firmware for this purpose, you can read more about this later in this chapter. The BSB-LAN software is still installed on the Due.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/due_clone_SPI.jpg">  
  
*The six-pole SPI header of the Arduino Due which has to be used.*  
   
The connections have to be done as follows:  
  
| Pin DUE | Function | Pin ESP8266 |  
|:-----------:|:-------------:|:----------:|  
|SPI 1 | MISO (Master Innput Slave Output) | D06 |  
|SPI 2 | VCC (power supply ESP) | +5V / Vin |  
|SPI 3 | SCK (Serial Clock) | D05 |  
|SPI 4 | MOSI (Master Output Slave Input) | D07 |  
|SPI 6 | GND (power supply ESP) | GND |  
|Pin 12 | SS (Slave Select) | D08 |  
   
If no further component connected via SPI (e.g. LAN shield, card reader) is used, the connection of "SS" (SlaveSelect, DUE pin 12 = D08 at ESP8266) can be omitted.  
In case of the use of SS the connection can also be made to another pin than pin 12, the corresponding pin must be defined accordingly in the file *BSB_lan_config.h*. In this case, however, it must be ensured that the pin to be used is not one of the protected pins and is not used elsewhere. It is therefore recommended to leave it at the default setting (pin 12).  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Wemos_SPI.jpg">  
  
*The corresponding connectors at the Wemos D1.*  
     
It is suitable to remove the LAN shield, place an unpopulated circuit board on the Due and provide it with the appropriate connections. So the Wemos D1 / NodeMCU can be placed stable onto the Due. Depending on the housing, the height may have to be taken into account.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Due_WiFi.jpg">  
  
*Wemos D1 at an empty circuit board onto the Arduino Due.*
   
*Note:*  
However, this solution does not allow data to be logged to a microSD card. If this still should be possible using the WiFi connection, either a corresponding card module must be connected additionally or the ESP must be connected in parallel to the existing LAN shield. In both cases, the SS pin *must* be connected (see pin assignment/connection). If a parallel usage of LAN shield and ESP8266 is possible without problems has not been tested yet though.
   
**Flashing the ESP8266:**  
The ESP8266 must be flashed with a special firmware. For the use of the Arduino IDE it must be ensured that the corresponding ESP8266 libraries have been installed before by using the board manager.  
The required firmware [WiFiSpiESP](https://github.com/JiriBilek/WiFiSpiESP) is already available as a zip-file in the BSB-LAN repository. The zip-file *must be unpacked in another folder than BSB_lan*! The ESP8266 has then to be flashed with the file *WiFiSPIESP.ino*.
  
**Configuration of BSB-LAN:**  
To use the WiFi function, the definement `#define WIFI` must be activated in the file *BSB_lan_config.h*. Furthermore, the two variables `wifi_ssid` and `wifi_pass` must be adapted accordingly and the SSID of the WLAN and the password must be entered. These entries can also be changed afterwards via the web interface. 
  
*Notes:*  
- When using DHCP, the IP address assigned by the router can be read out in the Serial Monitor of the Arduino IDE when starting the DUE.
- When using the ESP WiFi solution, the host name is *not* WIZnetXYZXYZ, but usually ESP-XYZXYZ, where the digit-letter combination "XYZXYZ" after "ESP-" is composed of the last three bytes (the last six characters) of the MAC address of the ESP.  
- When using the ESP WiFi solution, the MAC address of the ESP *can't* be set on your own.    
   
---
   
## 12.2 The ESP32
  
***Attention: We have tested a lot, but ALL functions etc. we have not been able to test. If you encounter any problems, incompatibilities, function restrictions or general bugs regarding the ESP32 usage, please report it (ideally in English as an issue in the repo)!***   
  
BSB-LAN is also executable on an ESP32. However, it is mandatory to make certain adjustments: 
- Remove (or move) the two folders "ArduinoMDNS" and "WiFiSpi" from the BSB-LAN subfolder "src" - these must no longer be present in the "BSB-LAN" or "src" folder!  
- Activate the definement `#define WIFI` in the file *BSB_LAN_config.h*!  
- Enter the access data for your WLAN (SSID and password)!  

Furthermore you have to pay attention to the following when using the Arduino IDE:  
- In the Arduino IDE the ESP32 platform must be installed and available in the board manager. 
*Note: For the Joy-It board recommended in the following chapter, a [user manual](https://joy-it.net/files/files/Produkte/SBC-NodeMCU-ESP32/SBC-NodeMCU-ESP32-Manual-20200320.pdf) is available from the manufacturer. There, in addition to the board-specific pinout scheme, is also a general guide to installing and using ESP32 boards with the Arduino IDE!*.     
- Select the appropriate ESP32 board type and port in the Arduino IDE. If you use the recommended Joy-It board or an identical clone with a "WROOM32" chip, you have to select "ESP32 Dev Module" as board type in the Arduino IDE.  
- Set the transfer speed/baud rate to 115200 (Attention: Per default the Arduino IDE usually sets 921600 for ESP32 boards).  
- Please select the variant "Default 4MB with spiffs (1.2BM APP/1.5MB SPIFFS)" for "Partition Scheme".  
  
**Note: If BSB-LAN cannot connect to WiFi on ESP32, it will set up its own access point "BSB-LAN" with password "BSB-LPB-PPS-LAN" for 30 minutes. After that, it will reboot and try to connect again.**    
  
*Note: Even though the logging function also works with the ESP32, it is not advisable to use that function excessively due to the wear of the flash memory.*   
  
  
---

### 12.2.1 ESP32 With Specific "BSB-LAN ESP32"-Adapter  
  
***Attention: We have tested a lot, but ALL functions etc. we have not been able to test. If you encounter any problems, incompatibilities, function restrictions or general bugs regarding the ESP32 usage, please report it (ideally in English as an issue in the repo)!***   

For a specific ESP32 board variant there is a separate BSB-LAN adapter board: "BSB-LAN ESP32".  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/ESP32-PCB.jpeg">  

*The "BSB-LAN ESP32" adapter board, unpopulated.*  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/ESP32-PCB_assembled.jpeg">  

*The "BSB-LAN ESP32" adapter board, assembled.*    
  
This BSB-LAN adapter board is designed for the 30 pin [ESP32 NodeMCU board from Joy-It](https://joy-it.net/en/products/SBC-NodeMCU-ESP32) (WROOM32 chip). A [user manual](https://joy-it.net/files/files/Produkte/SBC-NodeMCU-ESP32/SBC-NodeMCU-ESP32-Manual-20200320.pdf) is available for the board from the manufacturer. There are both the board-specific pinout scheme and a general guide to using ESP32 boards with the Arduino IDE!   
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/ESP32+Adapter.jpeg">  
  
*The Joy-It ESP32-NodeMCU on the "BSB-LAN ESP32" adapter.*  
  
If the Joy-It board is not available and another NodeMCU-ESP32 board is used, two things must be taken care of in any case, so that the ESP32-specific BSB-LAN adapter fits:  
1. The board *must* be a **30 pin** ESP32 NodeMCU! There are also 38 pin NodeMCUs - these do *not* fit!  
2. The pinout scheme *must* be identical to that of the Joy-It board.   
  
  
***Note:***  
***When using the Joy-It-Board or an identical clone with a "WROOM32" chip, "ESP32 Dev Module" must be selected as board type in the Arduino IDE.***  
  

---
  
### 12.2.2 ESP32 With Due-Compatible BSB-LAN-Adapter From V3  
  
***Attention: We have tested a lot, but ALL functions etc. we have not been able to test. If you encounter any problems, incompatibilities, function restrictions or general bugs regarding the ESP32 usage, please report it (ideally in English as an issue in the repo)!***   
  
The previous Due-compatible adapter (from v3) can also be used with an ESP32. The EEPROM of the adapter is not needed/used here and is accordingly also not to be considered with the wiring.  
With the choice of an ESP32 here is no compelling restriction on the Joy-It-board-compatible NodeMCU variant mentioned before, since a 'loose' wiring or the self-construction of a small adapter board for the more stable admission of the BSB-LAN adapter and the ESP32 is necessary anyway. However, care should be taken to ensure that the pin numbers/assignments given below match those of the selected ESP32.  
  
The connections are to be made as follows: 

| BSB-LAN adapter from v3 | Function | ESP32 board |
|:---------------:|:-----------:|:---------:|
| Pin 53 | VCC (power supply adapter) | 3,3V |
| GND | GND (power supply adapter) | GND |
| TX1 | TX (send) | Pin 17 (TX2) |
| RX1 | RX (receive) | Pin 16 (RX2) | 
  
As an example, an "ESP32 D1 R32 developer board" (WROOM32 chip) in the size of an Arduino Uno with a self-made adapter board (Uno-compatible prototyping board) for the inclusion of the BSB-LAN adapter v3 (Due version) is shown below. Of course, other variants are also possible, such as with an ESP32 NodeMCU and an appropriately adapted breadboard.  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/D1R32-Due_adapter.jpg">  
  
*Left the "ESP32 D1 R32" board, right the corresponding plug-on board for the BSB-LAN adapter v3 (due version).*  

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/D1R32+Due-adapter.jpg">  
  
*The complete Assembly.*  
  
  
---  
  
### 12.2.3 ESP32 With Due-Compatible BSB-LAN-Adapter V2  
  
***Attention: We have tested a lot, but ALL functions etc. we have not been able to test. If you encounter any problems, incompatibilities, function restrictions or general bugs regarding the ESP32 usage, please report it (ideally in English as an issue in the repo)!***   
    
The BSB-LAN adapter v2 can also be operated on an ESP32. In this way it is possible to benefit from the further development and the new functions of the BSB-LAN software from v2.x without having to purchase a new adapter. To do this, some changes must be made to the adapter itself, which are described below.  
*Caution: The steps described below to 'convert' the adapter to 3.3V are only valid for use on an ESP32 - on a Due the adapter v2 cannot be used due to the missing EEPROM!*       
    
To successfully operate the adapter v2 on an ESP32, the adapter must be 'adjusted' to operate with 3.3V. This is already provided for use with a Raspberry Pi. The following steps need to be taken:  
- The adapter must be *completely* assembled. If the adapter is so far only equipped for use with the Arduino Mega 2560, the following components must be retrofitted:  
    - 1x resistor 4.7kΩ (→ R11)
    - 2x resistor 10kΩ (→ R12, R13)
    - 1x transistor BC557A (→ Q11)
    - 1x transistor BC547A (→ Q12)
- The solder jumpers *SJ2* and *SJ3* are to be *closed* by a solder point.  
- The solder jumper *SJ1* is to be *removed*.  

Now the adapter is prepared for operation on a 3.3V system.  
For connection to the ESP now the "RasPi" contact row must be used and connected to the ESP32 as follows:

| BSB-LAN adapter v2 | Function | ESP32 board |
|:---------------:|:-----------:|:---------:|
| Pin 06 | GND (power supply adapter) | GND |
| Pin 08 | TX (send) | Pin 17 (TX2) |
| Pin 10 | RX (receive) | Pin 16 (RX2) |
| Pin 12 | 3,3V (power supply adapter) | 3,3V |     

The following picture shows a correspondingly equipped adapter v2. The yellow "X" at SJ1 marks the *removed* solder jumper (the non-closed contact), the two yellow outlines at SJ2 and SJ3 mark the solder jumpers *to be closed*.    
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/adapter_v2-ESP.jpeg">  
  
*The adjusted adapter v2 for use with an ESP32.*  
  
It is advisable to solder additional pins for the four contacts on the adapter and build yourself a small adapter board from a perforated board and pin headers, on which the adapter and the ESP32 board can be plugged to ensure a stable setup and a secure connection.
        
   
---   
   
## 12.3 Usage of Optional Sensors: DHT22, DS18B20, BME280
  
***ATTENTION: The GPIOs of the Arduino Due are only 3.3v compatible!***  
  
There is the possibility to connect additional sensors directly to certain pins of the adapter or the Arduino:  
- DHT22 (temperature, humidity; parameter numbers 20100-20199)
- DS18B20 (OneWire sensor: temperature; parameter numbers 20300-20399)
- BME280 (temperature, humidity, pressure; parameter numbers 20200-20299)  

The necessary libraries for the Arduino IDE are already included in the repository of the BSB-LAN software.  

Usually, the sensors can be connected to GND and +3,3V of the adapter/Arduino (by usage of the necessary additional pullup-resistors!).  
For the usage of these sensors, one has to activate the belonging definements in the file *BSB_lan_config.h* and has to set the specific pins which are used for DATA (also see [chapter 5](chap05.md)). Make sure you don't use any of the protected pins listed in the file *BSB_lan_config.h*! 
  
After successful installation you can access the values of the sensors either by clicking at the button "sensors" at the top of the webinterface, by clicking at the category "One Wire, DHT & MAX! Sensors" or by using the url command with the specific number of that category.  
   
Besides that, they are also displayed in the [IPWE extension](chap08.md#826-ipwe-extension) by default, which can be accessed by using the URL `<ip-address>/ipwe.cgi`. For using the IPWE extension, one has to activate the belonging definement in the file *BSB_lan_config.h* though.  
   
If you want to log the measured values or if you want to create 24h average calculations, you can realize that by adjusting the belonging parameters in the file *BSB\_lan\_config.h*.  
werden.
  
---
    

### 12.3.1 Notes on DHT22 Temperature/Humidity Sensors
  
***ATTENTION: The GPIOs of the Arduino Due are only 3.3v compatible!***  
  
DHT22 sensors are often advertised as "1 wire", but they are NOT part of the real OneWire bus system by Maxim Integrated and aren't compatible with these components.  
Furthermore they are not even part of any real bus system, because the sensors don't have any specific sensor id and can't be connected to the same DATA-pin if you are using more than one sensor.  
     
Usually these sensors have four pins, but only three of these are connected internally. Most in the time it's the third pin from the left (when viewed from the front) which isn't connected, but you should verify this before soldering.  
The most common pinout is:    
- Pin 1 = VCC (+)  
- Pin 2 = DATA  
- Pin 3 = usually not connected  
- Pin 4 = GND (-)  

When you connect the sensor, an additional pullup resistance has to be placed between VCC (pin 1) and DATA (pin 2) which should be in the range between 4,7kΩ to 10kΩ. In most cases a value of 10kΩ is suggested, but this should be determined individually (especially if any problems with the sensor occur).  
   
***Please note:***    
*If more than one DHT22 sensor should be used, you have to use an own pin at the Arduino for each DATA pin of the sensor. Furthermore you have to define them in the file BSB\_LAN\_config.h.*  
        
Besides the 'plain' sensors there are models which are already soldered onto a little circuit board, where the three necessary pins are lead out and labeled. The following picture shows one of these types with the identical sensor AM2302.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/AM2302.jpg">  
      
The query of the sensors/measured values can be done either via direct parameter call (`URL/20100-20199`) or by calling the corresponding category. The following screenshot shows the web output of a connected DHT22 sensor.  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/DHT22_web.png">  
  
*Display of the measured values of a DHT22 in the web interface (category "One Wire, DHT & MAX! Sensors").*  
  
*Note:*  
*You can find various tutorials and examples within the internet about the installation and usage of DHT22 sensors.*
        
---
    
### 12.3.2 Notes on DS18B20 Temperature Sensors
  
***ATTENTION: The GPIOs of the Arduino Due are only 3.3v compatible!***  
   
Sensors of the type DS18B20 are 'real' 1-wire/OneWire components of Maxim Integrated (initially Dallas Semiconductor).  
Each sensors has a unique internal sensor id which allows the clear identification of a certain sensor within a more complex installation of the bus system - if you wrote down the specific id for each sensor (regard the note in [chapter 12.3](chap12.md#123-usage-of-optional-sensors-dht22-and-ds18b20)).  
Besides the regular TO-92 type they are also available as waterproof capsuled types, which already have a cable connected.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/DS18B20.jpg">  

Especially for the usage within heating system installations the capsuled type is very interesting, because you can realize an individual (and waterproof!) installation easily and const-effective.  
         
The query of the sensors/measured values can be done either via direct parameter call (`URL/20300-20399`) or by calling the corresponding category. The following screenshot shows the web output of four DS18B20 sensors connected to pin 7.    
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/DS18B20_web.png">  
  
*Display of the measured values of four DS18B20 in the web interface (category "One Wire, DHT & MAX! Sensors").*   
   
***Note:***  
*If you are using DS18B20 sensors, the specific sensor id of each sensor will also be listed within the output of the category sensors (and the output of the IPWE extension, if used). Especially if more than one sensor will be added to the system, these unique sensor ids are necessary to identify a specific sensor later. So if you integrate BSB-LAN and/or these sensors in your home automation software, you should consider this (e.g. use RegEx on the sensor ids).*  
*It's adviseable to read out the sensor id (e.g. by using /K49) and label each sensor, so that you don't get confused later. For this, you can raise or lower the temperature of one sensor (e.g. hold it in your hand) and query the category sensors again after a certain time. Now you can see the changed value of one sensor and write down the specific sensor id.*  
*Besides that, if any sensor will be exchanged or added, most of the time the displayed order (within the output of the category sensors or the IPWE extension) of the sensors will change also, because internally they are listed following the specific sensor ids. So if you only adjust the reading following the order and name the sensors like that, it can happen, that the belonging name doesn't show the correct sensor anymore. The following screenshots show this circumstance.*  
*If any changes within the installation of the sensors occur (e.g. if you exchange, add or remove something), you have to reboot the Arduio, so that the sensors will be initially read out and added to the software.*  
     
   
***Notes on the elecrtical installation:***  
Each sensor usually offers three pins: VCC, DATA and GND.  
Within the capsuled types, the colors of belonging wires are often as follows:  
- Red = VCC (+3,3V)  
- Yellow = DATA  
- Black = GND (-)  
   
If you are using more than one sensor and/or larger cable lengths, it's advisable to add a 100nF ceramic capacitor (and maybe also an addditional 10µF tantal capacitor) for each sensor. The capacitors should be added as close as possible to the sensor and need to be connected between GND and VCC so that a brownout at the time of the query will be compensated.  
   
Besides the (optional but advisable) usage of capacitors, you have to use a pullup resistance (only one!) at the output of the adapter/Arduino and place it between DATA and VCC (+3,3V). If you are using more than one sensor and/or larger cable lengths, you probably have to evaluate the correct dimension of the resistor, which can be smaller than the 4,7kΩ which is suggested most of the times.  
  
Furthermore, in more complex or larger installations, it seems in individual cases that the voltage supply with the 3.3V of the Due does not always allow a problem-free operation of the sensors. Since these OneWire sensors are "open drain", they can also be operated with 5V of the Due, which seems to result in a more stable operation. However, it must then be ensured that the 5V is *never* applied to the GPIO of the Due!  
*For the installation this means that VCC of the sensors is connected to the 5V pin of the Due, but the PullUp resistor to be used must be placed between DATA and a 3.3V pin of the Due!*   
   
*Notes:*  
- If you are using the mentioned capsuled and already wired types, it's usually sufficient to place the capacitors where the wires will be connected. So you don't have to cut the wires at the capsule to place the capacitor there (according to experience, at least with the types which come with a cable length of 1m or 3m it's not necessary).  
- In contrary to ceramic capacitors you have to pay attention to the correct polarity if you are using additional tantal capacitors!  
- It's not advisable to use the 'parsite power mode'.  
- It's advisable to use a shielded cable for the connection. The shield should be connected to GND at one end of the cable. 
- To minimize the risk of electrical interference, try not to lead the cable parallel to power cords. Besides that, you can also add a ferrite ring to minimize the risk of electrical interference which maybe can come from the power supply of the Arduino. Just lead the cable a few times through the ferrite ring.   
   
If you have to use *larger* cable lengths, it's necessary to pay attention to the correct network topology. Have a look at the tutorial which was written from the manufacturer: "[Guidelines for Reliable Long Line 1-Wire Networks](https://www.maximintegrated.com/en/design/technical-documents/tutorials/1/148.html)".  
   
*Note:*  
*You can find various tutorials and examples within the internet about the installation and usage of DS18B20 sensors.*  
   
***Summary of needed parts for an installation:***  
- three-wired cable (if shielded, connect the shield at one end to GND)  
- one pullup resistance 4,7kΩ or maybe smaller, positioned between VCC and DATA at the adapter/Arduino   
- ceramic capacitor 100nF, one for each sensor, positioned between VCC and GND close to the sensor  
- optional: tantal capacitor 10µF, one for each sensor (additional to the ceramic capacitor!), positioned between VCC and GND close to the sensor (please pay attention to the correct polarity!)  
- optional: screw terminals, circuit board, housing, ...   
   
***Notes for the usage within your heating system installation:***  
- If you want to use the capsuled types of sensors, especially within bigger installations it can be adviseable to use the version with 3m cable instead of 1m cable. They are only a little bit more expensive but offer a greater freedom of movement when you want to place the sensors.  
- If you want to place the sensors at some pipes, it's adviseable to create a little bed made of thermal paste for the contact area. Fasten the sensor with a metal pipe clamp to the pipe and also fasten the cable itself with a cable tie, so that tensile forces won't work on the sensor itself and that the sensors stays in place. Of course you need to place the sensor between the pipe an the insulation and close the insulation after you are done with the installation. If there is no pipe insulation it's advisable to -at least- cover the sensor with a piece of insulation, so that it's not affected by any cold air or so.   
- In general, the sensors should me mounted one or two meters away from a heat source, so that they aren't affected by that.  
  
***Please note:***  
***Already installed sensors which belong to the heating system (e.g. sensors for a warm water tank or a heating buffer tank) are always more important than any sensor for your home automation system! The given installation of your existent heating system should never be adversely affected by any optional installed DS18B20 sensor!***  
        
***Construction plan:***  
If you want to set up an installation with more than one sensor and the common capsuled sensors with 1m or 3m cable length, you can build a little 'distribution box'. For this, you can solder the connection wires of the sensors and the belonging capacitors in line onto a circuit board. If you use screw terminals instead of soldering the sensors straight to the board, you can easily add or exchange sensors later. At the 'beginning' of this board, you connect the cable which leads to the adapter/Arduino. The following pictures show two of these little 'distribution boxes' I made - they work perfectly.    
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Verteiler_klein.jpg">  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Verteiler_groß.jpg">  
   
---
   
### 12.3.3 Notes on BME280 Sensors
  
***ATTENTION: The GPIOs of the Arduino Due are only 3.3v compatible!***  
  
Sensors of the BME280 type offer three (or five) measured variables: Temperature, humidity (plus the calculated absolute humidity) and air pressure (plus the calculated altitude). They are small, usually uncomplicated to connect and provide (sufficiently) accurate measurement results.  
**Up to two sensors of the type BME280 can be connected to the I2C bus of the Arduino Due (also to the Mega 2560).** To use them, the corresponding definition in the file *BSB_lan_config.h* must be activated and the number of connected sensors must be defined ([see chapter 5.2](chap05.md#52-configuration-by-adjusting-the-settings-within-bsb_lan_configh)).  
*Note: In principle BME280 can also be connected to an SPI, but* ***not*** *on the Arduino of our BSB-LAN setup!*  

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/BME280_double.jpg">  
    
*A BME280 sensor on a typical breakout board (clone); left = front, right = back.*  
  
The following points must be observed:  
- Make sure that the sensor is of type BME280 (and not e.g. a BMP280, BMP180,..).  
- Make sure that you use a module that already has pull-up resistors on the breakout board (like on the picture above). If your variant does *not* have pull-up resistors installed, you have to add them when connecting it to the Arduino (approx. 10kOhm, connect between SDA and 3.3V and between SCL and 3.3V)!  
- Make sure that the first sensor has the I2C address 0x76! This is usually the case with the module shown above.  
- The second sensor must get the address 0x77. How to do this on the module shown above is described below.  
- Make sure you connect the sensor to the 3.3V pin of the Arduino! The module shown above has a voltage regulator and level shifter built in, so in this case you *could* connect it to 5V - but to make sure that there is never 5V at SDA/SCL, you should always prefer to connect it to 3.3V.

  
**Connection**  
  
The breakout board is usually already clearly labeled, so the connections can be clearly identified here.  
Depending on the Arduino used, a different I2C connector must be used:  
- The **Due** has two I2C bus connections: SDA/SCL at pins 20/21 and SDA1/SCL1. Care must be taken to use the **SDA1 & SCL1** connectors, as the BSB-LAN adapter already uses the SDA/SCL connectors. SDA1/SCL1 are located next to the "AREF" pin. They are usually covered by the LAN-Shield and are not carried out upwards to/through the LAN shield. However, they are accessible below the LAN shield directly on the Due. For an exact positioning of SDA1/SCL1 please have a look at the [pinout diagram in appendix B](appendix_b.md).  
- The **Mega 2560**, on the other hand, has only one I2C bus connector: SDA/SCL on pins 20/21. This is not occupied by the old adapter v2, the connector can be used for the BME280.  

The wiring has to be done as follows:  

| BME280 | DUE | Mega2560 |
|:------:|:---:|:--------:|
| VIN | 3,3V | 3,3V |
| GND | GND | GND |
| SDA | SDA1 | SDA 20 |
| SCL | SCL1 | SCL 21 |
  
**Addressing**  
  
Common breakout boards like the BME280 module shown above have three solder pads on the front side below the actual sensor, where usually the *left* and the middle solder pad are connected by a conducting path. This usually corresponds to the address 0x76. The following picture shows this connection circled in yellow.  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/BME280_address76.jpg">  
    
*Address 0x76: trace between the left and the middle solder point.*  


If you want to connect a second sensor in parallel, you have to cut this conducting path carefully(!) and conscientiously with a fine sharp object (e.g. cutter, scalpel). After that the *right* and the middle pin have to be connected by some solder. The following picture shows the necessary steps: the red line on the left marks the necessary 'cut' on the board, the green line on the right marks the connection to be made afterwards using solder.  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/BME280_address77.jpg">  
    
*Address 0x77: The red line marks the cut trace, the green line marks the new connection to be made.*  
  
**Readout**  
  
The measured values of the connected BME280(s) can be read out as usual, e.g. by calling up the category "One Wire, DHT & MAX! Sensors" under "Heating Functions", by a direct click on the button "Sensors" or also by entering the specific parameter numbers. BME280 sensors can be found in the number range 'URL/20200-20299'. If a logging, a display within the IPWE extension etc. should be done, the specific parameter numbers of the desired measured values of the respective sensor have to be used.  
The following screenshot shows the corresponding display of a BME280 within the category "One Wire, DHT & MAX! sensors".   
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/BME280_screenshot.png">  
    
*Display of the measured values of a BME280 in the web interface (category "One Wire, DHT & MAX! Sensors".*  
  
  
---
    
## 12.4 Relays and Relayboards
  
***ATTENTION: The GPIOs of the Arduino Due are only 3.3v compatible!***  
  
In general it's possible and within BSB-LAN already implemented to connect and query a relay which is connected to the Arduino. By this one couldn't only change the state of a relay by sending a specific command, it's also possible to just query the state.  
***It is NOT possible to connect the Arduino directly with the multifunctional inputs of the controller!***  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/relaisboards.jpg">  

*A single and a 4-channel relaymodule for the usage with an Arduino.*  
       
The often cheap relaymodules available for the usage with an Arduino are often already supplied with a relay which can handle high voltage like 125V or 230V. However, due to poor quality or just an overload, different risky damage can occur. Because of that one should consider to (additionally) use common couple or solid state relays which are used by electricians. in that case one should see the specific data sheet to confirm that the electrical current of the Arduino is strong enough to trigger the swithcing process of the relay.  
   
***WATCH OUT:***  
***Electrical installations should only be done by an electrician! High voltage like 230V or 125V can be deadly!*** *It's adviseable to already include an electrician at the state of planning.*   
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/koppelrelais.jpg">  
   
*A common coupling relay. At this specific type, the corresponding pins at the Arduino have to be connected with "14" and "13".*  
      

*Example:*  
If the controller of a solarthermic installation isn't already connected with the controller of the heating system, it's possible to query the state of the pump by installing a coupling relay parallel to the pump and connect the other 'side' of the relay with the specific pins of the Arduino. Now you can query the state of the relay and therefore the state of the pump with the Arduino.  
    
---
     
## 12.5 MAX! Components
BSB-LAN is already prepared for the usage of MAX! heating system components. MAX! thermostats that shall be included into BSB-LAN, have to be entered with their serial number (printed on a small label, sometimes in the battery compartment) in the file *BSB\_lan\_config.h* into the array `max_device_list[]`. After starting BSB-LAN, the pairing button has to be pressed on the thermostats in order to establish a connection between BSB-LAN and the thermostats.  
  
In *BSB\_lan\_custom.h* you can use the following variables for using MAX! devices:  
  
- `custom_timer`  
This variable is set to the value of millis() with each iteration of the loop() function.  
  
- `custom_timer_compare`  
This variable can be used in conjunction with `custom_timer` to create timed executions of tasks, for example to execute a function every x milliseconds.  
  
In addition to that, all global variables from *BSB\_lan.ino* are available. In regard to MAX! functionality, these are most notably:  
  
- `max_devices[]`  
This array contains the DeviceID of each paired MAX! device. You can use this for example to exclude specific thermostats from calculations etc.  
  
- `max_cur_temp[]`  
This array contains the current temperature of each thermostat. However, only temperatures from wall thermostats are reliable because they transmit their temperature constantly and regularly. Other thermostats do this only when there is a change in the valve opening or upon a new time schedule.   
  
- `max_dst_temp[]`  
This array contains the desired temperature of each thermostat.  
  
- `max_valve[]`  
This array contains the current valve opening of a thermostat (wall thermostats only carry this value when they are paired with a heater thermostat).  
  
The order inside of these arrays is always the same, i.e. if `max_devices[3]` is wall thermostat with ID xyz in the living room, then `max_cur_temp[3]` contains the current temperature in the living room, `max_dst_temp[3]` the desired temperature in the living room etc.  
  
The order inside `max_devices[]` depends on how the devices have been paired with BSB-LAN and remains the same after restarts of BSB-LAN since they are stored in EEPROM until this is erased by calling `http://<IP-Adresse>/N`. However, one should not completely rely on this and rather compare the ID stored in `max_device[]` for example when planning to ignore a specific thermostat in some kind of calculations. You can obtain this ID from the second column of `http://<IP-Adresse>/X` and take note that this is not the same as the ID printed on the label.  
  
Important note for those users who use a Max!Cube that has been flashed to CUL/CUNO (see information [here](https://forum.fhem.de/index.php/topic,38404.0.html)):  
If BSB-LAN was not running (or was busy otherwise) when the CUNO was set up, then you have to press the pairing button again on these devices, because only in that specific pairing process the ID printed on the devices label is sent together with the internally used device ID (and is also used by FHEM).  
   
You can also use the MAX! thermostats to calculate a weighted or average current or desired temperature (see [here](https://wiki.fhem.de/wiki/MAX) for configuring MAX devices under FHEM and [here](https://forum.fhem.de/index.php/topic,60900.0.html) for using the average temperature in FHEM).  
  
FHEM forum user *„Andreas29"* has created an example on how to use MAX! thermostats with BSB-LAN without using FHEM. A detailed description can be found in this forum post [here](https://forum.fhem.de/index.php/topic,29762.msg851382.html#msg851382). The "Arduino room controller light" is described in chapter [12.6.2](chap12.md#1262-room-temperature-sensor-wemos-d1-mini-dht22-display).  
    
---
    
## 12.6 Own Hardwaresolutions
  
The following solutions have been developed by BSB-LAN users. They should not only be a stimulation for re-building but also an example what's possible with additional own built hardware solutions in combination with BSB-LAN.  
   
If you also created something by your own of which you think that it could be interesting for other users, please feel free to contact me (Ulf) via email at `adapter (at) quantentunnel.de`, so that I eventually can present it here in the manual. Thanks!  
    
---
    
### 12.6.1 Substitute for a Room Unit (Arduino Uno, LAN Shield, DHT22, Display, Push Button Switch)
The member *„Andreas29"* of the German FHEM forum has built a substitute for a room unit, based on an Arduino Uno. Besides the data from a DHT22 sensor, the current state of function of the heating system is displayed on a 4x20 LCD. With a little push button he imitates the function of the presence button of a common room unit.  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Raumgerät_light_innen.jpg">
    
*The 'inside' of his substitute of a room unit.*  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Raumgerät_light_Display.jpg">
    
*The display of his own built room unit.*  
    
A more detailed description including the circuit diagram and the software is available [here](https://forum.fhem.de/index.php/topic,91867.0.html) in the German FHEM forum.
   
Also, he expanded the functionality and implemented push messaging for certain error cases. The description and the software can be found [here](https://forum.fhem.de/index.php/topic,29762.msg878214.html#msg878214) in the German FHEM forum.
    
---
    
    
### 12.6.2 Room Temperature Sensor (Wemos D1 mini, DHT22, Display)
The member *„Gizmo\_the\_great"* of the FHEM forum has built a room temperature sensor based on a Wemos D1 mini and a DHT22 sensor. The current temperatures on the heating circuits 1 and 2 are additionally displayed at an OLED display. The Wemos D1 ist running ESPeasy.  

A more detailed description of his project you can find in [his GitHub Repo](https://github.com/DaddySun/Smart_Home_DIY).
     
---
  
### 12.6.3 Substitute for a Room Unit with UDP Communication (LAN Connection)
  
FHEM forum member *"fabulous "* has built a substitute for a room unit based on the above-mentioned variant of user "Andreas29", which communicates with the BSB LAN adapter via UDP. An Arduino Uno including LAN shield, a 20x4 LCD and a push button are used. A detailed description and the corresponding code can be found [here](https://forum.fhem.de/index.php/topic,110599.0.html).  
   
  
---
       
## 12.7 LAN Options for the BSB-LPB-LAN Adapter
Even though the wired LAN connection is definitely the best option for integrating BSB-LAN into your network, it could be necessary to create an alternative way of connection, because a full-range wired connection (bus cable or LAN cable) just isn't possible.  
      
---
    
### 12.7.1 Usage of a PowerLAN / dLAN
The use of powerline adapters for expanding the LAN is an option, which could be the best and most reliable solution.  
However, sometimes powerline installations can cause trouble because of possible interferences they may cause. If you have separated phases within your electrical installation, it may just not work though. In that case ask an electrician about a phase coupler that he may could install.    
    
---
    
### 12.7.2 WLAN: Usage of an Additional Router
Another option is to connect the Arduino via LAN with an old WLAN router (e.g. an old FritzBox) and integrate the router in your network via WLAN as a client. The speed of transmission usually is fast enough for the use of BSB-LAN. If the WLAN signal is weak, you can probably try to change the antennas and mount bigger ones.  
  
In addition to the use of a 'normal' router, there are small devices on the market that offer a RJ45 jack and a WLAN client or a WLAN client bridge mode. These devices connect to the network via WLAN (like the FritzBox solution described above). The Arduino can be connected via LAN cable to the device. These kinds of devices are often very small and can be plugged in a power outlet, so that the installation of the hardware can usually be done quite easily.   

However, a stable and reliable WLAN connection should be achieved. Especially, if you are using additional smart home software to create logfiles, if you are using additional hardware like thermostats or if you want to control and influence the behaviour of your heating system.  
    
        
---  
   
## 12.8 Housing
The market offers just a small range of housings which are compatible for an Arduino Due plus additional shields. If you search for them, you probably won't find anything. In that case look out for housing which are designed for an Arduino Mega 2560, because it has the same form factor as the Due. Try to find a housing, which can accommodate the whole setup including the LAN shield though, because many housings are only designed to accommodate the plain Mega. This kind of housing has some cutouts in the top cover to plug in additional shields, but in that case the LAN shield and the adapter won't be protected at all.  
  
Besides commercial products and creative own built solutions, a 3D printer could be used to create a great housing.  
**The member "EPo" of the German FHEM forum was so kind to create and offer STL datafiles for a housing.**  
**Thanks a lot!**  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/BSB-Gehaeuse.jpg">  
  
*3D printer model of the housing for the Arduino Due, the LAN-Shield and the adapter v3.*  
  
The STL data files are already included in the repository of BSB-LAN.  
   
---  
## 12.9 Raspberry Pi

The adapter v3 could also be used in conjunction with a Raspberry Pi. Therefore you have to pay attention to some points: 
- **A usage of the BSB-LAN-Software is NOT possible (see notes below)!**  
- You only have to use double-rowed female headers which fit the RPi pins (instead of the pin headers for the usage with an Arduino Due!).
- With the complete length of the female headers (6 pins 'long', so 12 pins in summary) the first pair of the adapter must NOT be plugged to the first pair of the RPi pins (1/2), you have to start with the second pair of the RPi pins (3/4). 
In other words: make sure that the pin of the adapter labeled as TX1 will fit on the RPi pin 8 (= GPIO 14, UART0_TXD), the pin RX1 in the RPi pin 10 (= GPIO 15, UART0_RXD) and so on.  
*Note:* This counting refers to the official RPi pinout and the naming.  
The picture below shows the plain adapter *next to* the belonging RPi pins just to visualize the displacement/alignment on the longitudinal axis.
- Before the usage of the software, the Pin 7 (GPIO 4) of the RPi must be  
a) defined as an output pin and    
b) set to "HIGH" within the OS of the RPi to achieve the power supply of the adapter.  
Therefore your have to execute two commands in the terminal (probably with a leading 'sudo'):   
`gpio -1 mode 7 output`  
`gpio -1 write 7 1`  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/rpi_v3_ausrichtung.jpg">  
  
*Exemplary alignment of the adapter along the longitudinal axis of the RPi pins.*  
   
   
***IMPORTANT NOTES:***  
- ***For the usage of the adapter in conjunction with an RPi you have to use a complete different software: ["bsb_gateway"](https://github.com/loehnertj/bsbgateway) by J. Loehnert!***  
- *For any support please contact the author of bsb_gateway!*  
- *We can not and will not provide any support with regard to RPi use!*  
- *From our side, the use of the adapter with the above mentioned software was only tested on an RPi 2. We are not able to judge whether it works properly with more recent RPi versions!*   
- *For the usage of the adapter with an RPi at the PPS interface, the Python script [PPS-monitor](https://github.com/dspinellis/PPS-monitor) by D. Spinellis can be used.*  
  
***This manual only refers to BSB-LAN!***  
   
   
---  
   
[Further on to chapter 13](chap13.md)      
[Back to TOC](toc.md)   
