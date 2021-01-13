[Back to TOC](toc.md)  
[Back to chapter 4](chap04.md)    
   
---  

# 5. Configuration of the BSB-LAN Software v2.x   
    
## 5.1 Configuration via Webinterface  
  
*The description of this new function is still in progress.*  
  
## 5.2 Configuration by Adjusting the Settings Within *BSB_lan_config.h*  
  
The BSB-LAN software can be configured by adjusting the settings in the file *BSB_lan_config.h*. All settings are listed below in the same way as they are listed and preset in the file. It is therefore advisable to work through the settings point by point with this manual at hand.  

*Note:  
To 'activate' or a definement you have to delete the two slashes in front of the hashtag, to 'deactivate' a definement you have to add two slashes in front of the hashtag. E.g.:  
A deactivated definement: `//#define XYZ`  
An activated definement: `#define XYZ`*  
  
---  
  
-  The **language of the user interface** of the web interface of the adapter as well as the category and parameter designations must be selected or defined. For "English" the following definition must be selected:
   `#define LANG EN`  
   Starting with BSB-LAN v.042 it is possible to use BSB-LAN in other languages, too, whereby in principle any language can be supported (only' the corresponding translations have to be created).  
   Currently available are: Czech (CZ), German (DE), Danish (DK), English (EN), Spanish (ES), Finnish (FI), French (FR), Greek (GR), Hungarian (HU), Italian (IT), Dutch (NL), Polish (PL), Russian (RU), Swedish (SE), Slovenian (SI) and Turkish (TR). If certain expressions are not available in the specific language, the English expression is automatically displayed. If this is also not available, the German expression is finally displayed.
  
---
  
-  **Load configuration settings from EEPROM or from the file *BSB_lan_config.h*:**  
   `byte UseEEPROM = 1;`  
   According to the default setting, the configuration settings are read from the EEPROM when BSB-LAN is started. As a fallback the variable can be set to '0', then the settings are read from the file *BSB_lan_config.h*.
  
---  
  
***Network settings:***  
*Note: By default, the usage of DHCP is activated, so you don't have to change any network settings. If you want to use a fixed IP though, deactivate DHCP and set the IP and the addresses of the Gateway and the Subnet accordingly.*   
  
-  **MAC address of the ethernet shield:**  
   `byte mac[] = { 0x00, 0x80, 0x41, 0x19, 0x69, 0x90 };`  
   The default MAC address can be kept. A change is usually only necessary if more than one adapter is used (in any case, you should make sure that each MAC address only occurs *once* in the network!). In this case, changes should only be made to the last byte (e.g. 0x91, if a second adapter is used).  
   *Important note:*  
   *The MAC address assigned here also influences the host name (or is a part of it), which is assigned by the router when using DHCP (see below): The host name consists of the identifier "WIZnet" and the last three bytes of the MAC address.*  
   
   *For the default MAC address mentioned above, the host name is thus "WIZnet196990". This host name is usually also displayed as such in the router. In this case the web interface of BSB-LAN can be reached in the browser under `http://wiznet196990`.*  
   *If a second adapter is used and the MAC address will be changed to*  
   *`byte mac[] = { 0x00, 0x80, 0x41, 0x19, 0x69, 0x91 };`*  
   *the host name is "WIZnet196991" or `http://wiznet196991`.*

-  **Ethernet port:**  
   `uint16_t HTTPPort = 80;`  
   Port 80 for HTTP is preset.  
   
-  **DHCP:**  
   `bool useDHCP = true;`  
   By default DHCP is used. If this is not desired and you want to assign a fixed IP address by yourself, set the variable to *false*.  
   
   *Important note:*  
   *Please see the notes above regarding the hostname based on the MAC address. The IP given by the router will also appear within the start process of the Arduino Due within the serial monitor of the Arduino IDE.*  
  
-  **IP address:**  
   `byte ip_addr[4] = {192,168,178,88};`  
   Fixed IP address of the adapter, if DHCP is not used - *please note the commas instead of dots!*  
   *Note: If you want to give the adapter a fixed IP, please make sure that it occurs only once in your network!*  
   
-  **Gateway address:**  
   `byte gateway_addr[4] = {192,168,178,1};`  
   IP address of the gateway (usually the one of the router itself) - *please note the commas instead of dots!*  
   
-  **Subnet:**  
   `byte subnet_addr[4] = {255,255,255,0};`  
   Address of the subnet - *please note the commas instead of dots!*  
   
---    
   
-   **WiFi by additional ESP8266:**  
    `//#define WIFI`  
    This definement has to be activated if the WiFi function of the [ESP8266 solution](chap12.md#1273-wlan-usage-of-an-additional-esp8266) should be used.  
    
    `char wifi_ssid[32] = "YourWiFiNetwork";` 
    For the usage of WiFi, *YourWiFiNetwork* has to be replaced by the SSID of the WiFi network.  
    
    `char wifi_pass[64] = "YourWiFiPassword";`  
    For the usage of WiFi, *YourWiFiPassword* has to be replaced by the password of the WiFi network.  
    
    `#define WIFI_SPI_SS_PIN 13`  
    The SS pin to be used at the DUE is defined here. It is advisable to leave the default setting. If, however, another pin should be used, it is essential to ensure that the desired pin is neither used elsewhere nor is included in the list of protected pins.  
      
---
   
-  **Using Multicast DNS:**  
   `#define MDNS_HOSTNAME "BSB-LAN"`  
   By default the usage of Multicast DNS with the hostname "BSB-LAN" is activated, so that you can find the adaptersetup under this name within your network.  
   Please note: mDNS is only available when using LAN, it is not available if you are using the [WiFi solution using an ESP8266](chap12.md#1273-wlan-usage-of-an-additional-esp8266)!  
   
---
  
-  **Debugging and related settings:**  
   - `#define DEBUG` → the debug module will be compiled (activated by default)    
   
   - `byte debug_mode = 1;` → The following debug options are available:  
   0 - debugging deactivated  
   1 - send debug messages to the serial interface (e.g. for using the aerial monitor of the Arduino IDE); default setting  
   2 - send debug messages to a TelNet client instead of the serial interface  
   
   - `byte verbose = 1;` → By default the verbose mode is activated (= 1), so that (besides the raw data) the respective plaintext (if available) of parameters and values is displayed. It is advisable to leave this setting as it facilitates possible trouble shooting. Furthermore, this setting is necessary if telegrams and command IDs of new parameters should be decoded.  
   
   - `byte monitor = 0;` → Bus monitor mode, deactivated by default; set to '1' to activate  
   
   - `bool show_unknown = true;` → All parameters including the *unknown parameters* (error message "error 7 (parameter not supported)") are displayed when querying via web interface (e.g. when querying a complete category); default setting.  
    If you want to hide the 'unknown' parameters that are not supported by the controller of your heating system (e.g. when querying a complete category), you have to set the variable to 'false' (`bool show_unknown = false;`). *The parameters are still queried in such a query (e.g. for a complete category) though.*  
    
---
  
***Security functions:***  
There are several options to control and protect access to your heating system. However, keep in mind, that even activating all three options are no guarantee that a versatile intruder with access to your (W)LAN won't be able to gain access. In any case, no encryption of data streams is provided by the Arduino itself. Use VPN or a SSL proxy if that is a must for you and connect the Arduino wired to the VPN server or SSL proxy.  
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
   `bool enableOneWireBus = true;`  
   `byte One_Wire_Pin = 7;`  
   
   If you want to use OneWire temperature sensors (DS18B20), the definition must be activated, the variable must be set to *true*  and the corresponding pin (DATA connection of the sensor on the adapter board / Arduino Due) must be defined. *Note: Make sure that you don't use any of the protected pins which are listed further down below!*   
    By default, the module is activated and pin 7 for DATA is set.  
    
    If you don't want to use DS18B20 sensors, the variable must be set to false:  
    `bool enableOneWireBus = false;`  
    
-  **DHT22 sensors:**  
   `#define DHT_BUS`  
   `byte DHT_Pins[10] = {2, 3};`  
   
   If you want to use DHT22 sensors (temperature & humidity), the definement must be activated and the corresponding pin(s) must be be defined. *Note: Make sure that you don't use any of the protected pins which are listed further down below!*     
   By default, the module is activated and the pins 2 & 3 are set for the DATA of two sensors (in summary - each sensor has to be connected to a different pin).     
  
---
  
-  **24h averages:**  
   `#define AVERAGES`  
   If you want to create 24h averages from certain parameters, the definement must be activated (default setting). 
   
   `bool logAverageValues = false;`
   If you want the averages to be logged to a microSD card within the file *averages.txt*, you need to change the default setting and change the variable to `true`. If you don't want to have these values logged, the variable must be set to `false` as per default.  
   
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
  
  - `bool logCurrentValues = false;`  
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
        
-  **MQTT:**  
   If you want to use MQTT the belonging varaibles and settings have to be adjusted:    

   - `define MQTT` → The MQTT module will be compiled (default setting).  
    
   - `byte mqtt_mode = 0;` → MQTT is deactivated (default setting); the following options are available:  
   1 = send messages in plain text format  
   2 = send messages in JSON format. Use this if you want a json package of your logging information printed to the mqtt topic  
       Structure of the JSON payload: {"MQTTDeviceID": {"status":{"log_param1":"value1","log_param2":"value2"}, ...}}  
   3 = send messages in rich JSON format. Use this if you want a json package of your logging information printed to the mqtt topic  
       Structure of the rich JSON payload: {"MQTTDeviceID": {"id": one_of_logvalues, "name": "program_name_from_logvalues", "value": "query_result", "desc": "enum value description", "unit": "unit of measurement", "error", error_code}}  
    
   - `byte mqtt_broker_ip_addr[4] = {192,168,1,20};` → IP of the MQTT broker (standard port 1883). *Please note the commas insted of dots!*    
        
   - `char MQTTUsername[32] = "User";` → Set username for MQTT broker here or set zero-length string if no username/password is used.   
    
   - `char MQTTPassword[32] = "Pass";` → Set password for MQTT broker here or set zero-length string if no password is used.   
    
   - `char MQTTTopicPrefix[32] = "BSB-LAN";` → Optional: Choose the "topic" for MQTT messages here. If zero-length string here, default topic name used.     
    
   - `char MQTTDeviceID[32] = "MyHeater";` → Optional: Define a device name to use as header in json payload. If zero-length string here, "BSB-LAN" will be used.  
    
    ***Note:***   
    *The parameters that should be queried and the interval for sending the values must be defined within the logger definement as mentioned above.*   
         
---   
      
-  **IPWE:**  
   `#define IPWE` → The ipwe module will be compiled.    
   `bool enable_ipwe = false;`  
   By default, the usage of the ipwe extension (URL/ipwe.cgi) is deactivated. If you want to use it, set the variable to 'true'.       
   Define the parameters that should be displayed (max 40):  
   ```  
   int ipwe_parameters[40] = {  
   8700,	// outside temperature
   8830	// DHW (warm water) temperature 
   };  
   ```
  
---  
   
-  **MAX! (CUNO/CUNX/modified MAX!Cube):**  
   If you want to use MAX! thermostats, adjust the following settings:  
    
   - `//#define MAX_CUL` → activate the definement (deactivated by default)  
     
   - `bool enable_max_cul = false;` → set the variable to 'true' (default value: 'false')  
     
   - `byte max_cul_ip_addr[4] = {192,168,178,5};` → Set the IP address of the CUNO/CUNX/modified MAX!Cube - *please note the commas instead of dots!*  
     
    - Define the MAX! thermostats that should be queried (max 20) by entering the 10 digit serial number / MAX! ID:  
    ```
    char max_device_list[20][11] = {   
    "KEQ0502326",  
    "KEQ0505080"
    };
    ```  
    
   See [chapter 12.5](chap12.md#125-max-components) for further informations about MAX! components.
    
---
  
-  Define the number of retries for the query command (default value is 3, doesn't need to be changed usually):  
   #define QUERY_RETRIES  3  
   
---
   
***Settings of the bus pins and bus type:***   
   
-  **RX/TX pinconfiguration:**  
   `byte bus_pins[2] = {0,0};` → automatic detection and selection of the used pins (RX,TX); possible options:  
   - Hardware serial (since adapter v3 & Arduino Due): RX = 19, TX = 18 (`{19,18}`)  
   - Software serial (up to adapter v2 & Arduino Mega 2560): RX = 68, TX = 69 (`{68,69}`)  
   
-  **Bus type / protocol:**  
   `uint8_t bus_type = 0;` → Depending on the connection of the adapter to the controller of your heating system (BSB/LPB/PPS), the corresponding bus type must be set (default value is 0 = BSB). Possible options:  
   0 = BSB  
   1 = LPB  
   2 = PPS  
   
-  **Bus settings:**  
   Depending on the bus type, you can/must adjust certain settings:   
   
   -  **BSB:**  
      `byte own_address = 0x42;` → sets own address of the BSB-LAN adapter; default setting is '0x42' = 66, which is 'LAN' in serial monitor  
      `byte dest_address = 0x00;` → destination address of the heating system; preset: 0
      See [chap. 2.1.1](chap02.html#211-addressing-within-the-bsb) for further informations.   

   -  **LPB:**  
      `byte own_address = 0x42;` → own address of the BSB-LAN adapter; preset: segment 4, device 3  
      `byte dest_address = 0x00;` → destination address of the heating system; preset: segment 0, device 1  
      See [chap. 2.1.2](chap02.html#212-addressing-within-the-lpb) for further informations.  
 
   -   **PPS:**  
      `bool pps_write = 0;` → Readonly access (default setting); if you want to enable writing to the controller of the heating system, set the variable to '1'. *Note: Only enable writing if there is no other 'real' room unit such as QAA50/QAA70!*  
      `byte QAA_TYPE = 0x53;` → type of the room unit which should be imitated; 0x53 = QAA70 (default setting), 0x52 = QAA50  

---
  
-  **Protected GPIO pins:**  
   Usually there is no need to change these settings if the standard configuration of the BSB-LAN ahrdware is used. However, if you can adjust these settings though, please refer to the listing within the file *BSB_lan_config.h*.  
   
--- 
  
-  **Detection or fixed setting of the controller type of the heating system:**  
   `static const int fixed_device_family = 0;`  
   `static const int fixed_device_variant = 0;`  
   By default, the automatic detection of the controller type is active. Usually there is no need to change this setting. However, you can set the type manually though, but you should *only* change this if you *really* know what you are doing! In that case set the variables of `fixed_device_family` and `fixed_device_variant` to your device family and variant (parameters 6225 and 6226).  
  
---
  
-  **Read/write access to the controller:**  
   `#define DEFAULT_FLAG FL_SW_CTL_RONLY`  
   By default, only read-access to the controller of the heating system is granted for the BSB-LAN adapter. If you want to make all parameters writeable / settable, then you can adjust this setting within the webinterface of BSB-LAN (menu "settings").  
   *Note for Mega-user:*  
   The possibility to configure BSB-LAN via the webinterface doesn't exist within the usage of the Mega 2560, because the module WEBCONFIG can't be compiled and used due to the limited memory of the Mega. In this case you still have to grant write access by setting the flag '0': `#define DEFAULT_FLAG 0`
   
---   
   
-  **Include own code:**  
   `//#define CUSTOM_COMMANDS`  
   This includes commands from the file *BSB_lan_custom.h* to be executed at the end of each main loop (deactivated by default).  
   
---
   
-  **Check for Updates of BSB-LAN:**  
   `#define VERSION_CHECK`  
   `bool enable_version_check = false;`  
   Check for new versions when accessing BSB-LAN's main page (internet access needed). Doing so will poll the most recent version number from the BSB-LAN server. This function is deactivated by default; to activate this function, set the variable to 'true'.  
   
   *Note: In this process, it is unavoidable that your IP address will be transferred to the server, obviously. We nevertheless mention this here because this constitutes as 'personal data' and this feature is therefore disabled by default. Activating this feature means you are consenting to transmitting your IP address to the BSB-LAN server where it will be stored for up to two weeks in the server's log files to allow for technical as well as abuse analaysis. No other data (such as anything related to your heating system) is transmitted in this process, as you can see in the source code.*  
   
---  
   
-  **"External" webserver:**  
   `//#define WEBSERVER`  
   Usage of the "external" web server if definement is active. Please see [chapter 8.2.10](chap08.html#8210-using-the-webserver-function) for further informations.  
   
---   
   
-  **Store configuration in EEPROM:**  
   `#define CONFIG_IN_EEPROM`  
   Stores the configuration in the EEPROM. If you don't want to use this function, deactivate the definement.  
   
---
   
-  **Compile web-based configuration and EEPROM config store module extension:**  
   `#define WEBCONFIG`  
   Activates the configuration via webinterface.    
   
---  
  
-  **Compile JSON-based configuration and EEPROM config store module extension.**  
   `#define JSONCONFIG`  
   
---   
   
-  **Variables for future use, no function yet (November 2020):**  

   `#define ROOM_UNIT` → compile room unit replacement extension   
   `byte UdpIP[4] = {0,0,0,0};` → destination IP address for sending UDP packets to  
   `uint16_t UdpDelay = 15;` → interval in seconds to send UDP packets 
     
   `#define OFF_SITE_LOGGER` → compile off-site logger extension  
   `byte destinationServer[128] = "";` → URL string to periodically send values to an off-site logger  
   `uint16_t destinationPort = 80;` → port number for abovementioned server  
   `uint32_t destinationDelay = 84600;` → interval in seconds to send values  
   
---  

***For users of the outdated setup based on an Arduino Mega 2560:***  
*Due to the lack of memory of the Mega 2560 it is neccessary to deactivate certain modules (or functions). Please also see [appendix d](appendix_d.md) for further informations.*  
  
-  **Disabling functions:**  
  
   If you use CONFIG_IN_EEPROM and WEBCONFIG modules then you can enable I_DO_NOT_WANT_URL_CONFIG for saving flash memory (~1.2Kb). This will disable configuration through URL commands (/A, /L, /P).  
   `#define I_DO_NOT_WANT_URL_CONFIG`  
  
   Enable I_WILL_USE_EXTERNAL_INTERFACE for saving flash memory (~6,8Kb). /DG command will be disabled.  
   `#define I_WILL_USE_EXTERNAL_INTERFACE`  
  
   Enabling I_DO_NOT_NEED_NATIVE_WEB_INTERFACE will eliminate native web interface and save up to 13 Kb of flash memory. /N[E] and /Q command still work. You can use this if you are using third-party software for BSB-LAN management. Do not forget to enable other required modules (JSONCONFIG, MQTT, WEBSERVER).  
   `#define I_DO_NOT_NEED_NATIVE_WEB_INTERFACE`  
     
-  **Disabling modules:**  
  
   If you want to try this version of BSB-LAN to run on an Arduino Mega 2560, you can change the modules which should be compiled so that they fit your needs and the Mega's memory constraints.  
   *Note: This overwrites any definements above.*  
   ```
   #if defined(__AVR__)  
   //#undef CONFIG_IN_EEPROM  
   //#undef WEBCONFIG  
   //#undef WEBSERVER  
   #undef AVERAGES  
   #undef DEBUG  
   #undef IPWE  
   #undef MQTT  
   #undef MDNS_HOSTNAME
   #undef OFF_SITE_LOGGER  
   #undef ROOM_UNIT  
   #undef VERSION_CHECK  
   #undef MAX_CUL  
   #endif  
   ```
   
---  
   
[Further on to chapter 6](chap06.md)      
[Back to TOC](toc.md)   

 
