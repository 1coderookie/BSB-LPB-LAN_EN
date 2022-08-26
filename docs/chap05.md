[Back to TOC](toc.md)  
[Back to chapter 4](chap04.md)    
   
---      

# 5. BSB-LAN: Query and Control
Because the webinterface basically is just set 'on top' to achieve access without further programs like FHEM or openHAB, it's possible to access the functions and parameters with external programs.  
  
    
---
    
## 5.1 URL Commands
  
| Note |
|:-----|
| The values and parameters in the following list of the URL commands must be written without the brackets. <br> E.g.: URL command `/<x>` for the simple query of parameter 8700 = `/8700`. |  
   
   

| URL Command           | Effect                                                                    |
|:----------------------|:------------------------------------------------------------------------------|
|  `/<x>`               | `Query value/setting of parameter <x>`  
|  `/<x>!<addr>`        | `Query value/setting of parameter <x> for destination address <addr>`  
|  `/<x>/<y>/<z>`     | `Query values/settings of parameters <x>, <y> and <z>`   
|  `/<x>-<y>`         | `Query values/settings of parameters <x> to <y>`  
|  `/<x>!<addr>-<y>`  | `Query values/settings of parameters <x> to <y> for destination address <addr>`  
|  `/A=0`                   | `Disable 24h average calculation temporarily` <br /> `Disables the 24h average calculation temporarily (until the next reboot of the Arduino). For a complete deactivation, uncomment all parameters for that function in the file BSB_LAN_config.h.`  
|  `/A=<x>,<y>,<z>`       | `Change 24h average value calculation of parameters <x>, <y>, <z>` <br /> `During runtime up to 20 new parameters can be defined for the 24h average calculation. These parameters are kept until the next reboot of the Arduino.`  
|  `/B0`                  | `Reset counter of accumulated burner-runtime and -cycles`  
|  `/C`                   | `Configuration page (aka webconfig) of BSB-LAN`  
|  `/CO`                  | `Display the configuration of BSB-LAN`  
|  `/D or /DD`            | `Display logfile from the microSD-card` <br /> `Displays the logfile datalog.txt which contains the values of the logged parameters defined in the file BSB_LAN_config.h.`
|  `/DG`                  | `Graphical display of the logfile from microSD-card` <br /> `Shows graphical output (graphs) of the logged values.` <br /> `Note: If you use Javascript blockers, make sure you allow access to cdn.jsdelivr.net and d3js.org, because the Arduino just loads the csv-file into the browser and the D3-framework converts the data.` <br /> `Mouseover, click and mouse wheel actions within the graphical display provide various control options:` <br /> `- better legibility for value numbers with plot lines close to each other (mouseover on plot)` <br /> `- user can interactively highlight plot lines for improved overview (mouseover on legend entries)` <br /> `- user can interactively disable plot lines for improved overview and vertical scaling (click on legend entries)` <br /> `- added zoom (mousewheel/pinch on plot) and pan capability (drag zoomed-in plot)` |   
|  `/DJ`                  | `Display logfile journal.txt from the microSD-card` <br /> `Displays the logfile journal.txt which shows the content of received and transmitted telegrams. This log is useful for debugging and the search for unknown parameters. To use this function, you must enable the LOGGER module in the file BSB_LAN_config.h and set the first element of the log_parameters array to 30000.`  
|  `/D0`                  | `Reset both logfiles & create new header` <br /> `This command deletes the content of the files datalog.txt and journal.txt and creates a new csv-header for datalog.txt. This command should be executed before first logging.`     
|  `/DD0`               | `Remove logfile datalog.txt only`  
|  `/DJ0`               | `Remove logfile journal.txt only`  
|  `/E<x>`              | `Display ENUM-values of parameter <x>` <br /> `At this command the adapter doesn't communicate with the controller, it's a software sided internal function of BSB-LAN. This command is only available for parameters of the type VT_ENUM, VT_CUSTOM_ENUM, VT_BITS and VT_CUSTOM_BITS.`  
|  `/G<x>`              | `GPIO: Query pin <x>` <br /> `Displays the actual state of GPIO pin <x>, where <y>=0 is LOW and <y>=1 is HIGH.`  
|  `/G<x>=<y>`        | `GPIO: Set pin <x> to HIGH (<y> = 1) or LOW (<y> = 0)` <br /> `Sets GPIO pin <x> to LOW (<y>=0) or HIGH (<y>=1).` <br /> `Reserved pins which shouldn't be allowed to be set can be defined previously at GPIO_exclude in the file BSB_lan_config.h.` 
|  `/G<x>,I`            | `GPIO: Query pin <x> while setting to INPUT` <br /> `If e.g. a coupling relay is connected to a GPIO pin and the state should just be queried, this command should be used. It sets the GPIO pin to input (default they are set to output) and keeps this as long until it's changed by using G<x>=<y>. After that, it's set to output again until the next "I" sets it to input again.`  
|  `/I<x>=<y>`        | `Send INF-message to parameter <x> with value <y>` <br /> `Some values can't be set directly, the controller gets these values by a TYPE_INF-message. As an example, the room temperature of 19.5°C should be transmitted: http://<ip-address>/I10000=19.5.`  
|  `/JC=<x>,<y>,<z>`         	| `JSON: Query possible values for parameters <x>, <y> and <z> for ENUM type parameters` <br /> `The format of the returned data is the same as the command /JK=<x>. Unlike the /JQ command, it does not return the current parameter values.`   
|  `/JI`                | `JSON: Display configuration of BSB-LAN`  
|  `/JK=<x>`         	| `JSON: Query all parameters of category <x>`  
|  `/JK=ALL`          	   | `JSON: List all categories with corresponding parameter numbers`  
|  `/JL`                | `JSON: Creates a list of the configuration in JSON format`  
|  `/JQ=<x>,<y>,<z>`      | `JSON: Query parameters <x>, <y> and <z>`  
|  `/JQ`                  | `JSON: Query parameters`  
|  `/JR<x>`                | `JSON: Query reset-value of parameter <x>` <br /> `Within the integrated operational unit of the heating system there are reset options available for some parameters. A reset is done by asking the system for the reset value and setting it afterwards.`    
|  `/JS`                  | `JSON: Set parameters`  
|  `/JV`                | `JSON: Queries the version of the JSON-API. Payload: {"api_version": "major.minor"}`  
|  `/JW`                   | `JSON: Reads the list of configuration created with /JL and adjusts the settings.`  
|  `/K`                   | `List all categories` <br /> `At this command the adapter doesn't communicate with the controller, it's a software sided internal function of BSB-LAN.`  
|  `/K<x>`              | `Query all parameters and values of category <x>` <br /> `At this command the adapter doesn't communicate with the controller, it's a software sided internal function of BSB-LAN.`  
|  `/L=0,0`               | `Deactivate logging to microSD-card temporarily` <br /> `In general, the activation/deactivation of the logging function should be done in the file BSB_lan_config.h before flashing the Arduino. During runtime the logging can be temporarily deactivated by using L=0,0 though, but it only has an effect until the next reboot of the Arduino.` 
|  `/L=<x>,<y1>,<y2>,<y3>`       | `Set logging interval to <x> seconds with (optional) logging parameter <y1>,<y2>,<y3>` <br /> `This command can be used to change the logging interval and the parameters that should be logged during runtime. All parameters that should be logged have to be set. After a reboot of the Arduino, again only the parameters are logged which has been defined in BSB_lan_config.h initially.`   
|  `/LB=<x>`            | `Configure logging of bus-telegrams: only broadcasts (<x>=1) or all (<x>=0)` <br /> `When logging bus telegrams (log parameter 30000 as the only parameter), only the broadcast messages (<x>=1) or all telegrams (<x>=0) are logged.`   
|  `/LD`                | `Disable logging of telegrams to journal.txt`  
|  `/LE`                | `Enable logging of telegrams to journal.txt`  
|  `/LU=<x>`            | `Configure logging of bus-telegrams: only unknown (<x>=1) or all (<x>=0)` <br /> `When logging bus telegrams (log parameter 30000 as the only parameter), only unknown command ids (<x>=1) or all telegrams (<x>=0) are logged.`  
|  `/M<x>`              | `Activate (<x> = 1) or deactivate (<x> = 0) bus monitor mode` <br /> `By default bus monitor mode is deactivated (<x>=0).` <br /> `When setting <x> to 1, all bytes on the bus are monitored. Each telegram is displayed in hex format with a timestamp in miliseconds at the serial monitor. The html output isn't affected though.` <br /> `To deactivate the monitor mode, set <x> back to 0: /M0.`  
|  `/N`                   | `Reset & reboot arduino (takes approx. 15 seconds)` <br /> `Reset and reboot of the Arduino.` <br /> `Note: Function must be activated in BSB_lan_config.h by #define RESET`  
|  `/NE`                  | `Reset & reboot arduino (takes approx. 15 seconds) and erase EEPROM` <br /> `Reset and reboot of the Arduino with additional erasing of the EEPROM.` <br /> `Note: Function must be activated in BSB_lan_config.h by #define RESET`
|  `/P<x>`              | `Set bus type/protocol (temporarily): <x> = 0 → BSB / 1 → LPB / 2 → PPS` <br />  `Changes between BSB (<x>=0), LPB (<x>=1) and PPS (<x>=2). After a reboot of the Arduino, the initially defined bus type will be used again. To change the bus type permanently, adjust the setting setBusType config in BSB_lan_config.h accordingly.`     
|  `/P<x>,<y>,<z>`  | `Set bus type/protocol <x>, own address <y>, target address <z> (temporarily)` <br /> `Temporarily change of the set bus type and addresses:` <br /> `<x> = bus type (0 = BSB, 1 = LPB, 2 = PPS),` <br /> `<y> = own address and` <br /> `<z> = destination address` <br /> `Empty values leave the address as it is already set.`  
|  `/Q`                   | `Check for unreleased controller-specific parameters`  
|  `/R<x>`              | `Query reset-value of parameter <x>` <br /> `Within the integrated operational unit of the heating system there are reset options available for some parameters. A reset is done by asking the system for the reset value and setting it afterwards.`  
|  `/S<x>=<y!z>`        | `Set value <y> for parameter <x> with optional destination address <z>` <br /> `Command for setting values (therefore, write-access must be defined previously in BSB_lan_config.h!). Additionally a destination address can be set by using <z>. If <!z> isn't used, the standard destination address will be used.` <br /> `To set a parameter to 'off/deactivated', just use an empty value: http://<ip-address>/S<x>=`  
|  `/U`                   | `Displays the user-defined variables if used in BSB_lan_custom.h` <br /> `For the creation of one’s own subroutines in BSB_lan_config.h two arrays of 20 bytes size, custom_floats[] und custom_longs[], are available. If used, these can be displayed via URL command /U and can be useful to query own sensors in BSB_lan_custom.h and display the results on the web-interface via /U.`  
|  `/V<x>`              | `Activate (<x> = 1) or deactivate (<x> = 0) verbose output mode` <br /> `The preset verbosity level is 1, so the bus is 'observed' and all data are displayed in raw hex format additionally.` <br /> `If the mode should be deactivated, <x> has to be set to 0: /V0` <br /> `Verbosity mode affects the output of the serial monitor as well as the (optional) logging of bus data to microSD card. Therefore the card could run out of space quickly, so it's advisable to deactivate the verbosity mode already in the BSB_lan_config.h: byte verbose = 0` <br /> `The html output isn't affected by /V1.`  
|  `/W`                   | `With a preceding /W the URL commands C, S and Q return data without HTML header and footer (e.g.:  /WC or /WS<x>=<y!z>); module WEBSERVER has to be compiled!`  
|  `/X`                   | `Query optional MAX!-thermostats` <br /> `Queries and displays the temperatures of optional MAX!-thermostats.` <br /> `Note: MAX!-components have to be defined in BSB_lan_config.h before!`  

      
---

