[Back to TOC](toc.md)  
[Back to chapter 9](chap09.md)    
    
---

# 10. Excursus: Heating Controllers and Accessories   
In general BSB-LAN works with controllers built by SIEMENS which are supported with a BSB and/or a LPB. These controllers are branded and used by different manufacturers of heating systems (e.g. Broetje, Elco). Please read the manual of your heating system to find out if the controller offers a BSB and/or LPB.  
   
*Clearification:*  
*Whenever I'm talking about the "controller", I mean the so called "BMU" (boiler management unit). That's the device with all the electronics inside, which controls the whole function of the heating system and which is located inside the housing of the heating system. At this device the sensors, pumps and the operating and room units are connected to.   
The 'operating unit' and the optional room units are the devices located outside at the housing of the heating system, the ones with a display and some buttons to interact with the BMU/controller.*  
   
***Note:***  
***Some recent models of Broetje don't have a BSB and are NOT compatible with BSB-LAN. Please see [chapter 10.2.3](chap10.md#1023-note-incompatible-systems-from-broetje-and-elco) for further informations.***  
   
---

## 10.1 Bus Systems of the Heating Controllers: BSB, LPB, PPS

BSB (Boiler System Bus), LPB (Local Process Bus) and PPS (point to point connection) are different types of bus systems (well, PPS isn't really a bus though). They aren't compatible between each other, so e.g. you can't connect a BSB unit to a LPB.  
Every of the controllers mentioned in this manual which are versions of RVS and LMS (and LMU7x) have at least one BSB port to offer. LPB isn't available at each type of these controllers, but for the usage of BSB-LAN it's not necessary - just use the BSB.  
PPS isn't used anymore at younger controllers, mostly old ones like RVA, RVP or LMU5x/6x are based on this type of connection system.  
   
In the following subchapters I'll give a short overview of the main aspects and differences of these bus/connection systems.  

BSB (Boiler System Bus) and LPB (Local Process Bus) are two different bus types, which can be divided into two different usage purposes:  
  
1. The BSB is a 'local' bus, where e.g. parts like the operating unit or a room unit are connected to the controller of the heating system. It offers 'local' access to the controller.  
   
2. The LPB is a bus, which offers access across connected controllers (if the installation was set up right!). Using the LPB you could e.g. connect two or more heating units to realize a burner cascade or to connect the controller of you heating system with the controller of your solarthermic system.  
If you have an existing installtion like that you could connect the BSB-LPB-LAN adapter to one of the mentioned controllers and would have access to certain parameters of both controllers. in that case you would have to pay attention to use the correct bus address of the units to make sure you reach the desired controller.  

Even though it's possible to use one adapter in an existing LPB structure with different controllers and query each controller by its own address, it's advisable to use one adapter-setup (Arduino + LAN shield + adapter) for each controller if they also offer a BSB port. It's just more comfortable because you wouldn't have to change the destination address every time you want to query another controller.  


---

### 10.1.1 BSB

The BSB (Boiler System Bus) is basically a 'local' bus. By 'local' in this case, I mean the specific controller that is used in the heat generator.  
  
The control panel, room units and expansion modules, for example, are connected to the BOD. These devices then have 'local' access to the controller. If the BSB-LAN setup is connected to the BSB, all bus devices of this controller can be accessed due to the unique addressing.  
  
The BSB is available at the following presented controllers of the TYps RVS, LMS1x as well as LMU7x. If your controller has a BSB *and* an LPB connection and no other controller via LPB (e.g. another heat generator in a cascade connection, a solar system controller or similar) and you only want access to this one controller, then the connection of the BSB-LAN setup to the BSB connection is recommended.  

**Addressing**
Because of the bus structure, each participant gets a specific address. The following addresses are already defined:  
   
| bus address | device address | device (name in the serial monitor) |
|:-----------:|:--------------:|:------------------------:|
| 0x00 | 0 | controller itself („HEIZ“) | 
| 0x03 | 3 | expansion module 1 („EM1“) / mixer-ClipIn AGU | 
| 0x04 | 4 | expansion module 2 („EM2“) / mixer-ClipIn AGU | 
| 0x06 | 6 | room unit 1 („RGT1: QAA55, QAA75, IDA“) | 
| 0x07 | 7 | room unit 2 („RGT2: QAA55, QAA75“) | 
| 0x08 | 8 | room unit 3/P and/or OCI700 servicetool („RGT3“) |  
| 0x0A | 10 | operating unit (with display) 1 („DSP1“) | 
| 0x0B | 11 | service unit (QAA75 defined as service unit) („SRVC“) |  
| 0x0C | 12 | operating unit (with display) 2 („DSP2“) | 
| 0x0D | 13 | operating unit (with display) 3 („DSP3“) | 
| 0x31 | 49 | OZW672 webserver | 
| 0x32 | 50 | (presumably) wireless receiver („FUNK“) | 
| 0x36 | 54 | Remocon Net B („REMO") |  
| **0x42** | **66** | **BSB-LPB-LAN adapter („LAN“)** | 
| 0x7F | 127 | broadcast message („INF“) |  
  
*Note:*  
*The preset bus address `0x42` of the BSB-LPB-LAN adapter is the BSB device address 66. This address is set in the file `BSB_LAN_config.h`.*  
   

---

### 10.1.2 LPB

The LPB (Local Process Bus) is an 'overlapping' bus for the use of several connected controllers in a communication-capable network.  
   
Via the LPB, several controllers can be connected to each other and (if the parameters are set correctly) they can share certain values or influence each other (like sending a telegram for starting the burner).  
In this way, for example, a cascade circuit of several burners can be realized or a gas or oil heating system can be 'connected' (in terms of control technology) with a solar system or a solid fuel boiler.  
The correct installation and connection of the individual components and the correct set up of the respective controllers should normally already be done by the installer of the heating system.    
  
A comprehensive query of values or parameters of two or more controllers in the LPB network via the BSB-LAN setup can be done by adding the specific address of the bus participant.  
   
The specific technical data, performance characteristics and requirements for corresponding installations and parameterizations with regard to the
device and segment addresses can be found in the respective technical documentation of the manufacturers.  
Since the installation, which can be quite complex, is usually already carried out by the respective installer during installation, no further special features are discussed here.  
For the interested user, two documents from "Siemens Building Technologies - Landis & Staefa Division" are recommended:  
- CE1N2030D Local Process Bus LPB System Basics  
- CE1N2032D Local Process Bus LPB project planning basics

**Addressing**  
The addressing within the LPB is different than the one within the BSB. Basically there are two 'addresses': an address of a segment and an address of a unit. Both have different meanings. Because the topic LPB is pretty complex, please search for further informations by yourself. Especially the documents about the LPB of "Siemens Building Technologies - Landis & Staefa Division" should be regarded as they are the main sources for these informations.  
   
*Note:*  
*The preset bus address `0x42` of the BSB-LPB-LAN adapter is the LPB segment address 4 with device address 3. This address is set in the file `BSB_LAN_config.h`. Usually it can be left to the default setting and doesn't need to be changed.*  
   
---

### 10.1.3 PPS

Right now, the PPS will just be mentioned really short here, because it's only available at *old* controllers and therefore not relevant for most of the users. As already said, PPS is not a real bus. It's more a point-to-point communication protocol for the usage of connecting a room unit to a controller for example. So if you have an old heating system like a Broetje WGB 2N.x and you have (or can connect) a room unit like a [QAA50 or QAA70](chap10.md#1056-qaa50--qaa70), then you are using PPS.  
The adapter has to be connected the same way the room unit would have to be. Please read the manual of your heating system to find out about that. In most cases though the two pins of the connectors at the controller are labeled as "A6" and "MD" (or just "M"). In that case, you have to connect "A6" to "CL+"  and "MD"/"M" to "CL-" of the adapter.  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/RVA53_back.jpg">
    
*Connectors "A6" and "MD" at a Siemens RVA53 controller.*  
  
The functionality of this 'bus' is very limited, so you probably only have a dozen of parameters available. In the webinterface of BSB-LAN you only have access to the category "PPS-Bus".  

Note:  
If there's already a room unit like [QAA70](chap10.md#1056-qaa50--qaa70) connected to the controller, BSB-LAN only can read values. If you want BSB-LAN to be able to set certain values, you would have to disconnect the room unit for the time you want to have the BSB-LAN adapter connected!  
Please take notice of the comments at the specific PPS definements in the file `BSB_LAN_config.h` when using PPS!  
  
***Important note for users of the old (retired) setup adapter v2 + Arduino Mega 2560:***   
Because of the time-critical communication of the PPS, it is recommended to adjust the setup for using the hardware serial. Therefore the following adaptions have to be done:
- The adapter has to be *fully* assembled. 
- Only the solder jumer SJ1 has to be set. 
- The adapter has to be plugged in one pinrow towards the center of the the arduino. 
- The configuration has to be changed: set the pins within the variable "BSBbus" to 19 (RX) and 18 (TX) (instead of "68,69").  
   
---  
       
## 10.2 Detailed Description of the Supported Controllers
   
The following list of controllers and descriptions should give a short
overview of a selection of devices already supported by BSB-LAN and their
rudimentary differences. The different controller-specific availability of special parameters will not
further received. It should be noted, however, that via 
BSB-LAN several parameters are available which aren't available by the regular operating unit of the heating system itself.  
    
---   
   
### 10.2.1 LMx Controllers
The following subchapters are about the LMU and LMS controller types. These seem to be used within gas fired heating systems.   
   
---   
   
#### 10.2.1.1 LMU Controllers   
Controllers of the series **LMU54/LMU64** are installed in older systems, they are out of date. According to experience, these controllers have neither a BSB nor a LPB, only a PPS interface is available here. Sometimes LPB can be retrofitted by means of a ClipIn module (OCI420).  
      
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/LMU64.jpg">  
   
*A LMU64 controller with an installed OCI420 ClipIn module.*  
    
     
Using BSB-LAN with these controller models is, according to experience, only possible to a limited extent. More detailed information can be found in [chapter 3.4](chap03.md#34-special-case-lmu54lmu64-controllers).  
    
---    
    
Controllers of the series **LMU74/LMU75** appear to be the successors of the LMU54/LMU64 controller series and are also no longer installed.   
      
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/LMU7.jpg">  
   
*A LMU7x controller.*  
      
The LMU7x controller type usually just offers BSB connection. If needed, LPB needs to be retrofitted using a ClipIn module (OCI420) (this is not necessary for using BSB-LAN though!).  
  
The control unit usually is a variant of the Siemens AVS37.294 (so called "ISR Plus" whithin Broetje).  
  
Usually NTC10k (QAD36, QAZ36) and NTC1k (QAC34 = outdoor temperature sensor) are used as sensors.    
  
---   
   
#### 10.2.1.2 LMS Controllers   
Controllers of the series **LMS** seem to be the successors of the LMU series and thus the current controller generation.   
      
The (functional) difference between the LMS14 and the LMS15
seems to be the "Sitherm Pro" application to optimize the overall
combustion process, which apparently only the LMS15 controller
seems to offer.  
   
The LMS controller type usually just offers a BSB connection. If needed, LPB can be retrofitted using a ClipIn module (OCI345) (this is not necessary for using BSB-LAN though!).  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/LMS15.jpeg">  
   
*A LMS15 controller.*  
        
The operating unit usually is a variant of the Siemens AVS37.294 (so called "ISR Plus" whithin Broetje).  
  
Usually NTC10k (QAD36, QAZ36) and NTC1k (QAC34 = outdoor temperature sensor) are used as sensors.    
    
---   
   
### 10.2.2 RVx Controllers   
The following subchapters are about the RVA, RVP and RVS (current one)  controller types. These seem to be used within oil fired heating systems, heat pumps and different 'standalone' systems (like solar or zone controllers).  
   
---   
   
#### 10.2.2.1 RVA and RVP Controllers  
Controllers of the type **RVA** seem to belong to the previous controller generation and, depending on the model, only offer a PPS (RVA53) or a PPS and a LPB connection (RVA63) but no BSB.  
As an (included) operating unit usually a variant of the so called "Eurocontrol" (Broetje) is installed.  
  
      
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/RVA53_back.jpg">  
   
*A RVA53 controller.*  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/RVA53_front.jpg">  
   
*Frontside view: Operating unit of a RVA53 controller.*  
   
Controllers of the type *RVP* seem to be even older than RVA controllers and only offer a PPS interface.  
 
   
---   
   
#### 10.2.2.2 RVS Controllers   
Controllers of the type **RVS** seem to be the current controller generation.  
They usually offer both a LPB and several BSB connections.   
Exceptions seem to be the controllers of the series RVS21, RVS41, RVS51, RVS61 and RVS23:  
- RVSx1 controllers are used within heat pumps, the RVS21 seems to offer only a BSB connector.  
- RVS23 controllers are used on a particular Weishaupt model (WTU) and seem to only offer a LPB. These controllers seem to be labeled by Weishaupt as "WRS-CPU Bx". Further information on this controller model can be found in [chapter 3.5] (chap03.md#35-special-case-weishaupt-heating-systems).  
   
The operating unit usually is a variant of the Siemens AVS37.294 (so called "ISR Plus" whithin Broetje).  
  
Usually NTC10k (QAD36, QAZ36) and NTC1k (QAC34 = outdoor temperature sensor) are used as sensors.  
   
The following gives a short overview of the main RVS controller types.  
  
---  
  
**RVS21.xxx**  
The RVS21 is the type of controller which is used in heatpumps. It offers BSB and a pair of connectors for an optional room unit.  

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/RVS21.jpeg">  
   
*A RVS21 controller.*  
   
If needed, LPB can be retrofitted using a ClipIn module (OCI345) (this is not necessary for using BSB-LAN though!).
   
---    
    
**RVS41.xxx**  
The RVS41 is another type of controller which is used within heat pumps. it offers BSB and LPB and seems to be pretty identical to the RVS43 (at least judging by the look of it).    
      
---   
   
**RVS43.xxx**  
The RVS43 is the type that usually is built in oil fired burner systems. The number of connectors and functions could be expanded with an AVS75 expansion module.  
      
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/RVS43.jpg">  
   
*A RVS43 controller.*  
   
---   
   
**RVS46.xxx**  
The RVS46 is a small zone controller, which offers one (ZR1) or two (ZR2) connections for a pump/heating circuit. The RVS46 can control zones/circuits by it's own, or integrated in the system via LPB connection to a main controller. It offers BSB and LPB.  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/RVS46_zr1.jpeg">
    
*The 'small' zone controller ZR1.*     
    
The ZR1/2 is not designed for controlling the whole functionality of e.g. a complete oil fired burner.   
   
---  
  
**RVS51.xxx**  
The RVS51 is the 'bigger' type of controller which is used in heatpumps. It offers BSB and LPB and seems to be pretty identical to the RVS63 (at least judging by the look of it).  
         
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/RVS51843.jpeg">  
   
*A RVS51.843 controller.*  
   
---  
  
**RVS61.xxx**  
The RVS61 is the 'bigger' type of controller which is used in heatpumps. It offers BSB and LPB and seems to be pretty identical to the RVS63 (at least judging by the look of it).  
   
---   
   
**RVS63.xxx**  
The RVS63 seems to be the 'biggest' controller with the most connectors and functions. Basically he is designed to control systems which are more complex, e.g. additionally solar thermic systems or an integrated oven. Therefore it is named "Solar System Controller" within Broetje.  
The RVS63 can already be built in within complex heating systems or it could optionally added. In that case it comes with an external housing and must be connected via LPB to the already existing controller. In that case, all the sensors, pumps etc. of the main system have to be connected to the RVS63, because it becomes the 'main' controller for the whole system.  
         
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/RVS63.jpg">  
   
*A RVS63 controller.*  
    
---
    
The **RVS65.xxx** seems to be pretty identical to the RVS63 and -until now- was reported only once by a user as being a wall-mounted "Solar System Controller" from Broetje.    
   

---
   
### 10.2.3 Note: Incompatible Systems from Broetje and Elco
   
It should be noted that the heating manufacturers introduced new device models to the market. According to current knowledge this type of controller is NOT compatible with BSB-LAN.  

Within Broetje these seem to be the heating system series 
- WLS / WLC (gas fired), 
- BOK (oil fired), 
- BLW Split-P, BLW Split C and BLW Split-K C (heat pump).  

Within Elco it seems to be the heating system series "Thision Mini".  

These systems seem to have 'IWR CAN'-based controllers built in (at the heater unit "IWR Alpha", room unit "IWR IDA"),
which have neither a LPB nor a BSB.  

The following image of a WLC24 board shows the existing connections.  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/regler-wlc24.jpg">
    
*Connectors of the new controller model at a Broetje WLC24 - this controller is incompatible with BSB-LAN!*     
    
In addition to a service socket (probably IWR CAN) there are not further documented 'L-Bus' and 'R-Bus'.  
At the 'R-bus' (room unit bus) either a room thermostat (on / off) or the new 'smart'
room unit "Broetje IDA" can be connected.   
   
***WATCH OUT:  
At none of these connectors the BSB-LPB-LAN adapter can be connected!***  

---   
   
### 10.2.4 Note: Special Case LMU54/LMU64 Controllers  
LMU54 / LMU64 controllers are based on OpenTherm, which has different bus specifications and also a different communication protocol. Therefore, OpenTherm is not compatible with BSB-LAN.  
However, often there is a possibility to connect this controller type anyway: as with the BSB controllers LMU7x and LMS1x, it is possible to retrofit a LPB by means of a so-called ClipIn module (OCI420). At this turn, the adapter can be connected.
  
      
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/LMU64.jpg">  
   
*A LMU64 controller with an installed OCI420 ClipIn module.*  
    
  
However, the functionality of this type of controller (even when using BSB-LAN) is relatively limited and also dependent to a certain extent on the software version of the controller (tested with LMU64, SW v2.08 vs. SW v3.0): controllers with SW from v3.0 seem to offer more functions (controllable via BSB-LAN) than controllers with SW <v3.0. In particular, the two setpoint temperature parameters 709 and 711 should be mentioned here. On their basis the burner behavior could be determined to a certain extent - these can only be used or changed with SW from v3.0. (Note: There is still an attempt if the burner behavior can be satisfactorily influenced by relays on another contact, but up to now we didn't find a solution for that.)  
   
However, according to current knowledge, parameters such as outside temperature, boiler temperature, DHW temperature, flow temperature, etc. can be accessed within both software versions mentioned.  
  
To be fair, it must be said here that the additional financial expense for purchasing an OCI420 LPB-ClipIn module may not be 'worthwhile'. However, this depends on the pursued goal. If you only want to log temperatures to get a rough overview of the actual state of the heating system, a more reasonable solution with a corresponding DS18B20 temperature sensor installation would be sufficient.  
  
Hints for connection and configuration of an OCI420-ClipIn are given in [chapter 10.2.6](chap10.md#1026-retrofitting-a-lpb-using-an-oci420-clipin).

---   
   
### 10.2.5 Note: Special Case Weishaupt Heating Systems   
Some Weishaupt devices (see list of successfully tested devices: Weishaupt WTU with WRS-CPU control unit) have RVS23 controllers installed. This controller type has a LPB on which the existing installation of Weishaupt systems is already based: room units, operating units and extension modules are already connected to each other via LPB.
The adapter can also be connected to this LPB, but it must be correctly integrated into the existing LPB installation. In general, this isn't a problem with the default LPB address of the adapter (segment 4, device address 3), but it should be checked again if there are any communication problems.  
   
The Weishaupt devices also seem to have a service socket in addition to the regular operating unit, with two of the four pins provided and led out. According to the statement of a Weishaupt user (*Thanks to BSB-LAN user Philippe!*), the upper one of the two pins seems to be MB and the lower one seems to be DB.  
     
---   

### 10.2.6 Note: Retrofitting an LPB by Using an OCI420 ClipIn   
If an OCI420 should be connected and used with a LMx controller, the installation and the connection must be made in accordance with the respective operating instructions.  
   
There are, however, a few key points that usually can't be found in the operating instructions although they are necessary for a successful operation. This mainly concerns the settings that have to be made for the LPB power supply. Furthermore, the LPB device address 1 with segment address 0 must be set and the setting as the time master has to be made.    
*As always, the following information comes without any guarantee!*  
   
If you follow the instructions on the OCI420, you will most likely encounter error 81, which means "short circuit in the LPB bus or missing power supply". If the OCI420 has been connected correctly, the LPB bus power must be activated in this case. The parameter is "LPBKonfig0".  
  
The following settings are described for controllers of type LMU64. Except for the parameter numbers, the settings of the bits are identical for other LMx controllers.  
For the LMU64, the relevant parameter has the number 604 (for LMU74: parameter number 6006). Here are eight bits (604.0 to 604.7) available to be set as follows (where "0" = OFF and "1" = ON):  
   
604.0 = 0 → time master  
604.1 = 1 → time master  
**604.2 = 1 → distributed bus supply AUTOMATIC**  
604.3 = 1 → status LPB bus supply: 1 = active  
604.4 = 1 → event behavior allowed  
604.5 = 0 → domestic hot water allocation  
604.6 = 0 → domestic hot water allocation  
604.7 = 0 → no priority of LMU request before external power specification  
    
If you call up the 'overview' of the LPBKonfig0 settings, however, the bit order is displayed from back to front (from bit 7 to bit 0!) and should be as follows after the successful setting: 00011110.  
Furthermore, the following settings have to be made:  
   
605 LPB device address = 1  
606 LPB segment address = 0  
   
After successful setting, no error code should occur and the green LED on the OCI420 should flash at regular intervals.  

---   
 
   
## 10.3 Expansion- and ClipIn-Modules    
If the available connectors and the range of function of the specific controller aren't enough (e.g. retrofitting of a solarthermic system), one can expand the system by using an expansion- or ClipIn-module. An expansion module offers connectors for (e.g.) a pump circuit and the belonging sensors.  
These modules are being connected at the main controller by using a special bus cable and the dedicated connector. Internally they are communicating with the controller via BSB (an exception seems to be the used controller type within the named Weishaupt heating systems). The parameterization takes places via the operating unit of the connected controller.  
Therefore, access to an extension module is only possible via the specific parameters within the main controller. Because the expansion modules are listed within a query of `ip/Q`, I'll present the two main types really short in the follwing.  
  
*Note:*  
*If you want to retrofit an expansion module, of course see the specific manual of your heating system for further informations and call a heating engineer for the installation.*    
  
---  
   
Expansion modules of the type **AVS75.xxx** are used within the RVS and LMS controller types. The bus connection usually takes places via the connector "Bus-EM".     
      
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/AVS75.jpg">  
   
*Expansion module AVS75.390.*  
   
---  
   
Expansion modules for LMU controller types are named "ClipIn-module". There seem to be different types for the specific needs (e.g. relay module, solar module). In general, the main appelation seems to be **AGU2.5x** (where the "x" seems to label the respective version), the bus connection usually takes place via the connector "X50".   
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/AGU255.JPG">  
   
*ClipIn-module AGU2.55.*  
   
---
   
## 10.4 Operating Units  
   
The operating unit (located at the heating system itself) within the systems of the recent years (with controller types LMU7x, LMS1x, RVS) usually are types of the **AVS37.xxx**. They look pretty much the same within the different manufacturers, within specific systems (e.g. heat pumps) certain buttons or functions can differ though.  
If you compare the look of the AVS37 operating unit and the QAA75.61x room unit, you can see that they actually also look pretty identical and the usage of both devices is also almost the same. In most cases the heater sided operating unit constantly shows the temperature of the heating device and the room unit shows the room temperature. Both units spread these values regularly (approx. every 10 seconds) over the BSB as a broadcast (INF-message).    
      
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/AVS37.jpg">  
   
*A typical operating unit AVS37.*  
   
---

The successor to the AVS37 model is the **AVS74.xxx**. It is equipped with a 3.8" LCD display and a rotary/push knob with which all settings are made. It is also used as a room unit under the designation [QAA74](chap10.md#1053-qaa74).  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/QAA74.jpg">  
  
*An AVS74 operating unit.*   
   
---  
  
Recently some manufacturers are using a new type of operating unit though, it's called **QAA75.91x**. It seems to be possible to detach these units from the heater itself and -by using an optional connection setup- to install them in your living area. In that case they are still working as the main operating unit for the controller, but with the additional benefits of a room unit.     
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/QAA75911_kessel.jpg">  
   
*A QAA75.91x operating unit.*  
   
| Note |
|:-----|
| It seems that changing the operating using the DHW/HC button (to the left of the rotary knob) for certain operating modes has the consequence that changing the operating mode via BSB-LAN and the 'regular' parameters 1600 (DHW) and 7xx (HC) is no longer possible if an operating mode other than "both" has been selected by using the mentioned key. The function of this new key is not (yet) implemented in BSB-LAN. If a change of the operating modes via BSB-LAN is desired, it must be ensured that the operating mode is set to "both" by using the mentioned key. |  
  
---   
   
In addition, there has recently been another model, the **AVS77.xxx**. This operation unit has so far only been reported to us from a user with a certain Baxi heating system (Baxi Luna Duo Tec MP). This unit offers (among other) buttons for changing the DHW and HC target temperature in small steps (degrees), but no longer has a rotary knob.  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/AVS77.png">  
  
*An AVS77.xxx operating unit.*  
   
| Note |
|:-----|
| It seems that changing the operating using the DHW/HC button (in the center of the operating unit) for certain operating modes has the consequence that changing the operating mode via BSB-LAN and the 'regular' parameters 1600 (DHW) and 7xx (HC) is no longer possible if an operating mode other than "both" has been selected by using the mentioned key. The function of this new key is not (yet) implemented in BSB-LAN. If a change of the operating modes via BSB-LAN is desired, it must be ensured that the operating mode is set to "both" by using the mentioned key. |    
   
---  
   
## 10.5 Conventional Room Units for the Listed Controllers   
The following briefly describes the different room units. These are also manufactured by SIEMENS and branded by the different heating manufacturers. Thus, they can be used across manufacturers, e.g. a corresponding QAA room unit of Elco can be used on a Broetje heater (of course, always provided that it is the right type of room unit). It's not yet known if there are certain restrictions in individual cases.  
   
As optional 'local' accessory parts of the heating system, they are connected to the BSB. That's why the connector for room units is what you are looking for, when you want to connect the adapter. So if you connect a room unit and adjusted the settings of the specific parameters (e.g. usage and influence of the room unit and room temperature), you can directly access the measured room temp. If you don't have an external room unit, but you can or want to measure your room temperature(s) in a different way, then you can imitate a room unit by transmitting these measured temperatures via BSB-LAN to the controller and influence the behaviour. For that, look up the function itself and the description of the URL command `/Ixxx=yyy`.   
   
The following description starts with the room units for the current heating system controllers (RVS and LMS), which are also fully supported by BSB-LAN (so called "Broetje ISR").

Note: It seems as if the product portfolio has been supplemented with new room units and other accessories. On occasion, I'll add relevant products here.  
   
---   
   
### 10.5.1 QAA55 / QAA58   
  
The QAA55 is the 'smallest' and most affordable ISR room unit model. At Broetje it is called "RGB B", sometimes it is also called "Basic Room Unit", "ISR RGB" or similar. It is quite limited in functionality and is basically 'just' a room temperature sensor with a few additional operating options. 
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/QAA55.jpg">  
   
*The QAA 55 room unit.*  
   
In addition to the optional measurement of the room temperature, it offers a presence button and the options for switching the operating mode and for changing the room set temperature. It only has a small LCD display that shows the current room temperature. It is connected via a two-pole cable to the BSB.
   
The QAA58 is the wireless version of the QAA55. It is battery operated, the AVS71.390 radio frequency receiver (868 MHz frequency) must in turn be connected to the X60 connection of the boiler controller via cable.
  
---   
   
### 10.5.2 QAA75 / QAA78   
The QAA75.61x is the 'big' ISR room unit. In addition to the integrated temperature sensor, it has the full functionality of the boiler-side control unit. In addition, there is a presence button and a manual DHW push can be triggered by pressing the DHW mode button for a longer time.   
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/QAA75.jpg">  
   
*The QAA75.61x room unit.*  
   
At Broetje the QAA75.61x is called "room unit RGT", sometimes it is also called "room unit RGT B Top", "ISR RGT" or similar. It is also connected by cable to the BSB, with a third connection for the optional backlight available (terminal "G +" on the controller).  
  
The QAA78.61x is the wireless version of the QAA75.61x. It is battery operated, the AVS71.390 radio frequency receiver (868 MHz frequency) must in turn be connected to the X60 connection of the boiler controller via cable. The above named "RGT" is extended by an "F" at Broetje, so it's "RGTF".
   
*Note:*  
At this point it has to be mentioned, that obviously two different versions of the QAA75 are available: the already mentioned room unit QAA75.61x and the different looking QAA75.91x.  
Whenever I'm referring to the "QAA75" in this manual, I mean the above described model QAA75.61x.  
   
The QAA75.91x seems to offer the same functionality like the QAA75.61x, but it seems to be used only with some types of heating systems by certain manufacturers (e.g. Broetje WMS/WMC C, BMK B, BMR B and Baxi Luna Platinum+). At these types of heating systems, it seems to be used as the operating unit which is located at the housing of the heating system itself, but (in conjunction with an optional adapter, e.g. Broetje "ISR RGA") could also be used as a room unit. In that case it seems to be still used as the operating unit, just with the additional benefit of the functions of a room unit.    
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/qaa75911.jpg">  
   
*A QAA75.91x operating unit, with optional equipment useable as a room unit.*  
   
---   
      
### 10.5.3 QAA74  
The QAA74 is a pretty new type of room unit at the market, which should/will replace the QAA75 in long term. At Broetje it's called "ISR RGP" (room unit premium), at Siemens "UI400". It is equipped with a 3,8" LCD display and a turn/push button for control purposes. Within some specific types of heating system, it's also used as the main operating unit, named AVS74.  

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/QAA74.jpg">  

   
---
### 10.5.4 Broetje IDA   
The "Broetje IDA" is a room unit which, in addition to an integrated temperature sensor and some functions, also offers a certain range of functions for controlling the heating system via app with a smartphone. A presence button is not available.  
   
IDA is integrated into the domestic WLAN and requires Internet access, if you want to control the unit via app. In the case of purely local use of the room unit (without remote access via the app), no WLAN access is required. Incidentally, the WLAN access also updates the IDA firmware.
An interesting analysis of the traffic was made [here](https://forum.fhem.de/index.php/topic.29762.msg833831.html#msg833831) by FHEM forum member "freetz".  
   
For connection to the BSB of the boiler controller, a BSB interface (GTW17) must be connected. Interested user must look for "ISR IDA" in this case, so that the GTW17 is included in the package.  
For controllers with the communication protocol OpenTherm (e.g. the older controller generation Broetje LMU6x), the OT interface (GTW16) must be used.  
IWR-CAN-based controllers (see [chapter 3.3](chap03.md#33-new-model--not-supported-controller-from-broetje)) can directly be connected to the service dongle GW05 (WLAN gateway).  
   
The exact functionality and installation steps of IDA are to be taken from the corresponding instructions of the manufacturer.  
   
The parallel use of IDA and BSB-LAN is possible in principle, however, due to the report of a user (*Thanks to FHEM-Foums member "mifh"!*) a few restrictions regarding the functional scope of BSB-LAN are known:  
If IDA is connected to the BSB, then it is the master for the settings or values of  
- time and date,  
- heating or switching programs and the  
- room temperature.  
If these settings / values are changed via BSB-LAN, they will be overwritten with the settings / values from IDA after a short time.  
It is thus no longer possible, for example, to detect the room temperatures from different rooms and to transmit them to the controller via BSB-LAN, since IDA overwrites this.  
   
The function of the presence button via BSB-LAN should still be available.  

---   
   
### 10.5.5 QAA53 / QAA73   
The room units QAA 53 and QAA 73 also differ in their functional scope. They are used in the OpenTherm-based LMU6x controllers.  
Further information on these room units can be found in the corresponding instructions.  
   
---   
   
### 10.5.6 QAA50 / QAA70   
In principle, the QAA50 and QAA70 also have the same difference in functionality. These room units are used in the old controller generations, which offers only one PPS connector. When using the adapter parallel to an already existing room unit it's only possible to read values via BSB-LAN. In that case no values and settings of the heating controller can be changed via BSB-LAN.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/QAA70.jpg">  
   
*A QAA70 room unit.*  
   
Further information on these room units can be found in the corresponding instructions.  
  
---
   
## 10.6 Special Accessories: Webserver OZW672 and Servicetool OCI700  
For the sake of completeness there should two commercial solutions be mentioned, which offer access to the controller of the heating system.  
Thats the webserver OZW672 and the servicetool OCI700.  
   
The webserver OZW672 (Broetje: "ISR OZW") is connected via bus cable to the controller and with a LAN cable to the network (and, if desired, also to the internet). If desired, one could connect with the fee-based dataportal of Broetje and offers remote access (via PC, tablet or smartphone+app) to the controller.  
   
The OCI700 is the servicetool used by the installation engineer. It is connected to a local computer running a special software and offers an overview of the settings of the controller.  
   
---
   
[Further on to chapter 11](chap11.md)      
[Back to TOC](toc.md)   
   
 
