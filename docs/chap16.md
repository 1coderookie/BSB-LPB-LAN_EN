[Back to TOC](toc.md)  
[Back to chapter 15](chap15.md)    
   
---   
    
    
# 16. FAQ 

## 16.1 Can I Use the Adapter & Software with a Raspbery Pi?

Yes and no.  
The adapter itself can be used in conjuction with a Raspberry Pi, if you use certain parts. Please see the following chapters for further informations: [chap. 1.4](chap01.md#14-raspberry-pi) and [appendix a2.2](appendix_a2.md#a22-parts-list). 

**The BSB-LAN software can NOT be used with a RPi, it is only usable with the described Arduino.** Further informations are available in [chap. 1.4](chap01.md#14-raspberry-pi).  
    
---
    

## 16.2 Can I Connect One Adapter to Two Controllers at the Same Time?

No, this isn't possible. If you want to connect the hardware setup (arduino, ethernet-shield, adapter) to the BSB of the controllers, you have to use one hardware setup for each controller. If the contollers are already connected via LPB though, please see the following FAQ.  
    
---
    
## 16.3 Can I Connect an Adapter via LPB And Query Different Controllers?
Yes, if the existing controllers are already connected with each other via LPB. This LPB setup of the controllers already  has to work without any problems, so the setup has to be done correctly (e.g. device and segment addresses have to be set right).  
For querying data of each controller, the specific address has to be set within BSB-LAN. See chapter [5.1](chap05.md#51-url-commands) for further informations.  
    
---
    

## 16.4 Is a Multifunctional Input of the Controller Directly Switchable via Adapter?

No!

The multifunctional inputs of the controllers (e.g. H1, H2, H3 etc.) are not connectable directly to the adapter/arduino!  
  
These inputs must be switched potential free, so you have to use a relay in conjunction with the adapter/arduino. See [chapter 7.2](chap07.md#72-relays-and-relayboards) for further informations.  
    
---
    
## 16.5 Can an Additional Relayboard Be Connected And Controlled by the Ardunio?

Yes. See [chapter 7.2](chap07.md#72-relays-and-relayboards) for further informations.  
    
---
    
## 16.6 Can I Query the State of a Connected Relay?

Yes. See the specific URL command in chapter [5.1](chap05.md#51-url-commands).  
    
---
    

## 16.7 Can I Be Helpful to Add Yet Unknown Parameters?

Yes! Please see chapter [9](chap09.md) for further instructions.  
    
---
    

## 16.8 Why Do Some Parameters Appear Doubly Within a Complete Query?

When you do a complete query of all parameters via URL command (`http://<ip-address>/0-10000`) it can happen, that some parameters or program numbers are displayed doubly in the output. This happens because the same command id can occur within different parameters. Anyway, this doesn't have any negative influence the functionality in any way.   
    
---
    
## 16.9 Why Aren't Certain Parameters Displayed Sometimes?

First of all, not all of the parameters which are known by BSB-LAN are available within every type of controller. Certain parameters which are controller / heating system specific just aren't available. E.g.: specific paramaters of a gas-fired heating system aren't available within an oil-fired system.  
  
Besides that, if the controller has been powered on after the arduino already started, the automatic detection of the connected controller doesn't work. In this case just restart the arduino, so that the connected controller can be recognized by BSB-LAN.  

If still certain parameters don't appear which are available via the operational unit of the heating system, please do a query of /Q (see chapter [3.3](chap03.md#33-checking-for-non-released-controller-specific-command-ids)).  
If the desired parameters still don't appear there (reported as 'error 7' parameters), please follow the instructions in chapter [10](chap10.md).    
    
---
    
## 16.10 Why Isn't Access to Connected Sensors Possible?
If you connected DHT22 and/or DS18B20 sensors correctly to the adpater/arduino but the corresponding link in the webinterface doesn't work, you probably didn't adjust the belonging parameter in the file *BSB_LAN_config.h*.  
See the description in the file *BSB_LAN_config.h* and the chapter [7.1]chap07.md#71-usage-of-optional-sensors-dht22-ds18b20-bme280).  
    
---
    
## 16.11 I'm Using a W5500 LAN-Shield, What Do I Have to Do?

Make sure you are using the latest ethernet library within the Arduino IDE (min. version 2.0).   
    
---
    
## 16.12 Can States Or Values Be Sent As Push-Messages?

No, not by only using BSB-LAN. For this, you have to use additional software (e.g. FHEM) to query the desired parameters and process the data.   
    
---
   
## 16.13 Can (e.g.) FHEM 'Listen' to Certain Broadcasts?

Well, FHEM can 'listen' - but BSB-LAN can't send any messages by itself.   
    
---
    

## 16.14 Why Sometimes Timeout Problems Occur Within FHEM?

This could be due to the length of the send / receive process. You should calculate the timeout value in FHEM in the way, that you estimate approx. two seconds for a query and one second for a setting of a parameter.  

Besides that it could be helpful to put as many BSB-LAN specific readings as possible in one group for a query, so that collisions could be avoided.   
    
---
    
## 16.15 Is There a Module For FHEM?

Yes and no. The member *„justme1968"* develops a module:
[https://forum.fhem.de/index.php/topic,84381.0.html](https://forum.fhem.de/index.php/topic,84381.0.html)  
But: the development isn't done yet, so that an unproblematic and reliable usage can't be guaranteed at this time.  
    
---
    
## 16.16 Why Aren't Any Values Displayed At Burnerstage 2 Within /K49?

If you are using a gas-fired heating system, it's most likely modulating and doesn't even have a two-staged burning system. These systems mostly are only available within oil-fired heating systems. But even within these systems not all controllers spread the differentiation between stage one and two over the bus by broadcast messages, so that BSB-LAN doesn't recognize either it's stage one or two which is active.  
    
---
    

## 16.17 It Appears to Me That the Displayed Burner-Values of /K49 Aren't Correct.

This could be. The specific starts and runtimes are determined by the detection of the belonging broadcasts sent by the controller. Sometimes it can occur that a broadcast doesn't reach the other bus members, e.g. when a query is done at the same time.     
    
---
    
## 16.18 What Is the Exact Difference Between /M1 and /V1?

Please see the descriptions of the monitor mode (/M) and the verbose mode (/V) in chapter [5.1](chap05.md#51-url-commands). 
    
---
    
## 16.19 Can I Implement My Own Code In BSB-LAN?

Yes, for this you can and should use the designated file *BSB_LAN_custom.h*. Here you can add own code which will be initiated at each loop function.  
    
---
    
## 16.20 Can I Integrate MAX! Thermostats?

Yes, that's possible. You have to activate and adjust the specific definement in the file *BSB_LAN_config.h*.  
For further informations see the corresponding chapter [7.3](chap07.md#73-max-components).  
    
---
    
## 16.21 Why Isn't the Adapter Reachable After a Power Failure?

This behaviour was noticed sometimes, the reason is unclear. Just press the reset button of the arduino, after that everything should work fine again. If this happens more often at your home, you can probably add a little emergency power supply to prevent this. 
    
---
    
## 16.22 Why Isn't the Adapter Reachable Sometimes (Without a Power Failure)?

This problem only occured in rare cases, there is no clear solution for this behaviour. The only solution was a reset and reboot of the arduino.  
   
---
    
## 16.23 Why Do 'Query Failed' Messages Occur Sometimes?

If this occurs within a system which is usually working fine, then it could probably be due to hardware issues.  
    
---
    
## 16.24 I Don't Find a LPB or BSB Connector, Only L-BUS And R-BUS?!

In this case you have a controller which isn't compatible with BSB-LAN - please DON'T try to connect the adapter!  
You can see chapter [10.2.3](chap10.md#1023-note-incompatible-systems-from-broetje-and-elco) for further informations.  
    
---
    
## 16.25 Is There An Alternative Besides Using LAN?

Yes, please see chapter [1.2.2](chap01.md#122-due--wlan-the-esp8266-wifi-solution) and capter [7.5.2](chap07.md#752-wlan-usage-of-an-additional-router) if you are already using the Due setup. If you don't have any hardware yet, you could also think about using an [ESP32](chap01.md#13-esp32).    
  
---  
  
## 16.26 I Am Using The Outdated Setup Adapter v2 + Arduino Mega 2560 - Is There Anything I Have To Take Care Of?  
  
Yes! Please see [appendix D](appendix_d.md). 

  
---  

## 16.27 I Am Getting Error Messages from the Arduino IDE - What Can I Do?  
  
Error messages from the Arduino IDE can be various and have different reasons, so we can't go into this in detail here. In this case you should check all settings regarding port, board type etc. If Google does not provide any further information, you can also ask in the forum.  
As an example, here is a type of error message that occurs when the wrong board type is set when using an Arduino DUE:    
```  
BSB_lan:802:27: error: 'pgm_read_byte_far' was not declared in this scope

 uint8_t second_char = pgm_read_byte_far(enum_addr + page + 1);

                       ^~~~~~~~~~~~~~~~~
```  
---  
  
## 16.28 Connection to the WiFi Network can't be Established.  
  
In this case, the WLAN access data entered in the *BSB_LAN_config.h* file must always be checked. Also check the other network settings.  
If necessary, the router configuration must also be checked to see whether new WiFi devices are allowed to register.  
However, if the following two error messages occur when starting an ESP32-based microcontroller where you want to use WiFi
```
E (1593) esp.emac: emac_esp32_init(349): reset timeout 
E (1594) esp_eth: esp_eth_driver_install(214): init mac failed
```
then the `#define WIFI` definition in the *BSB_LAN_config.h* file was not activated. To activate it, the two slashes `//` in front of it must be removed and BSB-LAN must be flashed again (see also chapter [2.2.2](chap02.md#222-configuration-by-adjusting-the-settings-within-bsb_lan_configh)).  
    
---  
  
## 16.29 I Have Further Questions, Who Can I Contact?

The best option is to create an account at the german FHEM forum ([https://forum.fhem.de/](https://forum.fhem.de/)) and ask your questions in the specific BSB-LAN thread: [https://forum.fhem.de/index.php/topic,29762.0.html](https://forum.fhem.de/index.php/topic,29762.0.html). 
    
---  

[Further on to chapter 17](chap17.md)      
[Back to TOC](toc.md)   
