[Back to TOC](toc.md)  
[Back to chapter 6](chap06.md)    
   
---  
    
# 7. BSB-LAN Web - the Webinterface of the Adapter
By accessing the adapters IP (`http://<IP-address>`), the starting page of the webinterface "BSB-LAN Web" is displayed.  
If you're using the passkey function (`http://<IP-address>/<passkey>/`) or additional security options, of course the URL has to be specifically expanded.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_home.png">  
   
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

The button "Display log file" will be displayed in black letters, if the function isn't active due to an uncommented definement in the file *BSB_lan_config.h* (like shown in the screenshot above).  
   
Underneath the header area the installed version of BSB-LAN is shown.  
BSB-LAN checks by default if a newer version is available. If there is a newer version, the link leads to the ZIP file of the repo, so that you can save it directly from within the webinterface.  
*Note: If you don't want this function to be active because BSB-LAN connects automatically to the internet, you can deactivate it by uncommenting the belonging definement (`//#define VERSION_CHECK 1`) in the file BSB_lan_config.h.*

   
---  
   
**Heater functions (URL command: /K):**  
The button "heater functions" displays a list of all categories within the supported controllers (therefore also categories which aren't supported by certain controller types).  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_categories.png">  
   
A click on the category name queries all supported parameters and displays them in the webinterface.  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_category-c1.png">
    
---  
    
**Sensors (URL command: /T):**  
If optional sensors (DS18B20 / DHT22) are connected and configured in *BSB_lan_config.h*, the sensors will be listed after clicking this button.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_sensors.png">
    
DS18B20 sensors are named "1w_temp[x]" and are listed with their individual sensor ID.  
DHT22 sensors show the temperature, humidity and absolute humidity.  
   
---  
   
**Display log file (URL command: /D and /DG):**  
If the logging function to the microSD card is set and active, the logfile (file *datalog.txt*) will be graphically displayed. Therefore it's neccessary to allow the JavaScriptFramework from d3js.org to work, so please don't use adblockers on that, if you want to use this function.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_log_graph.jpg">   
      
---  
      
**Check for new parameters (URL command: /Q):**  
This function queries all known parameters and checks, if any parameter would be supported by that special controller which isn't released yet.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_Q_en.png">
   
---     
   
**Settings (URL command: /C):**  
It shows an overview of certain functions that have been set.  
You get a quick overview of (e.g.) the used version of BSB-LAN, the uptime, the used bus type, the address, the readonly or read/write state of the adapter, about parameters that are set to log, protected GPIO pins and so on.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_configuration.png">
   
---  
   
**URL commands:**  
The button leads to the chapter "Cheatsheet URL Commands" of this manual, where the URL commands are listed in a short overview. Internetaccess needed.  
   
---  
   
**Manual:**  
The button leads to the table of content of this manual. Internetaccess needed.   
   
---  
   
**FAQ:**  
The button leads to the chapter "FAQ" of this manual. Internetaccess needed.  
   

---  
   
[Further on to chapter 8](chap08.md)      
[Back to TOC](toc.md)   

    

