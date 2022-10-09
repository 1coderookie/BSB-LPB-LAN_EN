[Back to TOC](toc.md)  
[Back to chapter 3](chap03.md)    
   
---  
    
# 4. BSB-LAN: The Webinterface

By accessing the adapters IP (`http://<IP-address>`), the starting page of the webinterface "BSB-LAN Web" is displayed.  
If you're using the passkey function (`http://<IP-address>/<passkey>/`) or additional security options, of course the URL has to be specifically expanded.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_home_new.png">  
   
---  
   
Within the webinterface there are some buttons at the top for an easy and direct access to certain functions:  
- Heater functions  
- Sensors  
- Display log file  
- Device-specific parameter list  
- Settings  
- URL commands  
- Manual  
- FAQ  

The button "Display log file" will be displayed in black letters, if the logging function isn't active (like shown in the screenshot above). If logging ist activated, the button is named "Plot log file".   
   
Underneath the header area the installed version of BSB-LAN is shown.  
BSB-LAN can check if a newer version is available. If there is a newer version, the link leads to the ZIP file of the repo, so that you can save it directly from within the webinterface.  

| Note |
|:-----|
| If you want to use this function, you need to activate it. Please see [chapter 2.2](chap02.md#22-configuration). |

   
---  
   
**Heater functions (URL command: /K):**  
The button "heater functions" displays a list of all categories within the supported controllers (therefore also categories which aren't supported by certain controller types):  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_categories.png">  
   
A click on the category name queries all supported parameters and displays them in the webinterface. Parameters which aren't supported/available within that specific type of controller will be displayed in grey letters and the note "(parameter not supported)":    
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_category-c1.png">
    
| Note |
|:-----|
| If you don't want these parameters to be shown, you can deactivate the output (see [chap. 2.2](chap02.md#22-configuration)). However, they will still be queried though if a whole category is queried. | 
    
---  
    
**Sensors (URL command: /K48):**  
If [optional sensors](chap07.md#71-usage-of-optional-sensors-dht22-ds18b20-bme280) are connected and configured correctly, the sensors will be listed after clicking this button.  
   
<!-- <img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_sensors.png"> -->
    
   
---  
   
**Display/Plot log file (URL command: /D and /DG):**  
If the [logging function to the microSD card](chap06.md#61-logging-data) is set and active, the belonging button is named "Plot log file". Once you click on it, the logfile (file *datalog.txt*) will be graphically displayed. If the logging function is deaktivated, the button is named "Display log file" and is shown in black letters.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_log_graph_en.png">   
  
Mouseover, click and mouse wheel actions within the graphical display provide various control options:
- better legibility for value numbers with plot lines close to each other (mouseover on plot)  
- user can interactively highlight plot lines for improved overview (mouseover on legend entries)  
- user can interactively disable plot lines for improved overview and vertical scaling (click on legend entries)  
- added zoom (mousewheel/pinch on plot) and pan capability (drag zoomed-in plot)  
  
| Note |
|:-------|
| To display the logfile graphically it's neccessary to allow the JavaScriptFramework from cdn.jsdelivr.net and d3js.org to work, so please don't use adblockers on that, if you want to use this function. |  
  
---  
      
**Device-specific parameter list (URL command: /Q):**  
After clicking on "Download" at the end of the displayed text, this function queries all parameters of the connected controller and creates the belonging textfile with the specific output. This file must be sent to Frederik (bsb(ät)code-it.de) in order to have the belonging file `BSB_LAN_custom_defs.h` created for your type of controller. After you received that file from Frederik, you the have to replace the existent file with this version and flash BSB-LAN again to gain full access to your controller. For further informations please see [chap. 3.3](chap03.md#33-create-device-specific-parameter-list).  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/QD_en.png">
   
---     
   
**Settings (URL command: /C):**  
It shows the [webinterface for configuration](chap02.md#221-configuration-via-webinterface) and an overview of certain functions that have been set.  
You get a quick overview of (e.g.) the used version of BSB-LAN, the uptime, the used bus type, the address, the readonly or read/write state of the adapter, about parameters that are set to log, protected GPIO pins and so on.  
   
<!-- <img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/webinterface_configuration.png"> -->
   
---  
   
**URL commands:**  
The button leads to the chapter [URL Commands](chap05.md#51-url-commands) of this manual, where the URL commands are listed in a short overview. Internetaccess needed.  
   
---  
   
**Manual:**  
The button leads to the [table of content](toc.md) of this manual. Internetaccess needed.   
   
---  
   
**FAQ:**  
The button leads to the chapter [FAQ](chap15.md) of this manual. Internetaccess needed.  
   

---  
   
[Further on to chapter 5](chap05.md)      
[Back to TOC](toc.md)   

    

