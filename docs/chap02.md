[Back to TOC](toc.md)  
[Back to chapter 1](chap01.md)    
    
---  
   
# 2. General Informations about BSB, LPB and PPS   
BSB (Boiler System Bus), LPB (Local Process Bus) and PPS (point to point connection) are different types of bus systems (well, PPS isn't really a bus though). They aren't compatible between each other, so e.g. you can't connect a BSB unit to a LPB.  
Every of the controllers mentioned in this manual which are versions of RVS and LMS (and LMU7x) have at least one BSB port to offer. LPB isn't available at each type of these controllers, but for the usage of BSB-LAN it's not necessary - just use the BSB.  
PPS isn't used anymore at younger controllers, mostly old ones like RVA, RVP or LMU5x/6x are based on this type of connection system.  
   
In the following subchapters I'll give a short overview of the main aspects and differences of these bus/connection systems.  
   
---   
      
## 2.1 BSB and LPB   
BSB (Boiler System Bus) and LPB (Local Process Bus) are two different bus types, which can be divided into two different usage purposes:  
  
1. The BSB is a 'local' bus, where e.g. parts like the operating unit or a room unit are connected to the controller of the heating system. It offers 'local' access to the controller.  
   
2. The LPB is a bus, which offers access across connected controllers (if the installation was set up right!). Using the LPB you could e.g. connect two or more heating units to realize a burner cascade or to connect the controller of you heating system with the controller of your solarthermic system.  
If you have an existing installtion like that you could connect the BSB-LPB-LAN adapter to one of the mentioned controllers and would have access to certain parameters of both controllers. in that case you would have to pay attention to use the correct bus address of the units to make sure you reach the desired controller.  

Even though it's possible to use one adapter in an existing LPB structure with different controllers and query each controller by its own address, it's advisable to use one adapter-setup (Arduino + LAN shield + adapter) for each controller if they also offer a BSB port. It's just more comfortable because you wouldn't have to change the destination address every time you want to query another controller.  


   
---  
   
### 2.1.1 Addressing within the BSB   
Because of the bus structure, each participant gets a specific address. The following addresses are already defined:  
   
| bus address | device address | device (name in the serial monitor) |
|:-----------:|:--------------:|:------------------------:|
| 0x00 | 0 | controller itself („HEIZ“) | 
| 0x03 | 3 | expansion module 1 („EM1“) / mixer-ClipIn AGU | 
| 0x04 | 4 | expansion module 2 („EM2“) / mixer-ClipIn AGU | 
| 0x06 | 6 | room unit 1 („RGT1“) | 
| 0x07 | 7 | room unit 2 („RGT2“) | 
| 0x08 | 8 | OCI700 servicetool („CNTR“) |  
| 0x0A | 10 | operating unit (with display) („DISP“) | 
| 0x0B | 11 | service unit (QAA75 defined as service unit) („SRVC“) |  
| 0x31 | 49 | OZW672 webserver | 
| 0x32 | 50 | (presumably) wireless receiver („FE“) | 
| 0x36 | 54 | Remocon Net B („REMO") |  
| **0x42** | **66** | **BSB-LPB-LAN adapter („LAN“)** | 
| 0x7F | 127 | broadcast message („INF“) |  
  
*Note:*  
*The preset bus address `0x42` of the BSB-LPB-LAN adapter is the BSB device address 66. This address is set in the file `BSB_lan_config.h`.*  
   
---  
    
### 2.1.2 Addressing within the LPB   
The addressing within the LPB is different than the one within the BSB. Basically there are two 'addresses': an address of a segment and an address of a unit. Both have different meanings. Because the topic LPB is pretty complex, please search for further informations by yourself. Especially the documents about the LPB of "Siemens Building Technologies - Landis & Staefa Division" should be regarded as they are the main sources for these informations.  
   
*Note:*  
*The preset bus address `0x42` of the BSB-LPB-LAN adapter is the LPB segment address 4 with device address 3. This address is set in the file `BSB_lan_config.h`.*  
   
---  
   
## 2.2 PPS   
Right now, the PPS will just be mentioned really short here, because it's only available at *old* controllers and therefore not relevant for most of the users. As already said, PPS is not a real bus. It's more a point-to-point communication protocol for the usage of connecting a room unit to a controller for example. So if you have an old heating system like a Broetje WGB 2N.x and you have (or can connect) a room unit like a [QAA50 or QAA70](chap03.md#366-qaa50--qaa70), then you are using PPS.  
The adapter has to be connected the same way the room unit would have to be. Please read the manual of your heating system to find out about that. In most cases though the two pins of the connectors at the controller are labeled as "A6" and "MD" (or just "M"). In that case, you have to connect "A6" to "CL+"  and "MD"/"M" to "CL-" of the adapter.  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/RVA53_back.jpg">
    
*Connectors "A6" and "MD" at a Siemens RVA53 controller.*  
  
The functionality of this 'bus' is very limited, so you probably only have a dozen of parameters available. In the Webinterface of BSB-LAN you only have access to the category "PPS-Bus" (category 42).  

Note:  
If there's already a room unit like [QAA70](chap03.md#366-qaa50--qaa70) connected to the controller, BSB-LAN only can read values. If you want BSB-LAN to be able to set certain values, you would have to disconnect the room unit for the time you want to have the BSB-LAN adapter connected!  
Please take notice of the comments at the specific PPS definements in the file `BSB_lan_config.h` when using PPS!  
  
***Important note for users of the old (retired) setup adapter v2 + Arduino Mega 2560:***   
Because of the time-critical communication of the PPS, it is recommended to adjust the setup for using the hardware serial. Therefore the following adaptions have to be done:
- The adapter has to be *fully* assembled. 
- Only the solder jumer SJ1 has to be set. 
- The adapter has to be plugged in one pinrow towards the center of the the arduino. 
- The configuration has to be changed: set the pins within the variable "BSBbus" to 19 (RX) and 18 (TX) (instead of "68,69").  
      
---  
   
# 2.3 Connecting the Adapter to the Controller  
**Basically the connection of the BSB-LPB-LAN adapter to the controller is made in the same way and at the same port where a room unit will be connected. To localize the specific port at your controller, please read the manual of your heating system.**  
  
In cases where only one BSB port is available at the controller (e.g. RVS21 controller within heat pumps) you can connect the adapter parallel to an already installed room unit.  


*Note:*  
Because BSB is a real Bus, you can also connect the adapter in your living area if there's already a wired room unit installed.  
If you don't already have a wired room unit, you can still think about if it's maybe easier to put a long thin bus cable to the heater than a LAN cable.  
So it's not necessary at all to connect the adapter exactly at the place where the heater is located. 
   
*When connecting or disconnecting the adapter, please make sure that you switched off both units before (Arduino and controller of your heating system)!*  
*Please make sure you are using the right pins and regard the polarity!*  
   
---   
   
**Adapter:**  
The PCB of the adapter is already labeled with "CL+ / DB" and "CL- / MB".  
If you are building an adapter completely by your own, please look at the schematics.  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-adapter-v3-unbestueckt_anschluss.jpeg">
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-adapter-v3-bestueckt_anschluss.jpeg">  
  
---  
  
**BSB:**  
The connection of the adapter takes places at the already described ports and pins.  
Please connect 
"CL+" (adapter) to "CL+" (controller) and 
"CL-" (adapter) to "CL-" (controller).    
  
An additional pin "G+" which could be found sometimes at the controller is only for the backlight of a QAA75 room unit (because it offers 12V) - please make sure that you DON'T use that pin by accident!  
   
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

*BSB at connector "X86" at a RVS21 controller (Note: only certain pins!).* 
   
---  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-servicebuchse.jpg">
    
*BSB (CL+ & CL-) at the four pin service plug at the front of the operating unit ISR Plus. The (permament) usage of this connector isn't advisable though.*  

---   
   
***Notes on connectors:***  
   
The connection of the cables to the respective contacts should always be done with the specific connectors if available. A general list of the respective connectors can't be named here though, because some controllers need special connectors.  
For the most common three poled "FB" port (connector for the room unit) which is available at most of the controllers, this connector seem to fit though: [Broetje Connector Room Unit ISR, Rast 5- 3pol. = 627528](https://polo.broetje.de/mobile/mobile_view.php?type=1&pid=5316&w=1680&h=1050).  
   
***BSB / LPB / PPS:*** If the original connectors are not available, (insulated) 6,3mm cable lugs can be used instead.
   
***Four pin service plug:*** For the (temporary) connection at the four pin service plug at the front of the operating unit, 2,54mm DuPont connectors (female) can be used. You can find them (e.g.) at the typical breadboard connection cables or at many cables used within the internal parts of desktop computer hardware (e.g. internal speaker, fan).  
   
---
   
***Notes on cables:***   
   
***LPB:*** In order to be as protected as possible from interference, the connection cables for the *LPB* connection should have a cross-section of 1.5mm² in accordance with LPB design principles, twisted two-core and shielded (cable length 250m max per bus node, max total length 1000m).  
   
***BSB:*** For the *BSB* connection, Cu cables with a minimum cross-sectional area of 0.8mm² (up to 20m) should be selected, eg LIYY or LiYCY 2 x 0.8. For cable lengths up to 80m 1mm² should be selected, up to 120m 1,5mm² cross section2. In general, a parallel installation with mains cables should be avoided (interference signals); shielded cables should always be preferred to unshielded cables.  
   
Even though these are the official notes, users reported success with cables like phone installation cables, 0.5-0.75mm speaker cables and so on. Before you have to buy something new, you probably can just give it a try and see if you have some cables already at home which will do the job.  
   

---  
[Further on to chapter 3](chap03.md)  
[Back to TOC](toc.md)  