## 5.2 MQTT
  
BSB-LAN supports the MQTT protocol, so the values and settings of the heating controller can be retrieved via MQTT.  
To use MQTT with BSB-LAN, it is mandatory that the definition "#define LOGGER" in the file *BSB_LAN_config.h* is activated. This is already the case in the default setting.  
  
The parameters to be sent (queried by BSB-LAN, the transmission interval (only one interval possible for all parameters!) and the other MQTT-specific settings (broker, topic, etc.) are to be set either via web configuration or directly in the file *BSB_LAN_config.h*. Please refer to the explanations in the corresponding subchapters of [chap. 5](chap05.md).  
  
Examples for an integration of BSB-LAN can be found in the corresponding subchapters of [chap. 11](chap11.md). 

| Note |
|:--------|
| If you use the MQTT function with fixed logging parameters and logging interval, make sure that you adjust the logging interval (= MQTT send interval)! <br> By default 3600 is set here, which means that the parameters are sent every 3600 *seconds*, so every 60 *minutes* and thus *hourly*! So if you set up your MQTT broker and you wonder why you don't receive values, check the logging interval at first place! |  
  
BSB-LAN uses the subtopic "status" below the defined "MQTTTopicPrefix" to publish its online state. Based on the default setting this would be "BSB-LAN/status". This allows you to track whether BSB-LAN is actually publishing current readings and able to receive commands.  
If BSB-LAN is available, the topic contains the value "online", otherwise you'll see "offline". The message is made persistant via the retain-flag, thus, the subscriber does not have to have the topic subscribed during BSB-LAN startup.  
Any restart initiated by the firmware (e.g. the URL-command /N) will immediately set the topic to "offline". Any uncontrolled shutdown (e.g. a power outage or some firmware flashing) will cause the broker to transmit the offline-message after a (broker specific) timeout.    
  
In addition to (broker-side) pure receiving, it is also possible to query and/or send control commands (URL commands /S and /I) to BSB-LAN from the broker via MQTT. Of course, BSB-LAN must be granted write access to the controller if one wants to change settings.  
  
The command syntax is:  

`set <MQTT server> publish <topic> <command>`  

- `<MQTT server>` = The name of the MQTT server.  

- `<topic>` = Default setting is "BSB-LAN", otherwise the defined "MQTTTopicPrefix" in the file *BSB_LAN_config.h* accordingly. If no topic is defined (not advisable), "FromBroker" must be taken as topic.  

- `<command>` = The query of the specific parameter or the corresponding parameter-specific URL command /S or /I.  

  | Attention |
  |:----------|
  | Only one query/command is possible at a time, so no parameter ranges can be queried!|  
  
Subsequently BSB-LAN sends back an acknowledgement of receipt ("ACK_\<command\>").  
   
| Example |
|:--------|
| The command `set mqtt2Server publish BSB-LAN /S700=1` sends from the MQTT broker named "mqtt2Server" the command "/S700=1" with the topic "BSB-LAN" and causes a mode switch to automatic mode. |
| The command `set mqtt2Server publish BSB-LAN /700` sends from the MQTT broker named "mqtt2Server" the command "/700" with the topic "BSB-LAN" and causes a query of parameter 700. | 
  
---

## 5.3 JSON
  
-  **Query of categories:**

    `http://<ip-address>/JK=<xx>`  
    Query of a specific category (`<xx>` = number of category)

    `http://<ip-address>/JK=ALL`  
    Query of all categories (including min. and max.)

-  **Query and set parameters via HTTP POST:**

    For this the following URL commands have to be used:  
    `http://<ip-address>/JQ` to query parameters   
    `http://<ip-address>/JS` to set parameters

    The following parameters are usable within these URL commands:
    
    ```
    http://<ip-address>/JQ
    Send: "Parameter"
    Receive: "Parameter", "Value", "Unit", "DataType" (0 = plain value (number), 1 = ENUM (value (8/16 Bit) followed by space followed by text), 2 = bit value (bit value (decimal) followed by bitmask followed by text/chosen option), 3 = weekday, 4 = hour:minute, 5 = date and time, 6 = day and month, 7 = string, 8 = PPS time (day of week, hour:minute)), "readonly" (0 = read/write, 1 = read only parameter), "error" (0 - ok, 7 - parameter not supported, 1-255 - LPB/BSB bus errors, 256 - decoding error, 257 - unknown command, 258 - not found, 259 - no enum str, 260 - unknown type, 261 - query failed), "isswitch" (1 = it VT_ONOFF or VT_YESNO data type (subtype of ENUM), 0 = all other cases)  
    
    http://<ip-address>/JS  
    Send: "Parameter", "Value", "Type" (0 = INF, 1 = SET)  
    Receive: "Parameter", "Status" (0 = error, 1 = OK, 2 = parameter read-only)  
    ```   
      
- The query of multiple parameters within one command is also possible:  
  The command `http://<ip-address>/JQ=<x>,<y>,<z>` queries the parameters `<x>, <y>, <z>`.  
       
       
- Example for setting parameters via *Linux command line* or *„[Curl for Windows](https://curl.haxx.se/windows/)“* with parameter 700 (operating mode heating circuit 1) → set to 1 (= automatic mode):
    
    Linux command line:   
    ```
    curl -v -H "Content-Type: application/json" -X POST -d '{"Parameter":"700", "Value":"1", "Type":"1"}' http://<ip-address>/JS
    ```

    Curl for Windows:   
    ```
    curl -v -H "Content-Type: application/json" -X POST -d "{\"Parameter\":\"700\", \"Value\":\"1\", \"Type\":\"1\"}" http://<ip-address>/JS
    ```
    
---
    
***User "hacki11" developed a detailed and interactive [documentation of the JSON API](https://editor.swagger.io/?url=https://raw.githubusercontent.com/fredlcore/bsb_lan/master/openapi.yaml).      
Thanks a lot!***  

| Note for developers |
|:-----------------------|
| The API can be tested on your own system using [Postman](https://www.postman.com). To do this you have to add the URL https://raw.githubusercontent.com/fredlcore/bsb_lan/master/openapi.yaml in File/Import/Link and (if necessary) change the specific settings like address, basic auth data etc. |   

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/swagger_api-docu.png">  
    

In addition to the descriptions including examples of the individual commands, all informations about the types, formats, possible values, etc. sre also listed. 

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/swagger_api-docu_schemes.png">  


| Notes | 
|:------|
| JSON commands can also be used via Linux command line or "[Curl for Windows](https://curl.haxx.se/windows/)". In the above mentioned interactive API documentation, the corresponding Curl commands can be generated and then copied for further use (the IP must be adjusted). To do this, proceed as follows: <br> 1. Click on the desired operation, e.g. "/JQ={parameterIds}". <br> 2. Click on "Try it out" on the right side of the window. <br> 3. Enter the desired parameter(s) (in the example shown below: 700,8300). <br> 4. Click on "Execute". <br> In the "Responses" field you will see the URL and Curl commands you can copy. | 
| Attention: The character combination `%2C` when listing multiple parameters is inserted by Swagger instead of the comma. If you want to copy and use the URL/Curl commands, please replace each `%2C` with a `,` (comma)! |  


<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/curl-beispiel.png"> 
    
*The output of the URL/Curl command.*  


<!-- *This function is still 'under development', so changes may occur!*

<!-- It's also possible to use JSON to query or set parameters.

<!-- -   **Query possible values for parameters:**
    `http://<ip-address>/JC=<x>,<y>,<z>`  
    Query possible values for parameters `<x>,<y>,<z>`. The format of the returned data is the same as the command `/JK=<x>`.
    
<!-- -   **Query the configuration of BSB-LAN:**
    `http://<ip-address>/JI`  
    Query configuration of BSB-LAN. Configuration will be reported in a JSON friendly structure.
    
<!-- 
      
<!-- -   **Query the reset value of a parameter:**  
    `http://<IP-Adresse>/JR<x>` → Queries the reset-value of parameter <x>. Within the integrated operational unit of the heating system there are reset options available for some parameters. A reset is done by asking the system for the reset value and setting it afterwards (JSON: via /JS).    
  
<!-- -   **Backup and restore the config settings of BSB-LAN:**  
    
    `http://<IP-Adresse>/JL` → Creates a list of the configuration in JSON format.  
    
    `http://<IP-Adresse>/JW` → Reads the list of configuration created with /JL and adjusts the settings.  
      
    *Note:* For the usage of this function the module "JSONCONFIG" (see file *BSB_lan_config.h*) has to be compiled!  -->

---
   
[Further on to chapter 6](chap06.md)      
[Back to TOC](toc.md)   



