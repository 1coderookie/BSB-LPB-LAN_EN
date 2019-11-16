## Preface

This manual was written to make the start and the usage of the BSB-LPB-LAN adapter (schematic layout v2, Arduino version) and the BSB-LAN software easier.  

***It is suggested to read the manual completely before starting the installation and usage of the adapter and the software.***    
    
---  
  
The copyright belongs to the author of this manual: Ulf Dieckmann.
  
---  
    
### Straight to the [TOC](toc.md)      
### [PDF version of this manual](https://github.com/1coderookie/BSB-LPB-LAN/raw/master/Handbuch_BSB-LPB-LAN-Adapter.pdf)     
### [Cheatsheet URL commands](https://github.com/1coderookie/BSB-LPB-LAN_EN/raw/master/Cheatsheet_URL-commands_EN.pdf)   


---  
2translate:
***ACHTUNG:  
Es gibt KEINE GARANTIE oder Gewährleistung jeglicher Art, dass dieser Adapter dein Heizungssystem NICHT beschädigt!  
Jegliche Umsetzung der hier beschriebenen Schritte, jeder Nachbau des Adapters sowie jede Verwendung der beschriebenen Hard- und Software erfolgt auf eigene Verantwortung und eigenes Risiko!  
Keiner der Mitwirkenden oder Autoren kann für etwaige Schäden jeglicher Art haftbar gemacht werden!***   

---
  
### BSB-LPB-LAN - a short introduction

"BSB-LPB-LAN" is a community based hardware and software project, which originally had the goal, to access the controller of different heat generators (oil and gas heating, heat pumps, solar thermal etc.) of certain manufacturers (initially mainly Brötje and Elco) via PC / laptop / tablet / smartphone.  
Later on it should be possible to read out data, process it further (eg log and graphically) or even influence the control system and integrate the system into existing SmartHome systems.
    
All this has now been implemented:
With the help of an inbuilt adapter, an Arduino Mega 2560 and a LAN shield, a suitable heat generator can now be inexpensively integrated into the domestic network.
The controller of the heat generator must be equipped with a "Boiler System Bus" (BSB), a "Local Process Bus" (LPB) or a point-to-point interface (PPS). These are systems in which a SIEMENS controller is used (or, depending on the heater manufacturer, usually a branded OEM version).

With the usage of the BSB-LPB-LAN adapter and the BSB-LAN software, various functions, values and parameters can now be easily monitored, logged and (if wanted) web-based controlled and changed.
An optional integration into existing SmartHome systems such as FHEM, openHab, HomeMatic or Loxone can be done via HTTPMOD, JSON or MQTT.
In addition, the use of the adapter as a standalone logger without LAN or Internet connection when using a microSD card is also possible.
In addition, temperature and humidity sensors can be connected and their data also logged and evaluated. By using an Arduino and the ability to integrate your own code into the BSB-LAN software, there is also a wide range of expansion options.

    
As a first rough orientation, whether your own heating system is compatible or not, you can search for a connection option for optional room units in the operating instructions of the heater. If room units of the QAA55 / QAA75 type are listed as compatible (Broetje also refers to these as "RGB Basic" and "RGT B Top"), then the adapter can be connected via BSB and the full functionality of BSB-LAN is given. This is the case with most oil, gas and heat pump systems of the last years.  
If other room units are listed, then see the chapter "[Room Units] (chap03.md#36-conventional-room-units-for-the-listed-controllers)" in the BSB-LPB-LAN manual.
However, accurate information if the adapter could be connected only provides the actual controller name and the manual of the controller (search for "BSB" and "room unit").
   
---

The following overview shows the most common used controllers of the different heating systems which will work with BSB-LAN. For further and more detailed informations about the different [controllers](chap03.md#32-detailed-listing-and-description-of-the-supported-controllers) and the [connection](chap02.md#23-connecting-the-adapter-to-the-controller) see the corresponding chapters in the [manual](https://1coderookie.github.io/BSB-LPB-LAN_EN).  
   
**Gas-fired heating systems controllers:**  
- [LMU74/LMU75](chap03.md#3211-lmu-controllers) and [LMS14/LMS15](chap03.md#3212-lms-controllers) (latest models), connection via BSB, complete functionality  
- [LMU54/LMU64](chap03.md#3211-lmu-controllers), connection via PPS, limited functionality  
   
**Oil-fired heating systems controllers / solarthermic controllers / zone controllers:**  
- [RVS43/RVS63/RVS46](chap03.md#3222-rvs-controllers), connection via BSB, full functionality  
- [RVA/RVP](chap03.md#3221-rva-and-rvp-controllers), connection via PPS (modelspecific sometimes LPB), limited functionality 
   
**Heat pump controllers:**  
- [RVS21/RVS61](chap03.md#3222-rvs-controllers), connection via BSB, full functionality  
   
**Weishaupt (model WTU):**  
- [RVS23](chap03.md#3222-rvs-controllers), connection via LPB, (nearly) full functionality  
   
**To see a more detailed listing of the reported systems which are sucessfully used with BSB-LAN please follow the corresponding link:**  
- **[Broetje](chap03.md#311-broetje)**  
- **[Elco](chap03.md#312-elco)**  
- **[Other Manufacturers (e.g. Fujitsu, Atlantic, Weishaupt)](chap03.md#313-other-manufacturers)**  

  
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


