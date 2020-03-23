[Back to TOC](toc.md)  
[Back to chapter 5](chap05.md)    
   
---  
# 6. Examining the Correct Functionality and First Usage of the Adapter   
To check if the adapter works correctly and recognizes your controller automatically, it's adviseable to follow these steps:  
   
1. Switch off the controller of the heater and connect the adapter at the right pins to the BSB (or LPB / PPS). Watch the polarity!  
2. Switch the controller back on and check if the red LED at the adapter is lit. If you see the LED flackering a little bit from time to time then that's no malfunction - it schows activity on the bus.  
3. Connect the Arduino (of course fully assembled with the lan shield and the adapter) via USB with your computer and via LAN with your network.  
4. Now start the Arduino IDE, choose the right COM port where the Arduino is connected to and start the serial monitor (menu "Tools" or the little magnifying glass symbol at the top right corner).  
5. If the connected controller has successfully been detected automatically by BSB-LAN it should appear an output in the serial monitor where the value/number behind "Device family" and "Device variant" is NOT 0.  
A correct output looks like that (with different numbers due to a different controller type):  
   
```
[...]
Device family: 96  
Device variant: 100  
[...]
```  
   
The following screenshot shows an output of the serial monitor right after the start (and a little runtime). The adapter is configured as room unit 2 (RGT2) and queries the parameters 6225 and 6226 initially for autodetection of the controller. The following lines already are telegrams. The display of the operating unit of the controller shows the temperature of the boiler unit (here: "Kesseltemperatur") which comes in periodically as a so called broadcast message (BC).  
  
  <img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/arduino-ide_serieller-monitor.png">      
  
*Note:*  
*If only weird character strings appear in the serial monitor, check the baud rate at the lower right corner of the serial monitor window. It should be set to 115200 baud.*  
   
If the connected controller hasn't been detected correct, the number behind "Device family" and "Device variant" will be a "0". Additionally to that six lines of "query failed" appear before the line "Device family".  
This is how it would look like:  
   
```  
[...]  
query failed  
query failed  
query failed  
query failed  
query failed  
query failed  
Device family: 0  
Device variant: 0  
[...]  
```  
   
In most cases there is a problem in the wiring or with certain components of the used harware or the adapter itself.  

   
---  
   
[Further on to chapter 7](chap07.md)      
[Back to TOC](toc.md)   

 
