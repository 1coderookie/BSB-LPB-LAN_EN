[Back to TOC](toc.md)  
[Back to chapter 12](chap12.md)    
   
---      
    
# 13. Possible Error Messages and their Causes
    
---
    
## 13.1 Error Message „unknown type \<xxxxxxxx\>"
This error states that there are no conversion instructions for this parameter is present to convert the raw data in a corresponding
unit (time, temperature, percent, pressure, etc.).  
   
To solve this problem, the respective telegram / command
ID of the relevant parameter and the associated value should be read out and reported. Should there be multiple setting options for one
parameters available, each option must also be read out,
so that a clear assignment can take place.  
    
---
    

## 13.2 Error Message „error 7 (parameter not supported)"
The associated Command ID is not recognized or the corresponding
parameter is not supported by the controller (e.g. specific
parameters related to a gas fired heater are not available at an oil fired heater).  
   
Error messages of this type are hidden by default since v0.41 (but will still be queried within a complete query for example). If you still want them to be displayed, you have to comment out the definement `#define
HIDE_UNKNOWN` in the file *BSB\_lan\_config.h* (so that it looks like `//#define HIDE_UNKNOWN`).  
   
To check whether the Command ID in principle is supported by the controller but not yet released for your specific device family, 
please execute the URL command /Q (also see [chapter 8.2.5](chap08.md#825-checking-for-non-released-controller-specific-command-ids)). If any 'error 7'-messages appear with this query, please report them with the complete output of /Q.    
    
---
    

## 13.3 Error Message „query failed"
This message appears when no response from the controller comes upon the request of the adapter.  
   
Possible causes are mostly to be found on the hardware side (e. g. faulty 
RX and/or TX connection, wrongly installed components or even a timeout due to a switched off or not connected controller).  
    
---
    

## 13.4 Error Message „ERROR: set failed! - parameter is readonly"
This message appears, when you are trying to adjust settings or when you are trying to send (e. g.) values like room temperature via BSB-LAN but didn't change the preset read-only state of BSB-LAN.  
   
To change this setting within BSB-LAN, you have two different options:  
1. You can allow BSB-LAN to be able to change all possible parameters which could be set. It's the most comfortable setting, because in this case you don't have to think about which parameters exactly you maybe want to be able to change one day. Besides that, it's easy to achieve: just open the file *BSB_lan_config.h* and change the flag in the definement `#define DEFAULT_FLAG FL_RONLY` to `#define DEFAULT_FLAG 0` (located at the bottom of the page), flash the Ardunio again and you are able to change settings of the controller via BSB-LAN.  
2. You can leave the mentioned flag preset as `FL_RONLY` and make only the specific desired parameters writeable. Therefore you have to make specific changes in the file *BSB_lan_defs.h* for each parameter which you want to be able to change the setting of. Please read [chapter 5](chap5.md) for further informations about the procedure.  
     
---  

[Further on to chapter 14](chap14.md)      
[Back to TOC](toc.md)   

