[Back to TOC](toc.md)  
[Back to chapter 4](chap04.md)    
   
---  

# 5. Configuration of the BSB-LAN Software v2.x   
    
## 5.1 Configuration via Webinterface  
  
*The description of this new function is still in progress.*  
  
## 5.2 Configuration by Adjusting the Settings Within *BSB_lan_config.h*  
  
The BSB-LAN software can be configured by adjusting the settings in the file *BSB_lan_config.h*. For this purpose, all possible settings are listed below in the same order as in the file BSB_lan_config.h. It is therefore advisable to work through the settings point by point.

*Note:  
To 'uncomment' a definement for making it active means to delete the two slashes in front of the hashtag. To 'comment out' a definement means to deactivate it by adding two slashes in front of the hashtag. E.g.:  
A deactivated definement: `//#define XYZ`  
An activated definement: `#define XYZ`*  
  
---  
  
-  The **language of the user interface** of the web interface of the adapter as well as the category and parameter designations must be selected or defined. For "English" the following definition must be selected:
   `#define LANG EN`  
   Starting with BSB-LAN v.042 it is possible to use BSB-LAN in other languages, too, whereby in principle any language can be supported (only' the corresponding translations have to be created).  
   Currently available are: Czech (CZ), German (DE), Danish (DK), English (EN), Spanish (ES), Finnish (FI), French (FR), Greek (GR), Hungarian (HU), Italian (IT), Dutch (NL), Polish (PL), Russian (RU), Swedish (SE), Slovenian (SI) and Turkish (TR). If certain expressions are not available in the specific language, the English expression is automatically displayed. If this is also not available, the German expression is finally displayed.
  
---
  
-  **Load configuration settings from EEPROM or the file BSB_lan_config.h:**  
   `byte UseEEPROM = 1;`  
   According to the default setting, the configuration settings are read from the EEPROM when BSB-LAN is started. As a fallback the variable can be set to '0', then the settings are read from the file *BSB_lan_config.h*.
  
---  
  
***Network settings:***  
  
-  **MAC address of the ethernet shield:**  
   `byte mac[] = { 0x00, 0x80, 0x41, 0x19, 0x69, 0x90 };`  
   The default MAC address can be kept. A change is usually only necessary if more than one adapter is used (in any case, you should make sure that each MAC address only occurs *once* in the network!). In this case, changes should only be made to the last byte (e.g. 0x91, if a second adapter is used).  
   *Important note:*  
   *The MAC address assigned here also influences the host name (or is a part of it), which is assigned by the router when using DHCP (see below): The host name consists of the identifier "WIZnet" and the last three bytes of the MAC address.*  
   
   *For the default MAC address mentioned above, the host name is thus "WIZnet196990". This host name is usually also displayed as such in the router. In this case the web interface of BSB-LAN can be reached in the browser under `http://wiznet196990`.  
   *If a second adapter is used and the MAC address will be changed to*  
   *`byte mac[] = { 0x00, 0x80, 0x41, 0x19, 0x69, 0x91 };`*  
   *the host name is "WIZnet196991" or `http://wiznet196991`.*

-  **Ethernet port:**  
   `uint16_t HTTPPort = 80;`  
   Port 80 for HTTP is preset.  
   
-  **DHCP:**  
   `boolean useDHCP = true;`  
   By default DHCP is used. If this is not desired, but you want to assign a fixed IP address yourself, set *false*.  
   
   *Important note:*  
   *Please see the notes above regarding the hostname based on the MAC address. The IP given by the router will also appear within the start process of the Arduino Due within the serial monitor of the Arduino IDE.*  
  
-  **IP address:**  
   `byte ip_addr[4] = {192,168,178,88};`  
   Fix IP address of the adapter, if DHCP is not used - *please note the commas instead of dots!*  
   *Note: If you want to give the adapter a fix IP, please make sure that it occurs only *once* in your network!*  
   
-  **Gateway address:**  
   `byte gateway_addr[4] = {192,168,178,1};`  
   IP address of the gateway (usually the one of the router itself) - *please note the commas instead of dots!*  
   
-  **Subnet:**  
   `byte subnet_addr[4] = {255,255,255,0};`  
   Address of the subnet - *please note the commas instead of dots!*  
   
---
  
-  **Debugging and related settings:**  
   - `#define DEBUG` → activate the debug mode (see following options)  
   
   - `byte debug_mode = 1;` → The following debug options are available:  
   0 - debugging deactivated  
   1 - send debug messages to the serial interface (e.g. for using the aerial monitor of the Arduino IDE); default setting  
   2 - send debug messages to a TelNet client instead of the serial interface  
   
   - `byte verbose = 1;` → By default the verbose mode is activated (= 1), so that (besides the raw data) the respective plaintext (if available) of parameters and values is displayed. It is advisable to leave this setting as it facilitates possible trouble shooting. Furthermore, this setting is necessary if telegrams and command IDs of new parameters should be decoded.  
   
   - `byte monitor = 0;` → Bus monitor mode, deactivated by default; set to '1' to activate  
   
   - `boolean show_unknown = true;` → All parameters including the *unknown parameters* (error message "error 7 (parameter not supported)") are displayed when querying via web interface (e.g. when querying a complete category); default setting.  
    If you want to hide the 'unknown' parameters that are not supported by the controller of your heating system (e.g. when querying a complete category), you have to set the variable to 'false' (`boolean show_unknown = false;`). *The parameters are still queried in such a query (e.g. for a complete category) though.*  
    
---
  
***Security functions:***  
There are several options to control and protect access to your heating system. However, keep in mind, that even activating all three options are no guarantee that a versatile intruder with access to your (W)LAN won't be able to gain access. In any case, no encryption of data streams is provided from the Arduino itself. Use VPN or a SSL proxy if that is a must for you and connect the Arduino wired to the VPN server or SSL proxy.  
The following three security options are available within BSB-LAN:
   
-  **Passkey:**  
   To protect the system from unwanted access from outside, the **function of the security key (PASSKEY)** can be used (very easy and not really secure!):
   `char PASSKEY[64] = "";`  
   
   To use this function, add a certain sequence of alphanumerical characters as a simple security function, e.g. `char PASSKEY[64] = "1234";` → in this example the passkey is '1234'. If no alphanumerical sequence is set (default), the passkey function remains deactivated.   
   
   Note:  
   If PASSKEY is defined, the URL has to contain the defined passkey as first element, e.g.: `URL/1234/` to view the main website (don't forget the trailing slash!). Only within the URL of the optional [IPWE extension](chap08.md#826-ipwe-cgi) the passkey has NOT to be added!   


-  **Trusted IP:**  
   `byte trusted_ip_addr[4] = {0,0,0,0};`  
   `byte trusted_ip_addr2[4] = {0,0,0,0};`  

   Within these variables you can define up to two IP addresses from which the access to BSB-LAN will then be possible (e.g. sever of your home automation system).  
   If the default setting will not be changed or if the first number is a '0', this function is deactivated (default setting).  
   
-  **User-Pass:**  
   `char USER_PASS_B64[64] = "";`  
   Provides a (base64-coded) username/password based access (default setting: deactivated). No encryption! 
   As an example, the username 'atari' and the password '800xl' are given as an option which could be used; the string for this combination is `YXRhcmk6ODAweGw=`:  
   `//char USER_PASS_B64[64] = "YXRhcmk6ODAweGw=";`    
   If you want to use this option, visit a website like https://www.base64encode.org/ to encode your own username/password combination in the format *username:password* and enter the specific string.   
  
--- 
  
**Settings for optional sensors:**  
  
-  **OneWire temperature sensors (DS18B20):**  
   `#define ONE_WIRE_BUS`  
   `boolean enableOneWireBus = true;`  
   `byte One_Wire_Pin = 7;`  
   
   If you want to use OneWire temperature sensors (DS18B20), the definition must be activated, the variable must be set to *true*  and the corresponding pin (DATA connection of the sensor on the adapter board / Arduino Due) must be defined. *Note: Make sure that you don't use any of the protected pins which are listed further down below!*   
    By default, the module is activated and pin 7 for DATA is set.  
    
    If you don't want to use DS18B20 sensors, the variable must be set to false:  
    `boolean enableOneWireBus = false;`  
    
-  **DHT22 sensors:**  
   `#define DHT_BUS`  
   `byte DHT_Pins[10] = {2, 3};`  
   
   If you want to use DHT22 sensors (temperature & humidity), the definement must be activated and the corresponding pin(s) must be be defined. *Note: Make sure that you don't use any of the protected pins which are listed further down below!*     
   By default, the module is activated and the pins 2 & 3 are set for the DATA of two sensors (in summary - each sensor has to be connected to a different pin).     
  
---
  
-  **24h averages:**  
   `#define AVERAGES`  
   If you want to create 24h averages from certain parameters, the definement must be activated (default setting). 
   
   `boolean logAverageValues = true;`
   If you want the averages to be logged to a microSD card within the file *averages.txt*, the default setting ('true') of the variable must be kept. If you don't want to have these values logged, the variable must be set to `false`.  
   
   Further more you have to list the specific numbers of the parameters you want to be calculated. E.g.:  
   ```
   int avg_parameters[40] = {
   8700,	// outside temperature
   8830	// DHW (warm water) temperature
   };
   ```
  
---

-  **Logging (also to microSD card) and/or usage of MQTT:**  
   `#define LOGGER` → The logging module will be compiled. *Note: This is a requirement for logging to a microSD card as well as for using MQTT!*   
   
   In the following, various settings can/should be made:  
   - If 'raw' *bus telegrams* should be logged, the selection can be specified. The telegrams are stored within the file *journal.txt* on the microSD card. By default the logging of these bus messages is deactivated:
   `int logTelegram = LOGTELEGRAM_OFF;`  
    
   The following options are available:  
   `LOGTELEGRAM_OFF` → no logging of bus telegrams (default setting)  
   `LOGTELEGRAM_ON` → all bus telegrams are logged  
   `LOGTELEGRAM_ON + LOGTELEGRAM_UNKNOWN_ONLY` → only unknown bus telegrams are logged  
   `LOGTELEGRAM_ON + LOGTELEGRAM_BROADCAST_ONLY` → only broadcast telegrams are logged  
   `LOGTELEGRAM_ON + LOGTELEGRAM_UNKNOWNBROADCAST_ONLY` → only unknown broadcast telegrams are logged  
  
  - `boolean logCurrentValues = false;`  
  The data of the parameters to be logged are stored in the file 'datalog.txt' on the microSD card (deactivated by default). For activating this function the variable must be set to 'true'.  
  
  - `unsigned long log_interval = 3600;`  
  The desired logging interval in seconds.  
  *Note: This interval must also be set for using MQTT, even though if no data should be logged!* 
  
  The parameters that should be logged must be listed:  
  ```
  int log_parameters[40] = {
  8700,	// outside temperature
  8830	// DHW (warm water) temperature
  };
  ```
  

---

***NOTE: THE DESCRIPTION WILL BE COMPLETED SOON, THE FOLLOWING IS OLD CONTENT!!***    
  
- If you want to use **MQTT**, you have to activate and adjust the following definements and settings:  
`#define MQTTBrokerIP 192,168,1,20` - insert the IP of the MQTT broker. You don't need to define the standard port 1883 though.  
`#define MQTTUsername "User"` - Set the username for the MQTT broker here or comment it out if no username/password is used.  
`#define MQTTPassword "Pass"` - Set the password for the MQTT broker here or comment it out if no password is used.  
`#define MQTTTopicPrefix "BSB-LAN"` - Choose the "topic" for the MQTT messages here (default: "BSB-LAN"). The messages will have the topic format `BSB-LAN/<parameter>` with the belonging value in the payload.    
`#define MQTT_JSON` - The parameters transmitted via MQTT won't be transmitted separately, they'll be transmitted within a JSON structure.  
`#define MQTTDeviceID "MyHeater"` - Passes the JSON structure below the here defined DeviceID.  
Example of this JSON structure: `{"MQTTDeviceID": {"status":{"log_param1":"value1","log_param2":"value2"}, ...}}`  
    
*Note:  
If you want to use the MQTT function, you have to list the desired parameters within the variable of the logging parameters (see logging example above). If you only want to use MQTT but niot the logging function to the microSD card at the same time, just deactivate the logging definement (`//#define LOGGER`). The sending of the 'log_parameters' to the MQTT broker will happen every 'log _interval' seconds (see logging example above).*  

- If you want to use the **IPWE extension**, you have to activate this definement:  
`#define IPWE`  
The parameters that should be shown in the [IPWE extension](chap08.md#826-ipwe-extension) have to be listed here (e.g.):  
```
const int ipwe_parameters[] = {
8700,	// outside temperature
8830	// DHW (warm water) temperature
};
```
  
- If you want to use **MAX! components**, you have to activate the belonging definement  
`#define MAX_CUL 192,168,178,5`  
and adjust the URL.  
The serial numbers of the MAX! thermostats have to be listed here (e.g.):  
``` 
const char max_device_list[] PROGMEM = {        // list of MAX! wall/heating thermostats that should be polled
  "KEQ0502326"                                  // use MAX! serial numbers here which have to have exactly 10 characters
  "KEQ0505080"
};
```
See [chapter 12.5](chap12.md#125-max-components) for further informations about MAX! components.  
  
- If you want to be able to **reset the Arduino by an URL command**, activate the belonging definement:  
`#define RESET`  
  
- **MAC address of the LAN shield:**  
If you find a MAC address printed on you LAN shield, insert it here:  
`static byte mac[] = { 0x00, 0x80, 0x41, 0x19, 0x69, 0x90 };`  
If you don't find a label with a specific MAC address (which often happens within cheap clones), just create and enter a valid and unused(!) address.  
  
- **Configuration of the adapter:**  
`BSB bus(19,18);`  
`constexpr uint8_t bus_type = 0;`

*Set the RX and TX pin* at which the adapter is connected to the Arduino and (optional) the addresses of the adapter and the destination.  
`BSB bus(19,18,parameter3,parameter4);`  
By default and if you are using the PCB of the adapter v3 with an Arduino Due, it's  
- RX pin = 19 (hardware serial)  
- TX pin = 18 (hardware serial)  
- Own bus address ("parameter3"): already set to 0x42 (BSB = 66 = "LAN"; LPB = segment address 4, device address 3) - usually there's no need to change that, see chapter [2](chap02.md) for further informations about addresses.  
If you want to define the adapter as a room unit, use "6" for room unit 1 and "7" for room unit 2.   
If you are using PPS, this optional third parameter set to "1" will enable writing to the heater - but only use this if there is no other room controller (such as QAA50/QAA70) active!  
- Bus address of the destination device ("parameter4"): already set to 0x00 (via BSB = connected controller; via LPB = segment address 0, device address 1) - usually there's no need to change that, see chapter [2](chap02.md) for further informations about addresses.   
  
*Set the type of bus system* which is used (where the adapter is connected to):  
`constexpr uint8_t bus_type = 0;`  
By default, BSB ("0") is set. If you want to use another bus system, use "1" for LPB or "2" for PPS.

*Note:*  
If you are using PPS, another definement should be adjusted:  
`#define QAA_TYPE  0x53`
where "0x53" imitates a QAA70 and "0x52" imitates a QAA50 room unit.  
  
- **Activate verbose mode:**  
By default, the verbose mode is activated (= 1), so that not only the 'raw' data (like the command ids) will be output to the serial monitor, but also the 'clear text' of the (known)  parameters and values. It's adviseable to leave this setting as it is, because it makes debugging easier. Besides that, it's necessary for decoding new telegrams and command ids, if you'll find parameters within your heating system which aren't implemented in BSB-LAN yet.  
`byte verbose = 1;`
  
- **Readonly or read/write access:**  
`#define DEFAULT_FLAG FL_RONLY`  
By default, the adpater/BSB-LAN is only allowed to read parameters (= flag `FL_RONLY`), so you can't change any settings or transmit any values to the controller. If you want to grant write access, you need to change the flag to "0" as it's shown here:  
`#define DEFAULT_FLAG 0`  
Now you are able to almost change any setting of the controller and you can transmit certain values (e.g. a room temperature).  
  
- **Including own code** from the file *BSB_lan_custom.h*:  
`#define CUSTOM_COMMANDS`  
Includes commands from the file `BSB_lan_custom.h` to be executed at the end of each main loop.  
  
- **Check for new versions when accessing BSB-LAN's main page:**  
`#define VERSION_CHECK 1`  
To have this function work, BSB-LAN needs internet access. If you don't want BSB-LAN to access the internet by it's own, deactivate the definement.  

- **Activate debugging via Telnet:**  
`#define DebugTelnet 1`  
If you activate this definement, the debug messages will be sent to a Telnet client instead of the serial port. For the regular usage it's adviseable to leave this definement deactivated though, so that you can use the local serial monitor (e.g. the one which is integrated in the ArduinoIDE).   

- **Activate "external" web server:**  
`#define WEBSERVER`  
If this definement is activated, BSB-LAN can act as a web server for static content. All files are / must be stored on SD card. Files can be placed in different directories. Only static content is supported.  
Supported file types are: html, htm, css, js, xml, txt, jpg, gif, svg, png, ico, gz.  
The web server supports static compression. If possible (if the client's browser supports gzip), it's always trying to deliver gzipped content (e.g. /d3d.js.gz for the URL /d3d.js).  
The web server supports the following headers: ETag, Last-Modified, Content-Length, Cache-Control.  
<!--*Note: If the web server finds a file named index.html when the user request / URL then it send index.html instead internal web server.-->
   
- **Select your heating system:**  
By default, BSB-LAN autodetects the specific controller of the connected heating system:  
`static const int fixed_device_family = 0;`  
`static const int fixed_device_variant = 0;`  
You can set `fixed_device_family` and `fixed_device_variant` to your device family and variant (query parameters 6225 and 6226 via BSB-LAN) if autodetect does not work or the heating system is not running when the Arduino is powered on.  
  


   
---  
   
[Further on to chapter 6](chap06.md)      
[Back to TOC](toc.md)   

 
