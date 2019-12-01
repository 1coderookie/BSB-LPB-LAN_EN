[Back to TOC](toc.md)  
[Back to chapter 6](chap06.md)    
   
---  
    
# 7. BSB-LAN Web - the Webinterface of the Adapter
By accessing the adapters IP (`http://<IP-address>`), the starting page of the webinterface "BSB-LAN Web" is displayed.  
If you're using the passkey function (`http://<IP-address>/<passkey>/`) or additional security options, of course the URL has to be specifically expanded.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_home_EN.png">  
   
---  
   
Within the webinterface there are some buttons at the top for an easy and direct access to certain functions:  
- Heater functions  
- Sensors  
- Display log file  
- Check for new parameters  
- Settings  
- URL commands  
- Manual  
- FAQ  

The two buttons "Sensors" and "Display log file" are displayed in black letters, if the function isn't active due to an uncommented definement in the file *BSB_lan_config.h*. In the above shown screenshot it's the button "Display log file", because no parameters to log are set.  
   
---  
   
**Heater functions (URL command: /K):**  
The button "heater functions" displays a list of all categories within the supported controllers (therefore also categories which aren't supported by certain controller types).  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_heater-categories.png">  
   
A click on the category name queries all supported parameters and displays them in the webinterface.  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_kategorie-hk1.png">
    
---  
    
**Sensors (URL command: /T):**  
If optional sensors (DS18B20 / DHT22) are connected and configured in *BSB_lan_config.h*, the sensors will be listed after clicking this button.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_sensors.png">
    
DS18B20 sensors are named "1w_temp[x]" and are listed with their individual sensor ID.  
DHT22 sensors show the temperature, humidity and absolute humidity.  
   
---  
   
**Display log file (URL command: /D and /DG):**  
If the logging function to the microSD card is set and active, the logfile will be graphically displayed. Therefore it's neccessary to allow the JavaScriptFramework from d3js.org to work, so please don't use adblockers on that, if you want to use this function.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_log.jpg">   
      
---  
      
**Check for new parameters (URL command: /Q):**  
This function queries all known parameters and checks, if any parameter would be supported by that special controller which isn't released yet.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_Q.png">
   
---     
   
**Settings (URL command: /C):**  
It shows an overview of certain functions that have been set. You get a quick overview of the bus type, the address, the readonly or read/write state of the adapter, about parameters that are set to log, protected GPIO pins and so on.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_config-settings.png">
   
---  
   
**URL commands:**  
This button leads to the chapter "Cheatsheet URL Commands" in the manual, where the URL commands are listed in a short overview. Internetaccess needed.  
   
---  
   
**Manual:**  
This button leads to the table of content of the manual. Internetaccess needed.   
   
---  
   
**FAQ:**  
This button leads to the chapter "FAQ" in the manual. Internetaccess needed.  
   

---  
   
[Further on to chapter 8](chap08.md)      
[Back to TOC](toc.md)   

    

