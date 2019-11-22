[Back to TOC](toc.md)  
[Back to chapter 15](chap15.md)    
   
---   
       
# 16. Quick Installation Guide
***The following brief instructions do not replace the reading of the detailed manual!
Please also read the respective more detailed explanations in the corresponding chapters.***
   
1. Download and install the latest version of the [ArduinoIDE](https://www.arduino.cc/en/Main/Software).  

2. Plug the LAN shield and adapter into the Arduino Mega 2560 and connect the Arduino setup to your computer with a USB cable.  

3. Download the current version of [BSB-LAN](https://github.com/fredlcore/bsb_lan/archive/master.zip).  

4. Unzip the downloaded file "BSB_lan-master.zip" and rename the folder to "BSB_lan".  

5. Rename the file "BSB_lan_config.h.default" to "BSB_lan_config.h".  

6. Start the ArduinoIDE by double-clicking the file "BSB_lan.ino" in the BSB_lan folder. The ArduinoIDE should recognize the connected Arduino Mega 2560 automatically together with the used COM port.  
*For steps 1-6, see the more detailed description in [chapter 4](chap04.md)!*  

7. Adjust the settings in the file "BSB_lan_config.h" according to your wishes and circumstances.  
*Note the [chapter 5](chap05.md)!*  
When all settings have been adjusted, flash the Arduino with the BSB-LAN software.  

8. After completing the flash process, remove the USB cable to de-energize the Arduino. Plug in the LAN cable and have the power adapter the Arduino ready.  

9. Switch off your heating system so that the controller is no longer power supplied. Now connect the adapter of the Arduino setup to the controller. To do this, connect the controller-side connections "CL +" and "CL-" (for BSB use) or "DB" and "MB" (for LPB use) to the identically named connections of the adapter. Pay attention to the correct connection: The connected connections must be *the same*, e.g. "CL +" to "CL +" and "CL-" to "CL-"!
*Also note the detailed description in [chapter 2.3](kap02.md#23-connecting-the-adapter-to-the-controller)!*  

10. Switch on the heating system / the controller.

11. Make the power supply to the Arduino setup, ideally with a specific power supply connected to the female connector socket. If you do not (yet) have a suitable power supply at hand, you can also power the Arduino setup via the USB port.

12. Start an internet browser and go to the page of the BSB-LAN web interface. It can be found at the IP address you previously set in step 6 (the default is "192.168.178.88").

If everything is installed correctly, you will now have access to the controller of your heating system. If -contrary to expectations- errors or problems arise, then in addition to the chapters already mentioned, also read chapters [13](chap13.md), [14](chap14.md) and [15](chap15.md).

Have fun with BSB-LAN wish you Frederik and Ulf! :)  
      
---  

[Further on to chapter 17](chap17.md)      
[Back to TOC](toc.md)   
