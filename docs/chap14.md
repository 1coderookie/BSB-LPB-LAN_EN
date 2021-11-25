[Back to TOC](toc.md)  
[Back to chapter 13](chap13.md)    
   
---      
    
# 14. Problems and their Possible Causes
---
    

## 14.1 The Red LED of the Adapter Isn't Lit
- Controller is switched off
- Adapter isn't connected with the controller via BSB or LPB
- Adapter is incorrectly connected to the controller (CL+/CL- or DB/MB interchanged)
- Probably hardware fault of the adapter (defect component, eroor in the construction
- Probably loose contact at the bus connector (Rx/Tx or CL+/CL-)  
    
---
    
## 14.2 The Red LED Is Lit, but a Query Isn't Possible

- Probably adapter is connected wrong (usage of G+ instead of CL+)
- Probably loose contact at the bus connector (Rx/Tx or CL+/CL-)
- Probably wrong pin setting (Rx/Tx)
- Probably confused the transistors Q1/Q2
- Probably cold solder joints
- See subchapter [„No Query of Parameters Possible"](chap14.md#144-no-query-of-parameters-possible)  
    
---
    

## 14.3 Access to the Webinterface Isn't Possible

- Adapter doesn't have any/sufficient/unreliable power supply
(→ recommended power supply via external device, 9V has been tested reliable; power supply via USB could lead to problems) 
- Adapter or LAN shield isn't connected to the LAN 
- IP and/or MAC address of the adapter isn't correct 
- Security functions [`passkey`](chap05.md), [`TRUSTED_IP`](chap05.md) and/or [`USER_PASS_B64`](chap05.md)
activated/deactivated → URL not adjusted, access from wrong IP etc.
- Check router and/or firewall settings 
- Access after power failure and/or restart of the Arduino isn't possible → press reset button at the Arduino / LAN shield
- Usage of a microSD card for logging → format as FAT32, execute URL command `/D0`, maybe try a different card and/or smaller capacity → see chapter [9.1](chap09.md#91-usage-of-the-adapter-as-a-standalone-logger-with-bsb-lan) 
- (Adapter,) LAN shield and/or Arduino is faulty (→ sometimes diffuse problems occured within the usage of cheap clones, maybe try other/original units)  

    
---
    

## 14.4 No Query of Parameters Possible

- See subchapter [„The Red LED of the Adapter Isn't Lit"](kap14.md#141-the-red-led-of-the-adapter-isnt-lit)
- See subchapter [„The Red LED Is Lit, but a Query Isn't Possible"](kap14.md#142-the-red-led-is-lit-but-a-query-isnt-possible)
- See subchapter [„Access to the Webinterface Isn't Possible"](kap14.md#143-access-to-the-webinterface-isnt-possible)
- Rx and/or Tx assignment isn't correct, pinout and/or connection of the adapter doesn't fit to the settings in *BSB_lan_config.h* 
- Wrong bus type (BSB/LPB)  
    
---
    

## 14.5 Controller Isn't Recognized Correctly

- See subchapter [„The Red LED Is Lit, but a Query Isn't Possible"](kap14.md#142-the-red-led-is-lit-but-a-query-isnt-possible)
- See subchapter [„No Query of Parameters Possible"](chap14.md#144-no-query-of-parameters-possible)  
- Controller is switched off
- Controller was switched on after the Arduino (automatic detection of the controller doesn't work in that case) → restart the Arduino
- Controller is not or not in the right way connected with the adapter
- Device family and variant of the controller isn't known yet → check `http://<IP-Adresse>/6225/6226` and report the output  
    
---
    

## 14.6 Heating Circuit 1 Can't Be Controlled

- Adapter probably defined as room unit 2  
    
---
    

## 14.7 Room Temperature Can't Be Transmitted to Heating Circuit 1
- Adapter probably defined as room unit 2
- Possible access of the adapter is readonly → write access must be granted (webconfig `/C`: "write access" must be set to "standard" or "complete")  
    
---
    

## 14.8 Heating Circuit 2 Can't Be Controlled

- Adapter probably defined as room unit 1  
    
---
    

## 14.9 Room Temperature Can't Be Transmitted to Heating Circuit 2

- Adapter probably defined as room unit 1
- Possible access of the adapter is readonly → write access must be granted (webconfig `/C`: "write access" must be set to "standard" or "complete")  
    
---
    

## 14.10 Settings of the Controller Can't Be Changed via Adapter
- Possible access of the adapter is readonly → write access must be granted (webconfig `/C`: "write access" must be set to "standard" or "complete")  
    
---
    

## 14.11 Sometimes the Adapter Doesn't React to Queries or SET-Commands

- The Arduino doesn't have multitasking capability - wait until a query is done (e.g. especially extensive queries of many parameters, whole categories or a big logfile may take quite a long time)  
    
---
    

## 14.12 'Nothing' Happens at the Query of the Logfile

- No microSD card is inserted in the slot
- Logging to microSD card was or is deactivated
- The logfile can get quite big, a query may take quite a long time  
- The graphical display (`http://<IP-Adresse>/DG`) of the logfile can't occur because of JavaScript-blockers within the browser  
    
---
    

## 14.13 No 24-Hour Averages Are Displayed

- The specific definement isn't activated
- No parameters for the calculation of the 24h-averages are set  
    
---
    

## 14.14 'Nothing' Happens at the Query of DS18B20/DHT22 Sensors

- There are no sensors connected
- The specific definements aren't activated
- The pinout isn't set correctly
- The sensors are faulty or defect  
    
---
    

## 14.15 The DS18B20 Sensors Are Showing Wrong Values

- Check power supply and whole installation (check size of the pullup-resistor,
use capacitors, check wiring, use correct topology etc.)  
    
---
    

## 14.16 The 'Serial Monitor' of the Arduino IDE Doesn't Provide Data

- Adapter isn't (additionally) connected via USB to your computer
- Wrong COM port or type of Arduino board is chosen
- Wring baud rate is set → set to 115200 baud
- Adapter isn't connected to the controller and/or controller is switched off → see subchapters above  
    
---  

[Further on to chapter 15](chap15.md)      
[Back to TOC](toc.md)   


