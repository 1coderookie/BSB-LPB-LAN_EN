[Back to TOC](toc.md)  
[Back to chapter 2](chap02.md)    
    
---  
   
# 3. BSB-LAN Setup: Connection and Startup

---
   
## 3.1 Connecting the Adapter  
  
**Basically the connection of the BSB-LPB-LAN adapter to the controller is made in the same way and at the same port where a room unit will be connected. To localize the specific port at your controller, please read the manual of your heating system.**  
  
In cases where only one BSB port is available at the controller (e.g. RVS21 controller within heat pumps) you can connect the adapter parallel to an already installed room unit.  


| Notes | 
|:------|
| Because BSB is a real bus, you can also connect the adapter in your living area if there's already a wired room unit installed. <br> If you don't already have a wired room unit, you can still think about if it's maybe easier to put a long thin bus cable to the heater than a LAN cable. <br> So it's not necessary at all to connect the adapter exactly at the place where the heater is located. |
| When connecting or disconnecting the adapter, please make sure that you switched off both units before (Arduino and controller of your heating system)! |
| Please make sure you are using the right pins and regard the polarity! A wrong connection might harm your system. |  
   
---   
   
**Adapter:**  
The PCB of the adapter is already labeled with "CL+ / DB" and "CL- / MB".  
If you are building an adapter completely by your own, please look at the schematics.  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-adapter-v3-unbestueckt_anschluss.jpeg">  
*The plain PCB.*  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-adapter-v3-bestueckt_anschluss.jpeg">  
  
*Fully assembled PCB.*    
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/HW-Setup.jpg">
    
*The complete setup (Arduino Due, LAN shield, BSB-LAN adapter), belonging cables included.*      
      
---  
  
**BSB:**  
The connection of the adapter takes places at the already described ports and pins.  
Please connect 
"CL+" (adapter) to "CL+" (controller) and 
"CL-" (adapter) to "CL-" (controller).    
  
An additional pin "G+" which could be found sometimes at the controller is only for the backlight of a QAA75 room unit (because it offers 12V constantly) - please make sure that you DON'T use that pin by accident!  
   
---   
   
**LPB:**  
The connection of the adapter takes places at the already described ports and pins.  
Please connect  
"DB (adapter)" to "DB (controller)" and  
"MB (adapter)" to "MB (controller)".     
   
---   
   
**PPS:**  
The connection of the adapter takes places at the already described ports and pins.  
In most of the cases it's "A6" and "M", therefore please connect  
"CL+" (adapter) to "A6" (controller) and  
"CL-" (adapter) to "M" (controller).  


---
  
**Connectors:**    
Both the BSB and LPB ports are double-pole and are labeled different sometimes by certain manufacturers. The most common names are:  
- BSB port: BSB, FB, BSB & M, CL+ & CL-  
- LPB port: LPB, DB & MB  
     
---  
  
**The following pictures show some examples of these connectors at different controllers:**    
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-lpb-anschluss.jpg">

*BSB (FB with CL+ & CL-) and LPB (DB & MB) at a Broetje ISR-RVS43.222 controller.*  
   
---  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-lpb-anschluss-2.jpg">
    
*Connectors b = BSB (CL+ & CL-) and a = LPB (DB & MB) at a Siemens RVS63.283 controller.*  
    
---
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/BSB-LMS.jpg">  

*BSB at connector "FB" at a LMS1x controller.*  
   
---   
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/BSB-X86-RVS21.jpg">      

*BSB at connector "X86" at a RVS21 controller.* 
   
---  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Baxi_Luna_BSB.png">      

