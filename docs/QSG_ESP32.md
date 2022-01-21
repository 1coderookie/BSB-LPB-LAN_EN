[Back to Quick Start Guide for Arduino Due](QSG_DUE.md)  

   
---   
       
# Quick Start Guide for [ESP32 Boards](chap01.md#13-esp32)  
***The following brief instructions do not replace the reading of the detailed manual!
Please also read the respective more detailed explanations in the corresponding chapters.***
   
1. *Attention: Consider your ESP32 board type in the following instructions!*   

  - ***[Joy-It ESP32 NodeMCU](chap01.md#1311-esp32-nodemcu-joy-it)***:  
  Plug the NodeMCU on the BSB-LAN adapter and connect the NodeMCU with a USB cable to your computer. If your computer does not recognize the NodeMCU automatically, you have to install a driver for your operating system.   
  
      <img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/ESP32nodeMCU+Adapter.jpeg">
    
      *The complete setup (Joy-It ESP32 NodeMCU + BSB-LPB-LAN adapter).*      
  
  - ***[Olimex ESP32-EVB](chap01.mdl#1312-esp32-olimex-esp32-evb)***:  
  Plug the BSB-LAN adapter into the Olimex and connect the Olimex with a USB cable to your computer. If your computer does not automatically recognize the Olimex, install the appropriate driver for your operating system.  
  
      <img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/OlimexESP32EVB_v42_small.jpg">
    
      *The complete setup (Olimex ESP32-EVB + BSB-LPB-LAN adapter).*  
  
2. Download and install the latest version of the [ArduinoIDE](https://www.arduino.cc/en/Main/Software).  

3. Download the [current version of BSB-LAN](https://github.com/fredlcore/bsb_lan/archive/master.zip).  

4. Unzip the downloaded file "BSB_LAN-master.zip" and enter it.  

5. Enter the folder "BSB_LAN". There, rename the file "BSB_LAN_config.h.default" to "BSB_LAN_config.h".  
*Important:*  
*It is* ***mandatory*** *to take the following steps!*  
- Remove or move the two folders "ArduinoMDNS" and "WiFiSpi" from the BSB-LAN subfolder "src" - they must not be present in the "BSB-LAN" or "src" folder!  
- Open the file "BSB_LAN_config.h" and activate the definition '#define WIFI'.
- Enter the access data for your WiFi network at the entries `char wifi_ssid[32] = "YourWiFiNetwork";` and `char wifi_pass[64] = "YourWiFiPassword";`.  

6. Start the ArduinoIDE by double-clicking the file "BSB_LAN.ino" in the BSB_LAN folder.  
Check the correct serial port to which the ESP32 board is connected to the computer under "Tools/Port".  
*Now select the appropriate ESP32 board type under Tools/Board or Tools/Board:*  
- For the [Joy-It ESP32-NodeMCU](chap01.md#1311-esp32-nodemcu-joy-it) recommended in this manual (or identical clones with an "ESP32-WROOM" chip) the appropriate board type is "ESP32 Dev Module". Then select the variant "Default 4MB with spiffs (1.2BM APP/1.5MB SPIFFS)" for "Partition Scheme".  
- For the recommended [Olimex ESP32-EVB](chap01.md#1312-esp32-olimex-esp32-evb) please select the entry with the same name from the list. Then select the variant "Minimal SPIFFS (Large APPS with OTA)" for "Partition Scheme".  
- Set the transfer speed/baud rate to 115200.  
*If you encounter problems until here (e.g. that the board is not recognized), please read the detailed description in [chapter 2.1.2](chap02.md#212-installation-onto-the-esp32)!*    

7. *Important:*  
Adjust the settings in the file "BSB_LAN_config.h" according to your wishes and circumstances. This applies in particular to settings regarding the use of DHCP, a possibly different IP address, and the optional security functions.  
*Note:*  
In addition to the adjustment of the file "BSB_LAN_config.h" the adjustment of the configuration of BSB-LAN can also be done later via web interface.  
*Further hints as well as a description of all configuration options can be found in [chapter 2.2](chap02.md#22-configuration)!*  
When all settings are adjusted, start the flash process by clicking on "Sketch/Upload" or "Sketch/Upload".  
  
8. After finishing the flash process start the [Serial Monitor of the Arduino IDE](chap12.md#122-serial-monitor) and watch the output which is done when starting the ESP32. Among other things, the IP that is assigned to the setup when using DHCP is displayed there.  
After finishing the startup process you can disconnect the power supply of the ESP32, that means  removing the board from the USB port of your computer. This is not mandatory, but recommended for safety reasons.  
  
9. Switch off your heating system so that the controller is no longer power supplied. Now connect the adapter of the Arduino setup to the controller. To do this, connect the controller-side connections "CL +" and "CL-" (for BSB use) or "DB" and "MB" (for LPB use) to the identically named connections of the adapter. Pay attention to the correct connection: The connected connections must be *the same*, e.g. "CL +" to "CL +" and "CL-" to "CL-"!  
*For detailed instructions on how to connect a controller with PPS connections and illustrations of various controllers and the corresponding connections to be used, please refer to [chapter 3.1](chap03.md#31-connecting-the-adapter)!*  

10. Switch on the heating system / the controller.

11. Restart the setup by pressing the reset button or restore the power supply of the ESP32 board setup, ideally with a specific power supply with connection to the microUSB socket (NodeMCU) or hollow plug socket (Olimex). If you don't have a suitable power supply at hand (yet), the power can also be supplied via your USB port on the computer. The latter variant is advantageous in that you can use the [Serial Monitor of the Arduino IDE](chap12.md#122-serial-monitor) in parallel to control the startup behavior of the setup.  

12. Start an internet browser and go to the page of the BSB-LAN web interface. It can be found at the IP address you previously set in step 7 (the default is "192.168.178.88"). When using DHCP, the IP can be read out from the start sequence of the Arduino Due by using the [Serial Monitor of the Arduino IDE](chap12.md#122-serial-monitor). 

If everything is installed correctly, you will now have access to the controller of your heating system. If -contrary to expectations- errors or problems arise, then in addition to the chapters already mentioned, also read chapters [13](chap13.md), [14](chap14.md) and [15](chap15.md).  
  
Now please execute ["check for new parameters" (URL command /Q)](chap03.md#33-checking-for-non-released-controller-specific-command-ids) (if you are using a controller which is connected via PPS, this step can be skipped) and send us the output of the webinterface together with the name of the manufacturer and the name of your specific heating system.   

Have fun with BSB-LAN wish you Frederik and Ulf! :)  
      
---  

[Further on to the Table of Contents](toc.md)      
