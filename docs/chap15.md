[Back to TOC](toc.md)  
[Back to chapter 14](chap14.md)    
   
---      
    
# 15. Problems and their Possible Causes
---
    
## 15.1 Arduino IDE Stops Compiling  
There are many possible errors which could be the reason that the Arduino IDE doesn't successfully compile and stops with an error, e.g. wrong board type/connection/speed chosen. However, there are three types of errors while trying to compile for an *ESP32* based board which should be mentioned here:  
- The error mentions something about "WiFiSPI"?  
→ If on ESP32, remove the `WiFiSPI` folder from the folder `src` - see step 5 in [chap. 2.1.2](chap02.md#212-installation-onto-the-esp32).
- The error mentions something about "ArduinoMDNS"?  
→ If on ESP32, remove the `ArduinoMDNS` folder from the folder `src` - see step 5 in [chap. 2.1.2](chap02.md#212-installation-onto-the-esp32).
- The error mentions something about "EEPROMClass"?  
→ Make sure you have the correct ESP32 framework installed (1.0.6 is too old) - see [chap. 12.1.2](chap12.md#1212-esp32).  
  
---
  
## 15.2 The Red LED of the Adapter Isn't Lit
- Controller is switched off
- Adapter isn't connected with the controller via BSB or LPB
- Adapter is incorrectly connected to the controller (CL+/CL- or DB/MB interchanged)
- Probably hardware fault of the adapter (defect component, eroor in the construction
- Probably loose contact at the bus connector (Rx/Tx or CL+/CL-)  
    
---
    
## 15.3 The Red LED Is Lit, but a Query Isn't Possible

- Probably adapter is connected wrong (usage of G+ instead of CL+)
- Probably loose contact at the bus connector (Rx/Tx or CL+/CL-)
- Probably wrong pin setting (Rx/Tx)
- Probably confused the transistors Q1/Q2
- Probably cold solder joints
- See subchapter [„No Query of Parameters Possible"](chap15.md#154-no-query-of-parameters-possible)  
    
---
    

## 15.4 Access to the Webinterface Isn't Possible

- Adapter doesn't have any/sufficient/unreliable power supply
- Adapter or LAN shield isn't connected to the LAN 
- IP and/or MAC address of the adapter isn't correct 
- Security functions [`passkey`](chap05.md), [`TRUSTED_IP`](chap05.md) and/or [`USER_PASS_B64`](chap05.md)
activated/deactivated → URL not adjusted, access from wrong IP etc.
- Check router and/or firewall settings 
- Access after power failure and/or restart of the microcontroller isn't possible → press reset button at the microcontroller
- Usage of a microSD card for logging → format as FAT32, execute URL command `/D0`, maybe try a different card and/or smaller capacity → see chapter [9.1](chap09.md#91-usage-of-the-adapter-as-a-standalone-logger-with-bsb-lan) 
- (Adapter,) LAN shield and/or Arduino/microcontroller is faulty (→ sometimes diffuse problems occured within the usage of cheap clones, maybe try other/original units)  

    
---
    

## 15.5 No Connection to WiFi Possible

Please check whether the definement `#define WIFI` is active, i.e. the trailing slashes have been removed. An indication that the definement is not active are these error messages in the serial monitior right after booting:  
```
E (1229) esp.emac: emac_esp32_init(349): reset timeout
E (1229) esp_eth: esp_eth_driver_install(214): init mac failed
```  
Furthermore, BSB-LAN can not connect to hidden WiFi networks. The only way to do this is by entering the BSSID of the WiFi network into the variable `bssid` in the configuration file `BSB_LAN_config.h`.  


---

## 15.6 No Query of Parameters Possible

- See subchapter [„The Red LED of the Adapter Isn't Lit"](kap15.md#151-the-red-led-of-the-adapter-isnt-lit)
- See subchapter [„The Red LED Is Lit, but a Query Isn't Possible"](kap15.md#152-the-red-led-is-lit-but-a-query-isnt-possible)
- See subchapter [„Access to the Webinterface Isn't Possible"](kap15.md#153-access-to-the-webinterface-isnt-possible)
- Rx and/or Tx assignment isn't correct, pinout and/or connection of the adapter doesn't fit to the settings in *BSB_lan_config.h* 
- Wrong bus type (BSB/LPB)  
    
---
    

## 15.7 Controller Isn't Recognized Correctly

- See subchapter [„The Red LED Is Lit, but a Query Isn't Possible"](kap15.md#152-the-red-led-is-lit-but-a-query-isnt-possible)
- See subchapter [„No Query of Parameters Possible"](chap15.md#154-no-query-of-parameters-possible)  
- Controller is switched off
- Controller was switched on after the microcontroller (automatic detection of the controller doesn't work in that case) → restart the microcontroller
- Controller is not or not in the right way connected with the adapter
- Device family and variant of the controller isn't known yet → check `http://<IP-Adresse>/6225/6226` and report the output  
    
---
    

## 15.8 Heating Circuit 1 Can't Be Controlled

- Adapter probably defined as room unit 2  
    
---
    

## 15.9 Room Temperature Can't Be Transmitted to Heating Circuit 1
- Adapter probably defined as room unit 2
- Possible access of the adapter is readonly → write access must be granted (webconfig `/C`: "write access" must be set to "standard" or "complete")  
    
---
    

## 15.10 Heating Circuit 2 Can't Be Controlled

- Adapter probably defined as room unit 1  
    
---
    

## 15.11 Room Temperature Can't Be Transmitted to Heating Circuit 2

- Adapter probably defined as room unit 1
- Possible access of the adapter is readonly → write access must be granted (webconfig `/C`: "write access" must be set to "standard" or "complete")  
    
---
    

## 15.12 Settings of the Controller Can't Be Changed via Adapter
- Possible access of the adapter is readonly → write access must be granted (webconfig `/C`: "write access" must be set to "standard" or "complete")  
    
---
    

## 15.13 Sometimes the Adapter Doesn't React to Queries or SET-Commands

- The microcontroller doesn't have multitasking capability - wait until a query is done (e.g. especially extensive queries of many parameters, whole categories or a big logfile may take quite a long time)  
    
---
    

## 15.14 'Nothing' Happens at the Query of the Logfile

- No microSD card is inserted in the slot
- Logging to microSD card was or is deactivated
- The logfile can get quite big, a query may take quite a long time  
- The graphical display (`http://<IP-Adresse>/DG`) of the logfile can't occur because of JavaScript-blockers within the browser  
    
---
    

## 15.15 No 24-Hour Averages Are Displayed

- The specific definement isn't activated
- No parameters for the calculation of the 24h-averages are set  
    
---
    

## 15.16 'Nothing' Happens at the Query of DS18B20/DHT22 Sensors

- There are no sensors connected
- The specific definements aren't activated
- The pinout isn't set correctly
- The sensors are faulty or defect  
    
---
    

## 15.17 The DS18B20 Sensors Are Showing Wrong Values

- Check power supply and whole installation (check size of the pullup-resistor,
use capacitors, check wiring, use correct topology etc.)  
    
---
    

## 15.18 The 'Serial Monitor' of the Arduino IDE Doesn't Provide Data

- Adapter isn't (additionally) connected via USB to your computer
- Wrong COM port or type of microcontroller board is chosen
- Wring baud rate is set → set to 115200 baud
- Adapter isn't connected to the controller and/or controller is switched off → see subchapters above  
        
---

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/U6U5NPB51)    

---  

[Further on to chapter 16](chap16.md)      
[Back to TOC](toc.md)   


