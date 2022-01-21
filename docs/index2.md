## Introduction

This manual was written to make the start and the usage of the BSB-LAN hard- & software easier.  

***It is suggested to read the manual completely before starting the installation and usage of the adapter and the software.***    
    
---  
  
The copyright belongs to the author of this manual: Ulf Dieckmann.
  
---  
    
#### [Jump straight to the TOC](toc.md)      

#### [Download the PDF version of this manual](https://github.com/1coderookie/BSB-LPB-LAN_EN/raw/master/BSB-LPB-LAN-manual.pdf)     

#### Quick Start Guides for the installation and commissioning of the BSB-LAN setups are available here:
#### [Quick Start Guide for Arduino Due](QSG_DUE.md)
#### [Quick Start Guide for ESP32 Boards](QSG_ESP32.md)



---  
***WATCH OUT:  
There is NO WARRANTY or GUARANTEE of any kind that this adapter will NOT damage your heating system!  
Any implementation of the steps described here, any replica of the adapter and any use of the hardware and software described is at your own risk!  
None of the contributors or authors can be held liable for any damages of any kind!***   

---
  
### BSB-LPB-LAN - A Short Introduction

"BSB-LPB-LAN" is a community based hardware and software project, which originally had the goal, to access the controllers of different heat generators (oil and gas heating, heat pumps, solar thermal etc.) of certain manufacturers (initially mainly Brötje and Elco) via PC / laptop / tablet / smartphone.  
Later on it should be possible to read out data, process it further (eg log and graphically) or even influence the control system and integrate the system into existing SmartHome systems.
    
All this has now been implemented:
With the help of an inbuilt adapter, an Arduino Due and a LAN shield or an ESP32, a suitable heat generator can now be inexpensively integrated into the domestic network.
The controller of the heat generator must be equipped with a ["Boiler System Bus" (BSB)](chap10.md#1011-bsb), a ["Local Process Bus" (LPB)](chap10.md#1012-lpb) or a ["Point-to-Point Interface" (PPS)](chap10.md#1013-pps). These are systems in which a SIEMENS controller is used (or, depending on the heater manufacturer, usually a branded OEM version).

With the usage of the BSB-LPB-LAN adapter and the BSB-LAN software, various functions, values and parameters can now be easily monitored, logged and (if wanted) web-based controlled and changed.
An optional integration into existing SmartHome systems such as [FHEM](chap08.md#81-fhem), [openHAB](chap08.md#82-openhab), [HomeMatic](chap08.md#83-homematic-eq3), [ioBroker](chap08.md#84-iobroker), [Loxone](chap08.md#85-loxone), [IP-Symcon](chap08.md#86-ip-symcon), [EDOMI](chap08.md#810-edomi), [Home Assistant](chap08.md#811-home-assistant), [SmartHomeNG](chap08.md#812-smarthomeng) or [Node-RED](chap08.md#813-node-red) can be done via [HTTPMOD](chap08.md#812-integration-via-httpmod-module), [JSON](chap05.md#53-json) or [MQTT](chap05.md#52-mqtt).
In addition, the use of the adapter as a [standalone logger](chap06.md#61-logging-data) without LAN or Internet connection when using a microSD card is also possible.
Furthermore, optional [temperature and humidity sensors](chap07.md#71-usage-of-optional-sensors-dht22-ds18b20-bme280) can be connected and their data also logged and evaluated. By using an Arduino and the ability to integrate your own code into the BSB-LAN software, there is also a wide range of expansion options.

    
As a first rough orientation, whether your own heating system is compatible or not, you can search for a connection option for optional room units in the operating instructions of the heater. If room units of the QAA55 / QAA75 type are listed as compatible (Broetje also refers to these as "RGB Basic" and "RGT B Top"), then the adapter can be connected via BSB and the full functionality of BSB-LAN is given. This is the case with most oil, gas and heat pump systems of the last years.  
If other room units are listed, see the chapter [Room Units](chap10.md#105-conventional-room-units-for-the-listed-controllers).  
However, accurate information if the adapter could be connected only provides the actual controller name and the manual of the controller (search for "BSB" and "room unit").
   
---

The following overview shows the most common used controllers of the different heating systems which will work with BSB-LAN. As a basic rule we can say, that the controller types of the last years which are named with an **S** at the end (RV**S** and LM**S**) are compatible with BSB-LAN and offer (mostly) the full range of funtionality. For further and more detailed informations about the different [controllers](chap10.md#102-detailed-description-of-the-supported-controllers) and the [connection](chap03.md#31-connecting-the-adapter) see the corresponding chapters.  
   
**Gas-fired heating systems controllers:**  
- [LMU74/LMU75](chap10.md#10211-lmu-controllers) and [LMS14/LMS15](chap10.md#10212-lms-controllers) (latest models), connection via BSB, complete functionality  
- [LMU54/LMU64](chap10.md#10211-lmu-controllers), connection via PPS, limited functionality  
   
**Oil-fired heating systems controllers / solarthermic controllers / zone controllers:**  
- [RVS43/RVS63/RVS46](chap10.md#10222-rvs-controllers), connection via BSB, full functionality  
- [RVA/RVP](chap10.md#10221-rva-and-rvp-controllers), connection via PPS (modelspecific sometimes LPB), limited functionality 
   
**Heat pump controllers:**  
- [RVS21/RVS61](chap10.md#10222-rvs-controllers), connection via BSB, full functionality  
   
**Weishaupt (model WTU):**  
- [RVS23](chap10.md#10222-rvs-controllers), connection via LPB, (nearly) full functionality  
   
**To see a more detailed listing of the reported systems which are sucessfully used with BSB-LAN please follow the corresponding link:**  
- **[Broetje](chap11.md#111-broetje)**  
- **[Elco](chap11.md#112-elco)**  
- **[Other Manufacturers (e.g. Fujitsu, Atlantic, Weishaupt)](chap11.md#113-other-manufacturers)**  

  
### The software is available [here](https://github.com/fredlcore/bsb_lan).

---  

### Authors:

-   Software, schematics v1, first documentation EN, support  
    up to v0.16:  
    *Gero Schumacher (gero.schumacher \[ät\] gmail.com)*

-   Software, PCB schematics v1 & v2, first documentation EN, support  
    since v0.17:  
    *Frederik Holst (bsb \[ät\] code-it.de)*

-   Debugging, manuals, translations, support  
    since v0.17:  
    *Ulf Dieckmann (adapter \[ät\] quantentunnel.de)*

*Based upon the code and the work of many other users and developers! Thanks!*  
      
    
---
    
[Further to the TOC](toc.md)  


