[Back to TOC](toc.md)  
[Back to chapter 7](chap07.md)    
   
---      

# 8. URL Commands and Special Functions
Because the webinterface basically is just set 'on top' to achieve access without further programs like FHEM or openHAB, it's possible to access the functions and parameters with external programs.  

*A short and printer friendly listing of the available URL commands is availabe [here](https://github.com/1coderookie/BSB-LPB-LAN_EN/raw/master/commandref/Cheatsheet_URL-commands_EN.pdf).*  
    
---
    
## 8.1 Listing and Description of the URL Commands
*Note:*  
The values and parameters in the following list of the URL commands must be written without the brackets. E.g.: URL command `/<x>` for the simple query of parameter 8700 = `/8700`.  
   
   

| URL Command           | Effect                                                                    |
|:----------------------|:------------------------------------------------------------------------------|
|  **/\<x\>**               | **Query value/setting of parameter \<x\>**  
|  **/\<x\>/<y\>/<z\>**     | **Query values/settings of parameters \<x\>, \<y\> and \<z\>**   
|  **/\<x\>-\<y\>**         | **Query values/settings of parameters \<x\> to \<y\>**  
|  **/A**                   | **Query 24h average values** <br /> Shows the 24h average values of the parameters, which were defined before within the variable `avg_parameters` in the file *BSB\_lan\_config.h*.  
|  **/A=0**                   | **Disable 24h average calculation temporarily** <br /> Disables the 24h average calculation temporarily (until the next reboot of the Arduino). For a complete deactivation, uncomment all parameters for that function in the file *BSB\_lan\_config.h*.  
|  **/A=\<x\>,\<y\>,\<z\>**       | **Change 24h average value calculation of parameters \<x\>, \<y\>, \<z\>** <br /> During runtime up to 20 new parameters can be defined for the 24h average calculation. These parameters are kept until the next reboot of the Arduino.  
|  **/B**                   | **Query accumulated burner-runtimes (in seconds) and -cycles (including DHW)** <br /> These values are calculated by the incoming broadcasts from the controller. Due to maybe missed broadcast, these values should be considered as orientational values. <br /> Certain controllers of oil fired burners support a two stage counter, most of the controllers just show values for stage one.  
|  **/B0**                  | **Reset counter of burner-runtime and -cycles**  
|  **/C**                   | **Display configuration of BSB-LAN**  
|  **/D**                   | **Display logfile from the microSD-card** <br /> Displays the logfile *datalog.txt* which contains the values of the logged parameters defined in the file *BSB_lan_config.h*.
|  **/DG**                  | **Graphical display of the logfile from microSD-card** <br /> Shows graphical output (graphs) of the logged values. <br /> *Note:* If you use Javascript blockers, make sure you allow access to d3js.org, because the Arduino just loads the csv-file into the browser and the D3-framework converts the data.     
|  **/D0**                  | **Reset logfile & create new header** <br /> This command deletes the content of the file *datalog.txt* and creates a new csv-header. This command should be executed before first logging.     
|  **/E\<x\>**              | **Display ENUM-values of parameter \<x\>** <br /> At this command the adapter doesn't communicate with the controller, it's a software sided internal function of BSB-LAN. This command is only available for parameters of the type VT\_ENUM.  
|  **/G\<x\>**              | **GPIO: Query pin \<x\>** <br /> Displays the actual state of GPIO pin \<x\>, where \<y\>=0 is *LOW* and \<y\>=1 is *HIGH*.  
|  **/G\<x\>,\<y\>**        | **GPIO: Set pin \<x\> to high (\<y\> = 1) or low (\<y\> = 0)** <br /> Sets GPIO pin \<x\> to *LOW* (\<y\>=0) or *HIGH* (\<y\>=1). <br /> Reserved pins which shouldn't be allowed to be set can be defined previously at `GPIO_exclude` in the file *BSB\_lan\_config.h*. 
|  **/G\<x\>,I**            | **GPIO: Query pin \<x\> while setting to INPUT** <br /> If e.g. a coupling relay is connected to a GPIO pin and the state should just be queried, this command should be used. It sets the GPIO pin to input (default they are set to output) and keeps this as long until it's changed by using `/G<x>=<y>`. After that, it's set to output again until the next "I" sets it to input again.  
|  **/I\<x\>=\<y\>**        | **Send INF-message to parameter \<x\> with value \<y\>** <br /> Some values can't be set directly, the controller gets these values by a TYPE_INF-message. As an example, the room temperature of 19.5°C should be transmitted: `http://<ip-address>/I10000=19.5`.  
|  **/JK=\<x\>**         	| **JSON: Query all parameters of category \<x\>**  
|  **/JK=ALL**          	   | **JSON: List all categories with corresponding parameter numbers**  
|  **/JQ=\<x\>,\<y\>,\<z\>**      | **JSON: Query parameters \<x\>, \<y\> and \<z\>**  
|  **/JQ**                  | ***→ with JSON-structure (see [chapter 8.2.4](chap08.md#824-retrieving-and-controlling-via-json)) via HTTP-POST request:* Query parameters**
|  **/JS**                  | ***→ with JSON-structure (see [chapter 8.2.4](chap08.md#824-retrieving-and-controlling-via-json)) via HTTP-POST request:* Set parameters**
|  **/K**                   | **List all categories** <br /> At this command the adapter doesn't communicate with the controller, it's a software sided internal function of BSB-LAN.  
|  **/K\<x\>**              | **Query all parameters and values of category \<x\>** <br /> At this command the adapter doesn't communicate with the controller, it's a software sided internal function of BSB-LAN.  
|  **/L=0,0**               | **Deactivate logging to microSD-card temporarily** <br /> In general, the activation/deactivation of the logging function should be done in the file *BSB\_lan\_config.h* before flashing the Arduino. During runtime the logging can be temporarily deactivated by using L=0,0 though, but it only has an effect until the next reboot of the Arduino. 
|  **/L=\<x\>,\<y1\>,\<y2\>,\<y3\>**       | **Set logging interval to \<x\> seconds with (optional) logging parameter \<y1\>,\<y2\>,\<y3\>** <br /> This command can be used to change the logging interval and the parameters that should be logged during runtime. All parameters that should be logged have to be set. After a reboot of the Arduino, again only the parameters are logged which has been defined in *BSB_lan_config.h* initially.   
|  **/LB=\<x\>**            | **Configure logging of bus-telegrams: only broadcasts (\<x\>=1) or all (\<x\>=0)** <br /> When logging bus telegrams (log parameter 30000 as the only parameter), only the broadcast messages (\<x\>=1) or all telegrams (\<x\>=0) are logged.   
|  **/LU=\<x\>**            | **Configure logging of bus-telegrams: only unknown (\<x\>=1) or all (\<x\>=0)** <br /> When logging bus telegrams (log parameter 30000 as the only parameter), only unknown command ids (\<x\>=1) or all telegrams (\<x\>=0) are logged.  
|  **/M\<x\>**              | **Activate (\<x\> = 1) or deactivate (\<x\> = 0) bus monitor mode** <br /> By default bus monitor mode is deactivated (\<x\>=0). <br /> When setting \<x\> to 1, all bytes on the bus are monitored. Each telegram is displayed in hex format with a timestamp in miliseconds at the serial monitor. The html output isn't affected though. <br /> To deactivate the monitor mode, set \<x\> back to 0: `/M0`.  
|  **/N**                   | **Reset & reboot arduino (takes approx. 15 seconds)** <br /> Reset and reboot of the Arduino. <br /> *Note:* Function must be activated in *BSB\_lan\_config.h*: `#define RESET`!  
|  **/NE**                  | **Reset & reboot arduino (takes approx. 15 seconds) and erase EEPROM** <br /> Reset and reboot of the Arduino with additional erasing of the EEPROM. <br /> *Note:* Function must be activated in *BSB\_lan\_config.h*: `#define RESET`!  
|  **/P\<x\>**              | **Set bus type/protocol (temporarily): \<x\> = 0 → BSB \| 1 → LPB \| 2 → PPS** <br />    Changes between BSB (\<x\>=0), LPB (\<x\>=1) and PPS (\<x\>=2). After a reboot of the Arduino, the initially defined bus type will be used again. To change the bus type permanently, adjust the setting `setBusType config` in *BSB\_lan\_config.h* accordingly.     
|  **/P\<x\>,\<y\>,\<z\>**  | **Set bus type/protocol \<x\>, own address \<y\>, target address \<z\> (temporarily)** <br /> Temporarily change of the set bus type and addresses: <br /> \<x\> = bus type (0 = BSB, 1 = LPB, 2 = PPS), <br /> \<y\> = own address and <br /> \<z\> = destination address <br /> Empty values leave the address as it is already set.  
|  **/Q**                   | **Check for unreleased controller-specific parameters**  
|  **/R\<x\>**              | **Query reset-value of parameter \<x\>** <br /> Within the integrated operational unit of the heating system there are reset options available for some parameters. A reset is done by asking the system for the reset value and setting it afterwards.  
|  **/S\<x\>=\<y!z\>**        | **Set value \<y\> for parameter \<x\> with optional destination address \<z\>** <br /> Command for setting values (therefore, write-access must be defined previously in *BSB_lan_config.h*!). Additionally a destination address can be set by using \<z\>. If \<!z\> isn't used, the standard destination address will be used. <br /> To set a parameter to 'off/deactivated', just use an empty value: `http://<ip-address>/S<x>=`.  
|  **/T**                   | **Query optional sensors (DS18B20/DHT22)** <br /> Queries the values of the optional sensors. <br /> DS18B20: the specific sensor id and the measured temperature are displayed. <br /> DHT22: the measured temperature, the relative and the absolute humidity are displayed.  
|  **/V\<x\>**              | **Activate (\<x\> = 1) or deactivate (\<x\> = 0) verbose output mode** <br /> The preset verbosity level is 1, so the bus is 'observed' and all data are displayed in raw hex format additionally. <br /> If the mode should be deactivated, \<x\> has to be set to 0: `/V0` <br /> Verbosity mode affects the output of the serial monitor as well as the (optional) logging of bus data to microSD card. Therefore the card could run out of space quickly, so it's advisable to deactivate the verbosity mode already in the *BSB_lan_config.h*: `byte verbose = 0` <br /> The html output isn't affected by /V1.  
|  **/X**                   | **Query optional MAX!-thermostats** <br /> Queries and displays the temperatures of optional MAX!-thermostats. <br /> *Note:* MAX!-components have to be defined in *BSB\_lan\_config.h* before!  
   

---
    
## 8.2 Special Functions
    
---
    
### 8.2.1 Transmitting a Room Temperature
By using an INF-message, a room temperature can be transmitted to the controller. Therefore you have to activate the function 'room influence' (parameter 750 for circuit 1, parameter 1050 for circuit 2) before. Also write-access has to be defined for BSB-LAN.  
For the room temperature at circuit 1 the virtual parameter 10000 has to be used, for circuit 2 it's parameter 10001. 

***Example:***  
*This command transmits a room temperature of 19.5°C to the circuit 1: `http://<ip-addresse/I10000=19.5`*

***Note: Room Influence Regarding the Room Temperature***   
*FHEM forum user "freetz" has decoded the model behind the "room influence" (parameter 750), so that the effects on the flow temperature became more clear. Thanks a lot for this!*  
His article as well as an Excel spreadsheet can be found [here](https://forum.fhem.de/index.php/topic.29762.msg754102.html#msg754102).
    
---
    
### 8.2.2 Simulating the Presence Function
The function of the presence button is implemented with the special parameters 701 (circuit 1) and 1001 (circuit 2) and has to be executed as a SET-command. Therefore BSB-LAN needs write-access. These parameters (701 & 1001) can not be queried!
   
With an active *automatic* heating mode one has to use `http://<ip-address>/S701=1` to change to the mode 'reduced' and `http://<ip-address>/S701=0` to the change to the mode 'comfort'. The setting is active until the next changement of the heating mode occurs, which is triggered by the time schedule.  
   
***Note: The function of the presence button is only available when the heater is in automatic mode!***

    
---
    
### 8.2.3 Triggering a Manual DHW-Push
Within many controllers there is a (nearly) undocumented function available: a manual DHW push. To initiate a manual DHW push, one has to press and hold the DHW-mode-button at the operational unit. After approx. three seconds a message appears at the display and the heating process starts.  
   
With some controllers this function can also be used with BSB-LAN using a SET-command: `http://<ip-address>/S1601=1`
    
---
    
### 8.2.4 Retrieving and Controlling via JSON
*This function is still 'under development', so changes can occur!*

It's also possible to use JSON to query or set parameters.

-   **Query of categories:**

    `http://<ip-address>/JK=<xx>`  
    Query of a specific category (\<xx\> = number of category)

    `http://<ip-address>/JK=ALL`  
    Query of all categories (including min. and max.)

-   **Query and set parameters via HTTP POST:**

    For this the following URL commands have to be used:  
    `http://<ip-address>/JQ` to query parameters   
    `http://<ip-address>/JS` to set parameters

    The following parameters are useable within these URL commands:
    
    ```
    http://<ip-address>/JQ
    Send: "Parameter"
    Receive: "Parameter", "Value", "Unit", "DataType" (0 = plain value (number), 1 = ENUM (value (8/16 Bit) followed by space followed by text), 2 = bit value (bit value (decimal) followed by bitmask followed by text/chosen option), 3 = weekday, 4 = hour:minute, 5 = date and time, 6 = day and month, 7 = string, 8 = PPS time (day of week, hour:minute))  
    
    http://<ip-address>/JS  
    Send: "Parameter", "Value" (only numeric), "Type" (0 = INF, 1 = SET)  
    Receive: "Parameter", "Status" (0 = error, 1 = OK, 2 = parameter read-only)  
    ```   
      
    The query of multiple parameters within one command is also possible:  
    The command `http://<ip-address>/JQ=<x>,<y>,<z>` queries the parameters \<x\>, \<y\> und \<z\>.  
       
       
-   **Set parameters via Linux command line or „[Curl for Windows](https://curl.haxx.se/windows/)“**   
    Exemplary for parameter 700 (operating mode heating circuit 1) → set to 1 (= automatic mode):
    
    Linux command line:   
    ```
    curl -v -H "Content-Type: application/json" -X POST -d '{"Parameter":"700", "Value":"1", "Type":"1"}' http://<ip-address>/JS
    ```

    Curl for Windows:   
    ```
    curl -v -H "Content-Type: application/json" -X POST -d "{\"Parameter\":\"700\", \"Value\":\"1\", \"Type\":\"1\"}" http://<ip-address>/JS
    ```
    
---
    
### 8.2.5 Checking for Non-Released Controller Specific Command IDs
*Note: It is adviseable to execute this one time query at the initial usage of BSB-LAN.*

`http://<ip-address>/Q`  

This function queries all command ids from the file *BSB\_lan\_defs.h* and sends those ones which aren't marked for the own type of controller as a query to the controller (type QUR, 0x06).  
  
This already happens regularly within parameters for which only one command id is known and creates the already known "error 7" messages. As soon as more than one command id is known for a specific parameter, the first known command id still stays at \"DEV\_ALL\" and is still the standard command id for all controllers. The new command id is then only approved for the new type of controller where that command id comes from. But: It's possible that this new command id also works with other controllers or that it's the 'regular' id. So the URL command /Q now checks all command ids which aren't approved for the own type of controller. By this it's often possible that 'new' parameters becoming available for the own controller.  

***Note:***  
*Within this command, only queries occur - so no changes of any settings within the controller will be changed!*  

If all command ids are already known and approved for the own type of controller, no "error 7" messages occur within the output of the /Q command.  
As an example, this is how the output of the webinterface looks in this case:
    
```
Gerätefamilie: 92 
Gerätevariante: 100 
Geräte-Identifikation: AVS37.294/100 
Software-Version: 2.0 
Entwicklungs-Index: 
Objektverzeichnis-Version: 1.3 
Bootloader-Version: 
EEPROM-Version: 
Konfiguration - Info 2 OEM: 
Zugangscode Inbetriebnahme?: 
Zugangscode Fachmannebene ?: 
Zugangscode OEM?: 
Zugangscode OEM2?: 
Bisher unbekannte Geräteabfrage: 20 
Hersteller-ID (letzten vier Bytes): 58469 
Bisher unbekannte Geräteabfrage: 
Außentemperatur (10003): 
Außentemperatur (10004): 

6225;6226;6224;6220;6221;6227;6229;6231;6232;6233;6234;6235;6223;6236;6237;
92;100;AVS37.294/100;2.0;;1.3;;;;;;;20;58469;;

Starte Test...

Test beendet.

Fertig. 
```
    
If some 'new' parameters have been identified by the function of /Q, the output of the webinterface looks like this (e.g.):
    
```
Gerätefamilie: 92 
Gerätevariante: 100 
Geräte-Identifikation: AVS37.294/100 
Software-Version: 2.0 
Entwicklungs-Index: 
Objektverzeichnis-Version: 1.3 
Bootloader-Version: 
EEPROM-Version: 
Konfiguration - Info 2 OEM: 
Zugangscode Inbetriebnahme?: 
Zugangscode Fachmannebene ?: 
Zugangscode OEM?: 
Zugangscode OEM2?: 
Bisher unbekannte Geräteabfrage: 20 
Hersteller-ID (letzten vier Bytes): 58469 
Bisher unbekannte Geräteabfrage: 
Außentemperatur (10003): 
Außentemperatur (10004): 

6225;6226;6224;6220;6221;6227;6229;6231;6232;6233;6234;6235;6223;6236;6237;
92;100;AVS37.294/100;2.0;;1.3;;;;;;;20;58469;;


Starte Test...

5
5 Uhrzeit und Datum - Sommerzeitbeginn Tag/Monat: error 7 (parameter not supported) 
DC C2 0A 0B 06 3D 05 04 B3 DA F8 
DC 8A 42 14 07 05 3D 04 B3 00 FF 03 19 FF FF FF FF 16 C4 C8 
6
6 Uhrzeit und Datum - Sommerzeitende Tag/Monat: error 7 (parameter not supported) 
DC C2 0A 0B 06 3D 05 04 B2 CA D9 
DC 8A 42 14 07 05 3D 04 B2 00 FF 0A 19 FF FF FF FF 16 80 41 

Test beendet.

Fertig.  
```  
    
In general, the output of /Q (together with the brand and the specific name of that type of heating system) should be reported in any case, so that we can add that system to the list of reported systems which work with BSB-LAN.  
But: Especially if any error7-messages occur this should be done, so that the reported error7-parameters can be approved for that specific type of controller and can be made available.  
For reporting the system, please copy and paste the output of the webinterface (like the examples above) and post it either in the german [FHEM-Forum](http://forum.fhem.de/index.php/topic,29762.0.html) or send it via email to Frederik or me (Ulf). Please don't forget to add the name of the brand and the specific type of your heating system!  
Thanks.
        
---
    
### 8.2.6 IPWE Extension  
The IPWE extension offers the presentation of various previously defined parameters by the usage of just one short URL. To access this table overview, the following URL has to be used:  
`<ip-address>/ipwe.cgi`.  
*Note: If the optional security function of the passkey is used, the passkey has NOT to be added within the URL in this case!*  
  
To use the function of the IPWE extension, one has to make two settings in the file `BSB_lan_config.h` before flashing the arduino:  
- The definement `#define IPWE` has to be active.  
- The desired parameters which should be displayed have to be listed.  
  
Additionally to the set parameters, the values of optional connected sensors (DHT22 / DS18B20) will be listed automatically. If DS18B20 sensors are connected, the specific sensor id of each sensor will also be displayed.  
  
Two short notes:  
- If there are -by accident- parameters defined which aren't supported by the specific heating system, the non-existent values will be displayed as "0.00". So don't get that wrong - it doesn't mean, that that specific value is "0.00"! Before you define the parameters, it's best to check before and make sure, that the heating system really offers these parameters.  
- Because the IPWE extension was originally designed to implement values of a specific wireless weather station, there will be some columns which doesn't seem to make sense, like "Windgeschwindigkeit" (wind speed) - you can just ignore that.  

    
--- 
    
### 8.2.7 Changing the Date, Time and Time Programs
Changing the date, time and time programs is only possible by using a special URL command, it is not possible via webinterface.  
To use this feature, BSB-LAN needs write access (see [chap. 5](chap05.md)).  
  
*Changing the date and time*  
The follwoing URL command sets the date to the fourth of january 2019 and the time to 08:15pm:  
`/S0=04.01.2019_20:15:00`  
Using this function it is possible to sychronize the time with (e.g.) a NTP time server. 
   
*Changing time programs*  
The following URL command sets the time program for wednesday at heating circuit 1 (parameter 502) to 05:00am-10:00pm:  
`/S502=05:00-22:00_xx:xx-xx:xx_xx:xx-xx:xx`  
     
---  
   
### 8.2.8 Transmitting an Alternative Outside Temperature
Certain specific controller types allow the usage of different wireless components. Amongst other things there is also a wireless outside temperature sensor available. Within these compatible controllers it is possible to use BSB-LAN to transmit an alternative outside temperature.   
   
Until now it seems that only controllers of the types LMS and RVS are compatible. Older types of controllers (e.g. LMU and RVA) don't seem to be compatible.  
   
For using this function the wired temperature sensor has to be deconnected from the controller. The transmission of the alternative outside temperature has to be done regularly within a time windows of (approx.) max. 10 minutes, but it is adviseable to transmit the value every 60 to 120 seconds. 
   
To use this function, BSB-LAN needs write access (see [chap. 5](chap05.md)). The outside temperature has to be transmitted as an INF message (with the virtual parameter 10003) using the URL command   
`<ip-address>/I10003=xx`  
where xx is the outside temperature in °C (degrees celcius); fractional values are possible.  
   
*Example:*  
With `<ip-address>/I10003=16.4` the outside temperature of 16.4°C is transmitted; `<ip-address>/I10003=9` transmits 9°C.  
   
   
---  
   
[Further on to chapter 9](chap09.md)      
[Back to TOC](toc.md)   