*BSB on the "M2" connector block (behind the plastic cover on the left side of the picture) of a Baxi Luna Platinum.*    
*User "olympia" kindly wrote a manual about how to connect it for the Baxi Luna Platinum and made it available on [his GitHub account](https://github.com/olympia/BaxiPlatinum_BSB_LAN/blob/main/LunaPlatinum-BSBLAN.pdf). Many thanks for that!* 
   
---   
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-servicebuchse.jpg">
    
*BSB (CL+ & CL-) at the four pin service plug at the front of the operating unit ISR Plus. The (permament) usage of this connector isn't advisable though.*  
   
---     
   
| **Notes on connectors** |
|:------------------------|   
| The connection of the cables to the respective contacts should always be done with the specific connectors if available. A general list of the respective connectors can't be named here though, because some controllers need special connectors. <br> For the most common three poled "FB" port (connector for the room unit) which is available at most of the controllers, this connector seem to fit though: [Broetje Connector Room Unit ISR, Rast 5- 3pol. = 627528](https://polo.broetje.de/mobile/mobile_view.php?type=1&pid=5316&w=1680&h=1050). | 
| **BSB / LPB / PPS:** If the original connectors are not available, (insulated) 6,3mm cable lugs can be used instead. |
| **Four pin service plug:** For the (temporary) connection at the four pin service plug at the front of the operating unit, 2,54mm DuPont connectors (female) can be used. You can find them (e.g.) at the typical breadboard connection cables or at many cables used within the internal parts of desktop computer hardware (e.g. internal speaker, fan). | 
  
   
|**Notes on cables** |   
|:-------------------|   
| **LPB:** In order to be as protected as possible from interference, the connection cables for the *LPB* connection should have a cross-section of 1.5mm² in accordance with LPB design principles, twisted two-core and shielded (cable length 250m max per bus node, max total length 1000m). |  
| **BSB:** For the *BSB* connection, Cu cables with a minimum cross-sectional area of 0.8mm² (up to 20m) should be selected, eg LIYY or LiYCY 2 x 0.8. For cable lengths up to 80m 1mm² should be selected, up to 120m 1,5mm² cross section. |
| In general, a parallel installation with mains cables should be avoided (interference signals); shielded cables should always be preferred to unshielded cables. | 
| Even though these are the official notes, users reported success with cables like phone installation cables, 0.5-0.75mm speaker cables and so on. Before you have to buy something new, you probably can just give it a try and see if you have some cables already at home which will do the job. | 
   
---

## 3.2 Function Test and First Use

To check if the adapter works correctly and recognizes your controller automatically, it's adviseable to follow these steps:  
   
1. Switch off the controller of the heater and connect the adapter at the right pins to the BSB (or LPB / PPS). Watch the polarity!  

2. Switch the controller back on and check if the red LED at the adapter is lit. If you see the LED flackering a little bit from time to time then that's no malfunction - it schows activity on the bus.  

3. Connect the Arduino Due (of course fully assembled with the lan shield and the adapter) via USB (use the "Programming Port" in the center) with your computer and via LAN with your network.  

4. Now start the Arduino IDE, choose the right COM port where the Arduino is connected to and start the serial monitor (menu "Tools" or the little magnifying glass symbol at the top right corner).  

5. If the connected controller has successfully been detected automatically by BSB-LAN it should appear an output in the serial monitor where the value/number behind "Device family" and "Device variant" is NOT 0.  
   
   A correct output looks (e.g.) like that (with different numbers due to a different controller type):  
   
   ```
   [...]
   Device family: 96  
   Device variant: 100  
   [...]
   ```  
   
The following screenshot shows an output of the serial monitor after a successful start.  
The adapter is configured by deafult as "LAN" and queries the parameters 6225 and 6226 initially for autodetection of the controller.  
The following lines already are telegrams.  
The display of the operating unit of the controller shows the temperature of the boiler unit (here: "Boiler temp actual value") which comes in periodically as a so called broadcast message (BC).  
  
  <img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/SerMo_start_EN.png">      
  
| Note |
|:-----|
| If only weird character strings appear in the serial monitor, check the baud rate at the lower right corner of the serial monitor window. It should be set to 115200 baud. |  
   
  

**Check if BSB-LAN is accessable**  
As a first test if you can reach the BSB-LAN server, just enter the specific URL of your BSB-LAN setup (if you are using DHCP, the IP will be shown during startup within the SerMo). You should reach the start page of BSB-LAN:

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_home.png">  

Next, please proceed with the following chapter.   

---

## 3.3 Checking for Non-Released Controller Specific Command IDs
  
| Note |
|:--------|
| The procedure described below applies to controllers that are connected to the BSB-LAN setup via BSB or LPB. If you have connected a controller via PPS, the following is not necessary, because the function `/Q` is not available for PPS controllers. There only the following output appears: <br> 
`Scanne nach Geräten...` <br> `Complete dump:` <br> `Not supported by this device. No problem.` <br> `Fertig.` |   
| There are also restrictions with controllers that are connected via an LPB that has become available through retrofitting with OCI420 (i.e. LMU54/64 controllers). Here the corresponding device data should be listed at the beginning, but the "complete dump" is also not available. |   
    
  
If everything worked as expected until now, proceed with checking if all available parameters are enabled for the specific controller type (if successfully detected) - once this query finished successfully, your setup is ready to use. Click on the button "Check for new parameters" or execute the following URL command:  

`http://<ip-address>/Q`  
  
*Note: This query takes a while, so please wait until the whole 'complete dump' is done!*  
  
This function queries all command ids from the file *BSB_LAN_defs.h* and sends those ones which aren't marked for the own type of controller as a query to the controller (type QUR, 0x06).  
  
This already happens regularly within parameters for which only one command id is known and creates the already known "error 7" messages. As soon as more than one command id is known for a specific parameter, the first known command id still stays at "DEV_ALL" and is still the standard command id for all controllers. The new command id is then only approved for the new type of controller where that command id comes from. But: It's possible that this new command id also works with other controllers or that it's the 'regular' id. So the URL command /Q now checks all command ids which aren't approved for the own type of controller. By this it's often possible that 'new' parameters becoming available for the own controller.  

| Note |
|:-----|
| Within this command, only queries occur - so no changes of any settings within the controller will be changed! |  

If all command ids are already known and approved for the own type of controller, no "error 7" messages occur within the output of the /Q command.  
As an example, this is how the output of the webinterface looks in this case:
    
```
Version: 2.0.108-20211114123620
Scanne nach Geräten...
Geräteadresse gefunden: 0
Geräteadresse gefunden: 3
Teste Geräteadresse 0...
Gerätefamilie: 134
Gerätevariante: 146
Geräte-Identifikation: RVS43.345/146
Software-Version: 4.1
Entwicklungs-Index:  000002 - decoding error
Objektverzeichnis-Version: 301.1
Bootloader-Version:  (parameter not supported)
EEPROM-Version: ---
Konfiguration - Info 2 OEM:  (parameter not supported)
Parameterversion:  000001 - unknown type
Parametersatznummer:  000001 - unknown type
Kesseltypnummer OEM:  (parameter not supported)
Parametersatzgruppe OEM:  (parameter not supported)
Bisher unbekannte Geräteabfrage: 20
Parametersatznummer OEM:  (parameter not supported)
Info 3 OEM:  (parameter not supported)
Info 4 OEM:  (parameter not supported)
Bisher unbekannte Geräteabfrage:  04016301F4 - unknown type
Hersteller-ID (letzten vier Bytes): 34979
Außentemperatur (10003): 3.9 °C
Außentemperatur (10004): 3.9 °C
6225;6226;6224;6220;6221;6227;6229;6231;6232;6233;6234;6235;6223;6236;6258;6259;6343;6344;
134;146;RVS43.345/146;4.1;;301.1;---;;000001;000001;;;20;;;;04016301F4;34979;

Starte Test...

Test beendet.  

Complete dump:
DC 80 42 17 13 00 01 11 05 0A 8C 01 F5 11 03 08 8A 00 99 19 03 BD 9B
DC 80 42 15 13 00 02 01 05 05 B2 02 04 11 00 0D 4F 00 7A 2A D4
DC 80 42 17 13 00 03 11 06 0A 8C 02 09 11 03 08 8A 00 99 19 03 2F 51
DC 80 42 15 13 00 04 01 06 05 B2 02 18 11 00 0D 4F 00 7A B8 55
[...]
Fertig.
```
    
If some 'new' parameters have been identified by the function of /Q, the output of the webinterface looks (e.g.) like this (note the additional "error 7 (parameter not supported)" entries):
    
```
Version: 2.0.108-20211114123620
Scanne nach Geräten...
Geräteadresse gefunden: 0
Geräteadresse gefunden: 3
Teste Geräteadresse 0...
Gerätefamilie: 134
Gerätevariante: 146
Geräte-Identifikation: RVS43.345/146
Software-Version: 4.1
Entwicklungs-Index:  000002 - decoding error
Objektverzeichnis-Version: 301.1
Bootloader-Version:  (parameter not supported)
EEPROM-Version: ---
Konfiguration - Info 2 OEM:  (parameter not supported)
Parameterversion:  000001 - unknown type
Parametersatznummer:  000001 - unknown type
Kesseltypnummer OEM:  (parameter not supported)
Parametersatzgruppe OEM:  (parameter not supported)
Bisher unbekannte Geräteabfrage: 20
Parametersatznummer OEM:  (parameter not supported)
Info 3 OEM:  (parameter not supported)
Info 4 OEM:  (parameter not supported)
Bisher unbekannte Geräteabfrage:  04016301F4 - unknown type
Hersteller-ID (letzten vier Bytes): 34979
Außentemperatur (10003): 3.9 °C
Außentemperatur (10004): 3.9 °C
6225;6226;6224;6220;6221;6227;6229;6231;6232;6233;6234;6235;6223;6236;6258;6259;6343;6344;
134;146;RVS43.345/146;4.1;;301.1;---;;000001;000001;;;20;;;;04016301F4;34979;

Starte Test...

5450 - Trinkwasser Durchlauferhitzer - Schwelle zum Beenden einer BW-Zapfung bei DLH
0x313D10B5
DC C2 00 0B 06 3D 31 10 B5 9F A2
DC 80 42 0D 07 31 3D 10 B5 00 10 02 00

5451 - Trinkwasser Durchlauferhitzer - Schwelle für Bw-Zapfung bei DLH in Komfort
0x313D10B6
DC C2 00 0B 06 3D 31 10 B6 AF C1
DC 80 42 0D 07 31 3D 10 B6 00 C0 90 2D

5452 - Trinkwasser Durchlauferhitzer - Schwelle für Bw-Zapfung bei Dlh in Heizbetrieb
0x313D10B7
DC C2 00 0B 06 3D 31 10 B7 BF E0
DC 80 42 0D 07 31 3D 10 B7 00 C0 A7 1D

5455 - Trinkwasser Durchlauferhitzer - Sollwertkorrektur bei Auslaufregelung mit 40°C (°K)
0x313D10B8
DC C2 00 0B 06 3D 31 10 B8 4E 0F
DC 80 42 0D 07 31 3D 10 B8 00 00 52 60

5456 - Trinkwasser Durchlauferhitzer - Sollwertkorrektur bei Auslaufregelung mit 60°C (°K)
0x313D10B9
DC C2 00 0B 06 3D 31 10 B9 5E 2E
DC 80 42 0D 07 31 3D 10 B9 00 00 65 50 

Test beendet.

Fertig.  

Complete dump:
DC 80 42 17 13 00 01 11 05 0A 8C 01 F5 11 03 08 8A 00 99 19 03 BD 9B
DC 80 42 15 13 00 02 01 05 05 B2 02 04 11 00 0D 4F 00 7A 2A D4
DC 80 42 17 13 00 03 11 06 0A 8C 02 09 11 03 08 8A 00 99 19 03 2F 51
DC 80 42 15 13 00 04 01 06 05 B2 02 18 11 00 0D 4F 00 7A B8 55
[...]
Fertig.
```    
    
In general, the output of /Q (together with the brand and the specific name of that type of heating system) should be reported in any case, so that we can add that system to the list of reported systems which work with BSB-LAN.  
But: Especially if any error7-messages occur this should be done, so that the reported error7-parameters can be approved for that specific type of controller and can be made available.  
For reporting the system, please copy and paste the output of the webinterface (like the examples above) and post it either in the german [FHEM-Forum](http://forum.fhem.de/index.php/topic,29762.0.html) or send it via email to Frederik or me (Ulf). Please don't forget to add the name of the brand and the specific type of your heating system!  
Thanks.  


---

## 3.4 Debugging and Troubleshooting

If, contrary to expectations, problems occur and the BSB-LAN setup is not usable, this can have several causes.  

As a first step it is always a good idea to check the cabling and see if the red LED on the BSB-LAN adapter is lit.  

As a further step it is always useful to connect the microcontroller additionally to the PC and to start the Serial Monitor (SerMo) of the Arduino IDE. There you can check the startup process. If only cryptic strings appear in the output, check the set baud rate (bottom right). This should be set to 115200 baud.   
    
If the connected controller is *not* automatically recognized correctly, there is a "0" at "Device family" and "Device variant", additionally there are six lines "query failed" in front of "Device family".  
  
*Example:*  
```
[...]  
query failed  
query failed  
query failed  
query failed  
query failed  
query failed  
Device family: 0  
Device variant: 0  
[...]
```
Mostly the reason is then a problem of the hardware setup or the cabling, because the parameters 6225 and 6226 could not be retrieved successfully ([error message "query failed"](chap13.md#133-error-message-query-failed)").  

Further reasons for malfunctions are listed in chapters [13](chap13.md), [14](chap14.md) and [15](chap15.md).  

---  

[Further on to chapter 4](chap04.md)  
[Back to TOC](toc.md)  




