[Back to TOC](toc.md)  
[Back to chapter 3](chap03.md)    
   
---  

# 4. Installation of the Arduino IDE and Configuration of the Adapter  
  
**Note: The following description is for the** ***Arduino Due!*** **If you want to install BSB-LAN on an** ***ESP32*** **, please see the [chapter 12.2](chap12.md#122-the-esp32)!**   
  
  
- Download and install the latest version of the Arduino IDE from [https://www.arduino.cc/en/Main/Software](https://www.arduino.cc/en/Main/Software) for your OS.  

- Connect the Arduino Due (plus installed LAN shield and BSB-LPB-LAN adapter!) via USB to your computer. Make sure that you are using the ["Programming Port" of the Due](chap12.md#121-the-arduino-due)! 

- Download the [latest version of BSB-LAN](https://github.com/fredlcore/bsb_lan/archive/master.zip) and extract the downloaded file *bsb_lan-master.zip*.  
  
- Enter the folder "bsb_lan-master"/"BSB_LAN" and rename the file *BSB_LAN_config.h.default* to ***BSB_LAN_config.h*** !  

- If you want to implement your own individual code, rename the file *BSB_LAN_custom.h.default* to ***BSB_LAN_custom.h*** !  

- Open the BSB_LAN sketch by double clicking the file *BSB_LAN.ino*. The necessary files like *BSB_LAN_config.h* and *BSB_LAN_defs.h* will automatically loaded within.  

- Switch to the tab "BSB_LAN_config.h" and configure the necessary parameters like IP address etc. corresponding to your network (if you don't want to use DHCP which is activated by default). Check if the IP you are typing in isn't already used by your router. You can also use DHCP though. Adjust the further settings of BSB-LAN in this file to your needs, e.g. logging, optional installed temperature sensors and so on. See [chap. 5.2](chap05.md#52-configuration-by-adjusting-the-settings-within-bsb_lan_configh) for further informations.   
  
- Make sure, that you are using the current Ethernet Library (min. v2). Therefore, open „Sketch“ → „Include Library“ → „Manage Libraries“ and check if an update or a newer version of the „Ethernet Library“ is available. If so, update to that version or install the newer one.  

- Now select "Arduino Due (Programming Port)" in "Tools/Board" in the main menu of the Arduino IDE.  
If the board doesn't appear in the list, you have to add the Atmel SAM Core to it. Simply choose Tools/Board/Boards Manager, search for 'Arduino SAM Boards' where the Due is included, click on it and then hit the 'Install' button. After doing that you will find the Arduino Due in Tools/Board.  

- Select the correct serial port in "Tools/Serial Port".  

- If you are using Windows, you probably have to install further drivers. Please see [https://www.arduino.cc/en/Guide/ArduinoDue](https://www.arduino.cc/en/Guide/ArduinoDue) for further informations.

- Upload/flash the sketch to your Arduino by selecting "Sketch/Upload".  

- Connect the Arduino with a LAN cable with your router/switch. Make sure that a you have a working power supply attached or that the Arduino is powered by USB (use the "Programming Port").    

- Open the page `http://<chosen-ip-address>/` (or `http://<chosen-ip-address>/<passkey>/` if you are using the optional passkey feature). Now the landing page of the BSB-LAN webinterface should appear. If not, reboot the Arduino by pressing the reset button on it and try again after a while.  
You can check your configuration of BSB-LAN by querying the page `http://<chosen-ip-address>/C`.  
   
*After you configured BSB-LAN by adjusting the file BSB_LAN_config.h to your needs and access to the webinterface of BSB-LAN was successful, you can now continue with checking the function of the adapter.*  
   
***Note: Once the adapter is connected to the bus of the controller of the heating system, you can let it be connected if you want to flash the Due again. There's no need to disconnect it if you want to update BSB-LAN.***     
   
---  
   
[Further on to chapter 5](chap05.md)      
[Back to TOC](toc.md)   
