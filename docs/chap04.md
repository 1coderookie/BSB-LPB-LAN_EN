#4. Installation of the Arduino IDE and configuration of the adapter#  
   
- Download and install the latest version of the Arduino IDE from https://www.arduino.cc/en/Main/Software for your OS.  
- Connect the Arduino Mega 2560 (+ LAN shield and BSB-LPB-LAN adapter!) via USB to your computer.  
- Download the latest version of BSB-LAN from https://github.com/fredlcore/bsb_lan and extract the downloaded file *bsb_lan_master.zip*.  
- Rename the now created folder *bsb_lan-master* to ***BSB_lan***!  
- Enter the folder and rename the file *BSB_lan_config.h* to ***BSB_lan_config.h***!  
- If you want to implement your own individual code, rename the file *BSB_lan_custom.h.default* to ***BSB_lan_custom.h***!  
- Open the BSB_lan sketch by double clicking the file *BSB_lan.ino*. The necessary files like *BSB_lan_config.h* and *BSB_lan_defs.h* will automatically loaded within.  
- Switch to the tab "*BSB_lan_config.h*" and configure the necessary parameters like IP address etc. corresponding to your network. Check if the IP you are typing in isn't already used by your router.  
- Now adjust the further settings of BSB-LAN in this file to your needs, e.g. logging, optional installed temperature sensors and so on. Please read the comments in the file behind the definements and parameters to gain further informations.  
- Now select "Arduino/Genuino Mega or Mega 2560" in "Tools/Board" in the main menu of the Arduino IDE.  
- Select "ATmega 2560" in "Tools/Processor".  
- Select "AVRISP mkII" in "Tools/Programmer".  
- Upload/flash the sketch to your Arduino by selecting "Sketch/Upload".  
- Connect the Arduino with a LAN cable with your router/switch. Make sure that a you have a working power supply attached or that the Arduino is powered by USB.    
- Open the page `http://<chosen-ip-address>/`(or `http://<chosen-ip-address>/<passkey>/`if you are using the optional passkey feature). Now the landing page of the BSB-LAN webinterface should appear. If not, reboot the Arduino by pressing the reset button on it and try again after a while.  
You can check your configuration of BSB-LAN by querying the page `http://<chosen-ip-address>/C`.  
   
*After you configured BSB-LAN by adjusting the file BSB_lan_config.h to your needs and access to the webinterface of BSB-LAN was successful, you can now continue with checking the function of the adapter.*  
   
--- 
