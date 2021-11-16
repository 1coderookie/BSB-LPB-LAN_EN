[Back to TOC](toc.md)  
[Back to Introduction](index.md)    
    
---
# 1. BSB-LAN: The Hardware   
   
In the following chapters the hardware of the BSB-LAN setup is introduced. On the one hand it is the respective BSB-LAN adapter and on the other hand the respective microcontroller on which the BSB-LAN software is flashed.  
BSB-LAN can be operated with an Arduino Due including a specific adapter as well as on an ESP32 including a specific adapter.     
  
---

## 1.1 Adapter

The BSB-LAN adapter is available in two different versions. On the one hand as an Arduino Due specific version with an EEPROM, on the other hand as an ESP32 specific version without EEPROM.   Depending on which microcontroller you want to use, you should choose the specific version, because the adapter can then be connected to the respective system comfortably and safely by plugging it in.  
  
| Note |
|:----|
| It should already be noted at this point that the ESP32-specific adapter version can only be used with an ESP32 due to the missing EEPROM - the Due-specific version, on the other hand, can also be used with an ESP32 (even if it cannot be plugged on comfortably). | 

---

### 1.1.1 Due Version
  
The Due-specific version of the BSB LAN adapter has an EEPROM in which the settings of the BSB LAN software (from v2.0) are stored. The adapter can be conveniently and securely plugged onto the Due. 

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-adapter-v3-unbestueckt-front.jpeg">

*The BSB-LAN adapter board v3, top side, unpopulated.*  
    
| Note |
|:----|
| Using the Due-specific adapter on an ESP32 is possible in principle, despite the EEPROM, but the adapter cannot be plugged onto an ESP32 board without problems, as is the case with a Due. If the adapter should nevertheless be used with an ESP32 board, care must be taken to ensure that the connections between the adapter and ESP32 are made correctly and reliably. | 

---

### 1.1.2 ESP32 Version

For a specific ESP32 board variant there is a separate BSB-LAN adapter board: "BSB-LAN ESP32".  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/ESP32-PCB.jpeg">  

*The "BSB-LAN ESP32" adapter board, unpopulated.*  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/ESP32-PCB_assembled.jpeg">  

*The "BSB-LAN ESP32" adapter board, assembled.*    
  
