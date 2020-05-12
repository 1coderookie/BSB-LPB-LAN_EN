[Back to TOC](toc.md)  
[Back to Introduction](index.md)    
    
---
# 1. The BSB-LPB-LAN Adapter and the BSB-LAN Software   
   
The BSB-LPB-LAN adapter and the BSB-LAN software were developed to realize remote access to the controllers of heating systems.  
Besides that it's possible to e.g. add additional temperature sensors (DHT22 & DS18B20) or relais boards or log parameters to an optional microSD card.  
You can also use additional individual code by using the file `BSB_lan_custom.h`.  
Of course BSB-LAN can be integrated in existing home automation solutions like FHEM, openHAB, nodeRed and so on by using the supported solutions MQTT, JSON and HTTPMOD.  
   
The software runs on an [Arduino Due](chap12.md#121-the-arduino-due) plus a [LAN shield](chap12.md#122-the-lan-shield) - that's (of course besides the adapter itself) already everything you need!  
Due to the limited space of flash memory you can't use boards like Arduino Uno or Nano or so.  

***For using the BSB-LAN system, the controller of your heating system has to be provided with a BSB (Boiler System Bus) or a LPB (Local Process Bus).***  
Most of the up to date controllers produced by SIEMENS which are used by manufacturers like Broetje or Elco offer at least one of these bus types.  
Older controllers which offer only a PPS (point to point connection) may work but mostly with (very) limited functionality.  
You can see an overview of the reported heating systems which are successfully used in combination with BSB-LAN [here](chap03.md#31-successfully-tested-heating-systems).  
***Please read the manual of your heating system to check if the controller offers this kind of connector(s).***  
  
You can find the schematic for the adapter in the [appendix A1](appendix_a1.md). If you don't want to build it by your own, you can contact Frederik Holst (bsb [at] code-it.de) and ask if a PCB is available.  

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-adapter-v3-unbestueckt-front.jpeg">

*The PCB of the BSB-LPB-LAN adapter v3, top view, not assembled.*  

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-adapter-v3-unbestueckt-back.jpeg">

*The PCB of the BSB-LPB-LAN adapter v3, bottom view, not assembled.*
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-adapter-komplett-due.jpeg">
    
*The PCB of the BSB-LPB-LAN adapter v3, fully assembled, mounted on an Arduino Due (Clone) plus LAN shield.*  
   

---  
   
[Further on to chapter 2](chap02.md)      
[Back to TOC](toc.md)   
