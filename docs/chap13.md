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
HIDE_UNKNOWN` in the file *BSB\_lan\_config.h* (so that it looks like `// # define HIDE_UNKNOWN`).  
   
To check whether the Command ID in principle is supported by the controller but not yet released for your specific device family, 
please execute the URL command /Q (also see [chapter 8.2.5](chap08.md#825-checking-for-non-released-controller-specific-command-ids)). If any 'error 7'-messages appear with this query, please report them with the complete output of /Q.    
    
---
    

## 13.3 Error Message „query failed"
This message appears when no response from the controller comes upon the request of the adapter.  
   
Possible causes are mostly to be found on the hardware side (e.g. faulty 
RX and/or TX connection, wrongly installed components or even a timeout due to a switched off or not connected controller).  
    
---
    

## 13.4 Fehlermeldung „FEHLER: Setzen fehlgeschlagen! - Parameter ist nur lesbar"
*Sorry, not yet translated.. :(*  

Diese Meldung erscheint bei dem Versuch, Werte zu schreiben bzw. zu
übermitteln (bspw. die Raumtemperatur) oder Parameter zu verändern,
während der Zugriff des Adapters nur auf Lesen beschränkt ist
(`FL_RONLY`).  
Der gewünschte Parameter (oder auch generell alle Parameter) muss in
diesem Fall als schreibbar definiert werden. Die hierfür notwendige
Vorgehensweise ist in Kap. [5](chap05.md) (vorletzter Punkt) ausführlich beschrieben.
     
---  

[Further on to chapter 14](chap14.md)      
[Back to TOC](toc.md)   

