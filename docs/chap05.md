[Back to TOC](toc.md)  
[Back to chapter 4](chap04.md)    
   
---  

# 5. Relevant Parameter Settings of the BSB-LAN Software   
*Note:  
To 'uncomment' a definement for making it active means to delete the two slashes in front of the hashtag. To 'comment out' a definement means to deactivate it by adding two slashes in front of the hashtag. E.g.:  
A deactivated definement: `//#define XYZ`  
An activated definement: `#define XYZ`*  
  
**The following functions and definements can or should be set individually (not every definement needs to be adjusted though!) within the file** ***BSB_lan_config.h***  
   
- **Ethernetport:**  
Define the port you want to use for BSB-LAN (port 80 is default for HTTP and doesn't need to be changed for the regular usage.)  
`#define Port 80`  
  
- **IP address of the adapter:**  
Define the IP address you want to give the adapter. It's adviseable to give the adapter a fix address (which has to be free and unused by your router!), so that you can setup your home automation system correct. In case that you want DHCP, deactivate the definement.  
`#define IPAddr 192,168,178,88`  
Please note the commas instead of dots!  
  
- **GatewayIP:**  
Define the IP address of your router (optional).  
`#define GatewayIP 192,168,178,1`  
Please note the commas instead of dots!  
  
- **DNSIP:**  
Here you can set an alternative IP, if your DNS server is different than the IP of your router (GatewayIP).  
`#define DNSIP 192,168,178,1`  
Please note the commas instead of dots!  
  
- **SubnetIP:**  
Here you can add the IP of an alternative (non-standard) gateway.  
`#define SubnetIP 255,255,255,0`  
Please use commas insteaf of dots!  
  
- Choose the **language of the webinterface** including the names of parameters, categories and so on. For "English" you have to choose the following:  
`#define LANG EN`  
Available languages are: Czech (CZ), German (DE), Danish (DK), English (EN), Spanish (ES), Finnish (FI), French (FR), Greek (EL), Hungarian (HU), Italian (IT), Dutch (NL), Polish (PL), Russian (RU), Swedish (SE), Slovenian (SI) and Turkish (TR).  
Note:  
So far the German language is the most complete one, followed by English. Other incomplete languages will automatically be filled up with English translations first, and if no English translation is available, fallback will take place to German. If you are a native speaker of one of the listed languages and you want to support BSB-LAN with some translations, please feel free to contact us!    
   
- **Security options:**  
There are several options to control and protect access to your heating system. However, keep in mind, that even activating all three options are no guarantee that a versatile intruder with access to your (W)LAN won't be able to gain access. In any case, no encryption of data streams is provided from the Arduino itself. Use VPN or a SSL proxy if that is a must for you and connect the Arduino wired to the VPN server or SSL proxy.  
The following three security options are available within BSB-LAN:  
  
- *Passkey function:*  
Add a certain sequence of alphanumerical characters as a simple security function.  
`#define PASSKEY "1234"`  
Note: If PASSKEY is defined, the URL has to contain the defined passkey as first element, e.g.:
`http://192.168.178.88/1234/` to view the main website (don't forget the trailing slash!)
`http://192.168.178.88/1234/K` to list all categories
`http://192.168.178.88/1234/8700/8740/8741` to list parameters 8700, 8740 and 8741 in one request
Only within the URL of the optional [IPWE extension](chap08.md#826-ipwe-cgi) the passkey has NOT to be added!  
   
- *IP-address-based access:*  
Only the last segment of the client's IP address is matched, as it is assumed that requests are made from the same subnet only. E.g.: if your trusted client's IP is 192.168.178.20, you have to set `TRUSTED_IP` to 20. You can add two IPs for this security function.  
`#define TRUSTED_IP 20`  
`#define TRUSTED_IP2 30`  

- *HTTP-Auth authentification:*  
Provides a (base64-coded) username/password based access. No encryption!  
`#define USER_PASS_B64 "YXRhcmk6ODAweGw="`  
Default sets username to "atari" and password to "800xl". Visit a website like https://www.base64encode.org/ to encode your own username/password combination in the format username:password and replace the `YXRhcmk6ODAweGw=` string accordingly.  
  
- **Select your heating system:**  
By default, BSB-LAN autodetects the specific controller of the connected heating system:  
`static const int fixed_device_family = 0;`  
`static const int fixed_device_variant = 0;`  
You can set `fixed_device_family` and `fixed_device_variant` to your device family and variant (query parameters 6225 and 6226 via BSB-LAN) if autodetect does not work or the heating system is not running when the Arduino is powered on.  
  
- **Hide unknown parameters** from the output of the webinterface:  
`#define HIDE_UNKNOWN`  
Note: The parameters will still be queried though!  
  
- Define the **pins for optional DS18B20 sensors**:  
`#define ONE_WIRE_BUS <n>`  
Enter the number of the pin (=`<n>`) you want to use for connecting [DS18B20](chap12.md#1232-notes-on-ds18b20-temperature-sensors) sensors. Make sure that you don't use any of the protected pins which are listed further down below in the file *BSB_lan_config.h*!

- Define the **pins for optional DHT22 sensors**:  
`#define DHT_BUS <n>`  
Enter the number of the pin (=`<n>`) you want to use for connecting [DHT22](chap12.md#1231-notes-on-dht22-temperature-humidity-sensors) sensors. Make sure that you don't use any of the protected pins which are listed further down below in the file *BSB_lan_config.h*!  
  

- If you want to create **24h averages** from certain parameters, you have to list the numbers of the parameters. E.g.:  
```
int avg_parameters[20] = {
8700,	// outside temperature
8830	// DHW (warm water) temperature
};
```
  
- If you want to **log certain values/parameters to a microSD card**, you have to  
a) activate the definement   
`#define LOGGER` and  
b) list the numbers of the desired parameters, e.g.:   
```
int log_parameters[20] = {
8700,	// outside temperature
8830	// DHW (warm water) temperature
};
```
  
- You can set the **logging interval** within  
`unsigned long log_interval = 3600;`  
with the logging interval in seconds.

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
   
---  
   
[Further on to chapter 6](chap06.md)      
[Back to TOC](toc.md)   

 