This BSB-LAN adapter board is designed for the *30 pin* [ESP32 NodeMCU board from Joy-It](https://joy-it.net/de/products/SBC-NodeMCU-ESP32) (WROOM32 chip).    
In addition, the adapter can also be used with an [Olimex ESP32-EVB](https://www.olimex.com/Products/IoT/ESP32/ESP32-EVB/open-source-hardware) and plugged directly into the ten-pin UEXT connector of Olimex boards by adding a double-row five-pin connector (2x5 pin, RM 2.54mm) on the bottom of the board.  
  
The ESP32 specific version of the BSB-LAN adapter has no EEPROM, settings are stored in the flash memory of the ESP32.  

| Note |
|:----|
| Using the ESP32 specific adapter on a Due is *not* possible due to the missing EEPROM! |  

---

## 1.2 Arduino Due
*In general, the use of an [original Arduino Due](https://store.arduino.cc/arduino-due) is recommended.*  
From experience, however, cheap replicas ("clones") of the Arduino Due can also be used, the use of these clones is usually possible without any problems. But: It should be paid attention if a modified board layout (e.g. changed pin assignments) is described in the prduct description. If this is the case and you still want to buy it, you may need to make specific adjustments in the file *BSB_lan_config.h*.
   
***ATTENTION: The GPIOs of the Arduino Due are only 3.3v compatible!***   
  
*A pinout diagram of the Arduino Due is available in [appendix b](appendix_b.md).*   
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/due_clone_pp.jpg">  
   
*A compatible clone of the Arduino Due.*  
   
| Notes |
|:----|
|  Regarding to the [tech specs of the Arduino Due](https://store.arduino.cc/arduino-due), it is recommended to use an external power source (recommended: 7-12V, limits: 6-16V) at the intended connection of the Arduino (e.g. 9V/1000mA). |  
| If you want to power the Due via USB, please use the "Programming Port". |  
| It's possible to power the Due via the DC-IN and use USB connection at the programming port for connecting it to the computer at the same time. | 
| You can let the adapter be connected to the controller bus of the heater when flashing the Due. |  
| Make sure that you are using an USB cable of good quality! This applies to the case that you want to power the Due via USB as well as to the case that you want to connect the Due to your PC for flashing. Especially long and thin cables (e.g. accessories of smartphones) can cause problems with the power supply and thus the stability of the Due and/or are not always fully wired, so that a use for data transfer is not possible. |  
| With some Due models/clones it can happen that they do not seem to work properly after an initial start (e.g. after a power failure) and only work correctly after pressing the reset button. A possible solution for this problem could be to [add a capacitor](https://forum.arduino.cc/index.php?topic=256771.msg2512504#msg2512504). |   
 
  
  
   
    
    
---
    
### 1.2.1 Due + LAN: The LAN Shield
*In general, the use of an [original Arduino LAN shield (v2)](https://store.arduino.cc/arduino-ethernet-shield-2) is recommended.*  
From experience, however, cheap replicas ("clones") of these LAN shields can also be used, the use of these clones is usually possible without any problems. But: It should be paid attention if a modified board layout (e.g. changed pin assignments) is described in the product description. If this is the case and you still want to buy it, you may need to make specific adjustments in the file *BSB_lan_config.h*.  
   
There are / have been two different versions of LAN shields available on the market: one with a WIZnet W5100 chip (v1) and one with a W5500 chip (v2). The usage of a v2-shield is recommended, it's also available at the official [Arduino store](https://store.arduino.cc/arduino-ethernet-shield-2).  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/lanshield_clone.jpg">  
   
*A compatible clone of a LAN shield with a W5100 chip.*  
       
| Note |
|:----|
| As a LAN cable one should preferably use a S/FTP type with a minimum length of one metre. |  
   
---  
   
### 1.2.2 Due + WLAN: The ESP8266-WiFi-Solution
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
In case of the use of SS the connection can also be made to another pin than pin 12, the corresponding pin must be defined accordingly in the file *BSB_LAN_config.h*. In this case, however, it must be ensured that the pin to be used is not one of the protected pins and is not used elsewhere. It is therefore recommended to leave it at the default setting (pin 12).  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Wemos_SPI.jpg">  
  
*The corresponding connectors at the Wemos D1.*  
     
It is suitable to remove the LAN shield, place an unpopulated circuit board on the Due and provide it with the appropriate connections. So the Wemos D1 / NodeMCU can be placed stable onto the Due. Depending on the housing, the height may have to be taken into account.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Due_WiFi.jpg">  
  
*Wemos D1 at an empty circuit board onto the Arduino Due.*
   
| Note |
|:----|
| However, this solution does not allow data to be logged to a microSD card. If this still should be possible using the WiFi connection, either a corresponding card module must be connected additionally or the ESP must be connected in parallel to the existing LAN shield. In both cases, the SS pin *must* be connected (see pin assignment/connection). <br> *If a parallel usage of LAN shield and ESP8266 is possible without problems has not been tested yet though.* |
   
**Flashing the ESP8266:**  
The ESP8266 must be flashed with a special firmware. For the use of the Arduino IDE (or other) it must be ensured that *version 2.7.4* of the corresponding ESP8266 libraries has been installed and chosen by using the board manager.  
  
The required firmware [WiFiSpiESP](https://github.com/JiriBilek/WiFiSpiESP) for the ESP8266 is already available as a zip-file in the BSB-LAN repository. The zip-file *must be unpacked in another folder than BSB_lan*! The ESP8266 has then to be flashed with the file *WiFiSPIESP.ino*.
  
**Configuration of BSB-LAN:**  
To use the WiFi function, the definement `#define WIFI` must be activated in the file *BSB_LAN_config.h*. Furthermore, the two variables `wifi_ssid` and `wifi_pass` must be adapted accordingly and the SSID of the WLAN and the password must be entered. These entries can also be changed afterwards via the web interface. 
  
| Notes |
|:----|
|  When using DHCP, the IP address assigned by the router can be read out in the Serial Monitor of the Arduino IDE when starting the DUE. |
| When using the ESP WiFi solution, the host name is *not* WIZnetXYZXYZ, but usually ESP-XYZXYZ, where the digit-letter combination "XYZXYZ" after "ESP-" is composed of the last three bytes (the last six characters) of the MAC address of the ESP. | 
| When using the ESP WiFi solution, the MAC address of the ESP *can't* be set on your own. |
   
---
   
## 1.3 ESP32

  
The BSB-LAN software can also be run on an ESP32. However, it is mandatory to make certain adjustments, which are described in chap. [2.1.2](chap02.md#212-installation-onto-the-esp32).

Basically any ESP32 can be used, but due to the specific board design the use of the [ESP32 NodeMCU board from Joy-It](https://joy-it.net/en/products/SBC-NodeMCU-ESP32) or the [Olimex ESP32-EVB](https://www.olimex.com/Products/IoT/ESP32/ESP32-EVB/open-source-hardware) is recommended.

    
---

### 1.3.1 ESP32 With Specific "BSB-LAN ESP32"-Adapter  


For a specific ESP32 board variant there is a separate BSB-LAN adapter board: "BSB-LAN ESP32".  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/ESP32-PCB.jpeg">  

*The "BSB-LAN ESP32" adapter board, unpopulated.*  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/ESP32-PCB_assembled.jpeg">  

*The "BSB-LAN ESP32" adapter board, assembled.*    
  
This BSB-LAN adapter board is designed for the *30 pin* [ESP32 NodeMCU board from Joy-It](https://joy-it.net/en/products/SBC-NodeMCU-ESP32) (WROOM32 chip).      
The ESP32 adapter version can also be used with an [Olimex ESP32-EVB](https://www.olimex.com/Products/IoT/ESP32/ESP32-EVB/open-source-hardware) and can be plugged onto the ten pin UEXT connector of Olimex boards by adding a double row five pin socket (female pinheader, 2x5 pins, grid dimension 2.54mm) to the bottom side of the PCB.   
 
--- 
 
#### 1.3.1.1 ESP32: NodeMCU "Joy-It"  
  
This BSB-LAN adapter board is designed for the *30 pin* [ESP32 NodeMCU board from Joy-It](https://joy-it.net/en/products/SBC-NodeMCU-ESP32) (WROOM32 chip). A [user manual](https://joy-it.net/files/files/Produkte/SBC-NodeMCU-ESP32/SBC-NodeMCU-ESP32-Manual-2021-06-29.pdf) is available for the board from the manufacturer. There are both the board-specific pinout scheme and a general guide to using ESP32 boards with the Arduino IDE!  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/ESP32+Adapter.jpeg">  
  
*The Joy-It ESP32-NodeMCU on the "BSB-LAN ESP32" adapter.*  
  
If the Joy-It board is not available and another NodeMCU-ESP32 board is used, two things must be taken care of in any case, so that the ESP32-specific BSB-LAN adapter fits:  
1. The board *must* be a **30 pin** ESP32 NodeMCU! There are also 38 pin NodeMCUs - these do *not* fit!  
2. The pinout scheme *must* be identical to that of the Joy-It board.   
  
  
---

#### 1.3.1.2 ESP32: Olimex ESP32-EVB 

The ESP32 adapter version can also be used with an [Olimex ESP32-EVB](https://www.olimex.com/Products/IoT/ESP32/ESP32-EVB/open-source-hardware) and can be plugged onto the ten pin UEXT connector of Olimex boards by adding a double row five pin socket (female pinheader, 2x5 pins, grid dimension 2.54mm) to the bottom side of the PCB.   
This Olimex board variant offers, among other things, a LAN port, a microSD card reader and two relays in addition to the ESP32-based WLAN functionality and is therefore highly recommended.  

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/OlimexESP32EVB_small.jpg">   
  
*The Olimex ESP32-EVB with the plugged on "BSB-LAN ESP32" adapter.*     
  
| Attention, important notes |
|:---------------------------|
| When plugging on the adapter board, make sure meticulously that the UEXT1 socket of the board is plugged on exactly in the middle of the Olimex socket and that all pins of the Olimex have contact! Otherwise, when the adapter is correctly connected to the heating controller, the LED of the adapter lights up, but no access to the controller is possible. |
| Adapter boards that are used on Olimex boards at the UEXT connector *and* have a BSB-LAN board revision up to and including 4.1 (and *only* in this combination) do not start correctly if the power supply was interrupted when the BSB-LAN adapter was plugged in. The reset button must then also be pressed once after switching on. <br> To solve this problem, you have to cut (marked yellow) the conductor path from resistor R6 in direction of the UEXT connector (marked red) with a sharp object (e.g. razor blade/carpet knife/scalpel). It's recommended to check with a multimeter before and after you do this to make sure that there really isn't a connection anymore between the end of R6 and pin 3 of the UEXT connector after cutting. Instead, a conductive connection must then be made using a thin wire between this end of R6 to pin 10 of the UEXT connector (below the "U" of "UEXT"; marked green). <br> <img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Olimex_fix_R6.jpg"> <br> **BSB-LAN boards from board revision 4.2 are no longer affected by this problem.** |


  
---
  
### 1.3.2 ESP32 With Due-Compatible BSB-LAN-Adapter From V3  
  
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

| Note |
|:-----|
| The ESP32 "D1 R32 developer board" shown below I personally can explicitly NOT recommend, because it obviously has much worse reception properties than other ESP32 boards. Although the router was only a few meters away, it was not possible for me to establish a stable WLAN connection. When I asked the seller, this impression was confirmed to me stating that the "cause for this is rooted in the design". |
 
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/D1R32-Due_adapter.jpg">  
  
*Left the "ESP32 D1 R32" board, right the corresponding selfmade plug-on board for the BSB-LAN adapter v3 (due version).*  

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/D1R32+Due-adapter.jpg">  
  
*The complete assembly.*  
  
  
---  
  
### 1.3.3 ESP32 With Due-Compatible BSB-LAN-Adapter V2  
    
The BSB-LAN adapter v2 can also be operated on an ESP32. In this way it is possible to benefit from the further development and the new functions of the BSB-LAN software from v2.x without having to purchase a new adapter. To do this, some changes must be made to the adapter itself, which are described below.  

| Attention |
|:----------|
| The steps described below to 'convert' the adapter to 3.3V are only valid for use on an ESP32 - on a Due the adapter v2 cannot be used due to the missing EEPROM! |
    
To successfully operate the adapter v2 on an ESP32, the adapter must be 'adjusted' to operate with 3.3V. This is already provided for use with a Raspberry Pi. The following steps need to be taken:  
- The adapter must be *completely* assembled. If the adapter is so far only equipped for use with the Arduino Mega 2560, the following components must be retrofitted:  
    - 1x resistor 47kΩ (→ R11)
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

## 1.4 Raspberry Pi

The adapter v3 could also be used in conjunction with a Raspberry Pi. Therefore you have to pay attention to some points: 

- **A usage of the BSB-LAN-Software is NOT possible (see notes below)!**  
- You only have to use double-rowed female headers which fit the RPi pins (instead of the pin headers for the usage with an Arduino Due!).
- With the complete length of the female headers (6 pins 'long', so 12 pins in summary) the first pair of the adapter must NOT be plugged to the first pair of the RPi pins (1/2), you have to start with the second pair of the RPi pins (3/4). 
In other words: make sure that the pin of the adapter labeled as TX1 will fit on the RPi pin 8 (= GPIO 14, UART0_TXD), the pin RX1 in the RPi pin 10 (= GPIO 15, UART0_RXD) and so on.  

  | Note |
  |:-----|
  | This counting refers to the official RPi pinout and the naming. | 
  
  The picture below shows the plain adapter *next to* the belonging RPi pins just to visualize the displacement/alignment on the longitudinal axis.  
  
  <img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/rpi_v3_ausrichtung.jpg">  
  
  *Exemplary alignment of the adapter along the longitudinal axis of the RPi pins.*  
  
- Before the usage of the software, the Pin 7 (GPIO 4) of the RPi must be  
a) defined as an output pin and    
b) set to "HIGH" within the OS of the RPi to achieve the power supply of the adapter.  
Therefore your have to execute two commands in the terminal (probably with a leading 'sudo'):   
`gpio -1 mode 7 output`  
`gpio -1 write 7 1`  
      
   
| **Attention** |
|:--------------|
| **For the usage of the adapter in conjunction with an RPi you have to use a complete different software: ["bsb_gateway"](https://github.com/loehnertj/bsbgateway) by J. Loehnert!** <br> For any support please contact the author of bsb_gateway! |  
| **This manual only refers to BSB-LAN!** <br> We can not and will not provide any support with regard to RPi use! |  
| From our side, the use of the adapter with the above mentioned software was only tested on an RPi 2. We are not able to judge whether it works properly with more recent RPi versions! |   
| For the usage of the adapter with an RPi at the PPS interface, the Python script [PPS-monitor](https://github.com/dspinellis/PPS-monitor) by D. Spinellis can be used. |  
  
     
---

## 1.5 Housing
The market offers just a small range of housings which are compatible for an Arduino Due or a ESP32-NodeMCU plus additional shields. If you search for them, you probably won't find anything. In the case of an Arduino Due, look out for housings which are designed for an Arduino Mega 2560, because it has the same form factor as the Due. Try to find a housing, which can accommodate the whole setup including the LAN shield though, because many housings are only designed to accommodate the plain Mega. This kind of housing has some cutouts in the top cover to plug in additional shields, but in that case the LAN shield and the adapter won't be protected at all.  
  
Besides commercial products and creative own built solutions, a 3D printer could be used to create a great housing.  
**The member "EPo" of the German FHEM forum was so kind to create and offer STL datafiles for a housing.**  
**Thanks a lot!**  
    
***The STL data files for Due, ESP32-NodeMCU and the Olimex ESP32-EVB housings including the BSB-LAN adapter are already included in the repository of BSB-LAN (subfolder "[schematics](https://github.com/fredlcore/BSB-LAN/tree/master/BSB_LAN/schematics)".  
    
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/BSB-Gehaeuse.jpg">  
  
*3D printer model of the housing for the Arduino Due, the LAN-Shield and the adapter v3.*  
  
   
    
---  
   
[Further on to chapter 2](chap02.md)      
[Back to TOC](toc.md)   
