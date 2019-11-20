[Back to TOC](toc.md)  
[Back to Introduction](index.md)    
    
---
# 1. The BSB-LPB-LAN Adapter and the BSB-LAN Software   
   
The BSB-LPB-LAN adapter and the BSB-LAN software were developed to realize remote access to the controllers of heating systems.  
Besides that it's possible to e.g. add additional temperature sensors (DHT22 & DS18B20) or relais boards or log parameters to an optional microSD card.  
You can also use additional individual code by using the file `BSB_lan_custom.h`.  
Of course BSB-LAN can be integrated in existing home automation solutions like FHEM, openHAB, nodeRed and so on by using the supported solutions MQTT, JSON and HTTPMOD.  
   
The software runs on an [Arduino Mega 2560](chap12.md#121-the-arduino-mega-2560) plus [LAN shield](chap12.md#122-the-lan-shield). Due to the limited space of flash memory yuo can't use boards like Arduino Uno or Nano or so.  

***For using the BSB-LAN system, the controller of your heating system has to be provided with a BSB (Boiler System Bus) or a LPB (Local Process Bus).***  
Most of the up to date controllers produced by SIEMENS which are used by manufacturers like Broetje or Elco offer at least one of these bus types.  
Older controllers which offer only a PPS (point to point connection) may work but mostly with (very) limited functionality.  
You can see an overview of the reported heating systems which are successfully used in combination with BSB-LAN [here](chap03.md#31-successfully-tested-heating-systems).  
***Please read the manual of your heating system to check if the controller offers this kind of connector(s).***  
  
You can find the schematic for the adapter in the [appendix A1](appendix_a1.md). If you don't want to build it by your own, you can contact Frederik Holst (bsb [at] code-it.de) and ask if a PCB is available.  

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-platine-unbestueckt.jpeg">

*The PCB of the BSB-LPB-LAN adapter v2, not assembled.*  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-adapter-komplett-ardu.jpeg">
    
*The PCB of the BSB-LPB-LAN adapter, fully assembled, mounted on an Arduino Mega 2560 plus LAN shield.*  
   

*Note:*  
The adapter could also be used with a Raspberry Pi 2. Therefore you have to make sure you are using different pin headers, the additional circuits and parts (see [schematic](appendix_a1.md)). In that case you also have to use a different software than BSB-LAN: [bsb_gateway](https://github.com/loehnertj/bsbgateway) by J. Loehnert.  
**Here no support is given about bsb_gateway, this manual is only about BSB-LAN!**  

For those users who want to use the adapter with an RPi and an old controller with PPS, D. Spinelli wrote a Python script [PPS-monitor](https://github.com/dspinellis/PPS-monitor).  

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-adapter-komplett-rpi.jpeg">  
    
*The BSB-LPB-LAN adapter mounted to a Raspberry Pi 2.*  
   
***All informations in this manual are just about the Arduino version!***

---  
   
[Further on to chapter 2](chap02.md)      
[Back to TOC](toc.md)   
