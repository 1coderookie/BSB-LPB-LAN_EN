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

"BSB-LPB-LAN" is a community based hardware and software project, which allows access to the controllers of different heating systems from certain manufacturers via PC / laptop / tablet / smartphone.  
  
The project consists of two specific components:  
- the [hardware](chap01.md), which basically is a logic level converter and which is called "BSB-LAN adapter" in the following and  
- the [BSB-LAN software](chap02.md), which has to be flashed onto a compatible microcontroller.    

The [BSB-LAN adapter](chap01.md#11-adapter) converts the 12V bus signals from the heating system to a suitable 3,3V signal for the necessary microcontroller.  
The adapter has to be connected to the [compatible controller](chap10.md) of the heating system and has to be used in conjunction with a compatible microcontroller ([Arduino Due](chap01.md#12-arduino-due) or [ESP32](chap01.md#13-esp32)).  
The microcontroller itself then will be integrated in your home network (either via LAN oder WiFi, depending on the chosen microcontroller).  
The controller of the heating system must be equipped with a ["Boiler System Bus" (BSB)](chap10.md#1011-bsb), a ["Local Process Bus" (LPB)](chap10.md#1012-lpb) or a ["Point-to-Point Interface" (PPS)](chap10.md#1013-pps). These are mostly heating systems in which a SIEMENS controller is used (or, depending on the heater manufacturer, usually a branded OEM version).

The [BSB-LAN software](chap02.md) then converts the logic levels to specific 'bus telegrams'. It basically gives access to the controller of the heating system. It offers various functions like monitoring values and the state of parameters, logging and (if wanted) controlling and changing settings via a [webinterface](chap04.md).  
An optional integration into an existing SmartHome system is also possible. An integration in systems such as [FHEM](chap08.md#81-fhem), [openHAB](chap08.md#82-openhab), [HomeMatic](chap08.md#83-homematic-eq3), [ioBroker](chap08.md#84-iobroker), [Loxone](chap08.md#85-loxone), [IP-Symcon](chap08.md#86-ip-symcon), [EDOMI](chap08.md#810-edomi), [Home Assistant](chap08.md#811-home-assistant), [SmartHomeNG](chap08.md#812-smarthomeng) or [Node-RED](chap08.md#813-node-red) can be done via [HTTPMOD](chap08.md#812-integration-via-httpmod-module), [JSON](chap05.md#53-json) or [MQTT](chap05.md#52-mqtt).  
In addition, the use of the adapter as a [standalone logger](chap06.md#61-logging-data) without LAN/WiFi or internet connection when using a microSD card is also possible.  
Furthermore, optional [temperature and humidity sensors](chap07.md#71-usage-of-optional-sensors-dht22-ds18b20-bme280) can be connected and their data can also be logged and evaluated.  
You also have the ability to integrate your own code into the BSB-LAN software, which offers a wide range of expansion options.  
    
---
    
As a first rough orientation, whether your own heating system is compatible or not, you can search for a connection option for optional room units in the operating instructions of your heater.  
If room units of the QAA55 / QAA75 type are listed as compatible (Broetje also refers to these as "RGB Basic" and "RGT B Top"), then the adapter can be connected via BSB. This is the case with most oil fired, gas fired and heat pump systems of the last years.  
If other room units are listed, see the chapter [Room Units](chap10.md#105-conventional-room-units-for-the-listed-controllers).  
However, accurate information if the adapter could be connected only provides the actual controller name and the manual of the controller.  
  
The following overview shows the most common used controllers of the different heating systems which will work with BSB-LAN.  
As a basic rule we can say, that the controller types of the last years which are named with an **S** at the end (RV**S** and LM**S**) are compatible with BSB-LAN and offer (mostly) the full range of funtionality.  
For further and more detailed informations about the different [controllers](chap10.md#102-detailed-description-of-the-supported-controllers) and the [connection](chap03.md#31-connecting-the-adapter) see the corresponding chapters.  
    
 **To see a detailed listing of the reported systems which are sucessfully used with BSB-LAN please follow the corresponding link:**  
- **[Broetje](chap11.md#111-broetje)**  
- **[Elco](chap11.md#112-elco)**  
- **[Other Manufacturers (e.g. Fujitsu, Atlantic, Weishaupt)](chap11.md#113-other-manufacturers)**    
    
**Gas-fired heating systems controllers:**  
- [LMU74/LMU75](chap10.md#10211-lmu-controllers) and [LMS14/LMS15](chap10.md#10212-lms-controllers), connection via BSB, complete functionality  
- [LMU54/LMU64](chap10.md#10211-lmu-controllers), connection via PPS, limited functionality  
   
**Oil-fired heating systems controllers / solarthermic controllers / zone controllers:**  
- [RVS43/RVS63/RVS46](chap10.md#10222-rvs-controllers), connection via BSB, full functionality  
- [RVA/RVP](chap10.md#10221-rva-and-rvp-controllers), connection via PPS (modelspecific sometimes LPB), limited functionality 
   
**Heat pump controllers:**  
- [RVS21/RVS61](chap10.md#10222-rvs-controllers), connection via BSB, full functionality  
   
**Weishaupt (model WTU):**  
- [RVS23](chap10.md#10222-rvs-controllers), connection via LPB, (nearly) full functionality  
  
---  
  
### The software is available [here](https://github.com/fredlcore/bsb_lan).

---  

### Authors:

-   Software, schematics v1, first documentation EN, support  
    up to v0.16:  
    *Gero Schumacher*

-   Software, PCB schematics v1 & v2, first documentation EN, support  
    since v0.17:  
    *Frederik Holst (bsb \[ät\] code-it.de)*

-   Debugging, manuals, translations, support  
    since v0.17:  
    *Ulf Dieckmann (adapter \[ät\] quantentunnel.de)*

*Based upon the code and the work of many other users and developers! Thanks!*  
          
---

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/U6U5NPB51)    

    
---
    
[Further to the TOC](toc.md)  


