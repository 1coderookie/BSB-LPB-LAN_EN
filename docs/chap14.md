[Back to TOC](toc.md)  
[Back to chapter 13](chap13.md)    
   
---      
    
# 14. Possible Error Messages and their Causes
    
---
    
## 14.1 Error Message "unknown type \<xxxxxxxx\>"
This error states that there are no conversion instructions for this parameter is present to convert the raw data in a corresponding unit (time, temperature, percent, pressure, etc.).  
   
To solve this problem, the respective telegram / command ID of the relevant parameter and the associated value should be read out and reported. Should there be multiple setting options for one parameters available, each option must also be read out, so that a clear assignment can take place. See [chap. 9](chap09.md) for further instructions.   
    
---
    

## 14.2 Error Message "error 7 (parameter not supported)"
The associated Command ID is not recognized or the corresponding parameter is not supported by the controller (e.g. specific parameters related to a gas fired heater are not available at an oil fired heater).  
   
Error messages of this type are hidden by default since v0.41 (but will still be queried within a complete query for example). If you still want them to be displayed, you have to comment out the definement `#define
HIDE_UNKNOWN` in the file *BSB\_lan\_config.h* (so that it looks like `//#define HIDE_UNKNOWN`).  
   
To check whether the Command ID in principle is supported by the controller but not yet released for your specific device family, 
please execute the URL command /Q (also see [chapter 8.2.5](chap08.md#825-checking-for-non-released-controller-specific-command-ids)). If any 'error 7'-messages appear with this query, please report them with the complete output of /Q.    
    
---
    

## 14.3 Error Message "query failed"
This message appears when no response from the controller comes upon the request of the adapter.  
   
Possible causes are mostly to be found on the hardware side (e. g. faulty 
RX and/or TX connection, wrongly installed components or even a timeout due to a switched off or not connected controller).  
    
---
   
## 14.4 Error Message "ERROR: set failed! - parameter is readonly"
This message appears, when you are trying to adjust settings or when you are trying to send (e. g.) values like room temperature via BSB-LAN but didn't change the preset read-only state of BSB-LAN.  
   
You have to grant write access to BSB-LAN.    
     
---  
        
## 14.5 Error Message "decoding error"  
  
The error message "decoding error" means, that the parameter and the command id are known or match, but that the data packet doesn't correspond to the known decoding. The reason for this could be a different length or a different unit.  
  
To update this for the specific type of controller / heating system, the belonging data packet, the exact value and the specific unit is needed. Please see [chap. 9](chap09.md) for further instructions.  
      
---

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/U6U5NPB51)    

---

[Further on to chapter 15](chap15.md)      
[Back to TOC](toc.md)   

