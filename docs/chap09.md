[Back to TOC](toc.md)  
[Back to chapter 8](chap08.md)    
   
---      
    

    
# 9. Logging Data 
    
---
    
## 9.1 Usage of the Adapter as a Standalone Logger with BSB-LAN
Insert a FAT32-formatted microSD card into the
memory card slot of the ethernet shield before powering up the Arduino.  
          
Before flashing, activate the definement `#define LOGGER` in the file *BSB\_lan\_config.h*, add the parameters to be logged to the variable
`log_parameters` and determine the log interval with the variable
`log_interval`. Please also note the corresponding points in [chapter 8.1](chap08.md#81-listing-and-description-of-the-url-commands).  
Later, during the runtime, both the interval and the logging parameters can be changed by using the command `"/L=[Interval],[Parameter1],...,[Parameter20]"`.  
   
All data is stored within the file *datalog.txt* on the card in csv format. Thus the data can easily be imported in Excel or OpenOffice
Calc.  
   
The file contents can be viewed with the URL command `/D`, a
graphical representation of the log files is done by `/DG`.  
   
To delete and rebuild the file *datalog.txt*, use the
URL command `/D0`.  
    
**The URL command `/D0` should also be executed on first use! 
This will initiate the file with the appropriate CSV header.**  
    
***Notes:***  
       
*Occasionally it may happen that certain microSD cards are not
easily recognized by the LAN shield. Should this
Problem occur, the usage of cards with memory sizes
from 1GB, 2GB to max. 4GB is recommended. Should these cards also be problematic, try formatting it as FAT16.*  
     
*Please note that the Arduino is not an exact clock. Even if the interval has been set up to e.g. 60 seconds, the time displayed in the file (which is received by the heating control) possibly will differ - this can take up to a second per minute.  
If an exact log time is absolutely necessary, you can measure the average time difference between the Arduino time and the real time and adjust the log interval accordingly (e.g. set 59 seconds instead of 60 seconds).*
       
---
    
## 9.2 Usage of the Adapter as a Remote Logger
*Sorry, not yet translated.. :(*     

In addition to the use of complex systems such as FHEM and the specific
logging solutions, you can e.g. execute the following command periodically (for example via cron job):  
    
```
DATE=`date +%Y%m%d%H%M%S`; wget -qO- http://192.168.178.88/8310/720/710 | egrep "(8310|720|710)" | sed "s/^/$DATE /" >> log.txt  
```
    
The log file \'* log.txt *\' resulting from this example contains the
recorded values of parameters 8310, 720 and 710.  
Later you can sort the log file based on the parameter numbers, use the command \'sort\' for this:  
   
`sort -k2 log.txt`  
    
***Note:***  
*Of course the IP, optionally activated optional security features, the desired parameters etc. have to be adjusted in the above example.*  
    
       
---  
   
[Further on to chapter 10](chap10.md)      
[Back to TOC](toc.md)   



