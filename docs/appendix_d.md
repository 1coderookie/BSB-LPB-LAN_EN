# Appendix D: Notes For Users Of The Outdated Setup Adapter v2 + Mega 2560

For users of the outdated setup, the following refers to some questions and points that need clarification or that need to 
be considered. If further questions occur, please post your questions in the [corresponding thread of the german FHEM forum](https://forum.fhem.de/index.php/topic,29762.0.html).  
Please understand, however, that we will not answer any questions that may arise, for example, about building an adapter v2 after the changeover to adapter v3.  
**PCBs v2 are no longer available, state of the art is the combination adapter v3/v4 + Due.**  
  
- ***Do I have to switch to the new setup adapter v3/v4 + Due?***  
No, if you are satisfied with the outdated setup and the range of functions of BSB-LAN has met your requirements so far, 
you can of course continue to use the old setup.  
  
  **In this case [BSB-LAN version v0.44](https://github.com/fredlcore/bsb_lan/releases/tag/v0.44) is the last 'officially' 
tested and recommended version for your setup! Within the zip-file there you'll also find the last 'Mega2560-specific' version of the manual (PDF).**  
  
  It has been shown by several users that even the **[v1.1](https://github.com/fredlcore/bsb_lan/releases/tag/v1.1)** still runs without major restrictions, but due to the lack of memory of the Mega 2560 probably already no longer with all available options that BSB-LAN offers.  
  
  Starting from **v2.x** it is then definitely necessary to deactivate some modules which BSB-LAN offers. Hints concerning this can be found in [chap. 5.2](chap05.md#52-configuration-by-adjusting-the-settings-within-bsb_lan_configh) or in the comments of the file *BSB_lan_config.h*. Special attention should be paid to the last points, which offer a comfortable deactivation of individual modules (e.g. Webconfig, MQTT, IPWE etc.) at central place. 
   
  Besides disabling specific modules, there another way to save memory as of *v2.x*:  
There is a perlscript named *selected_defs.pl* and a Windows executable named *selected_defs.exe* in the repo that filters the file *BSB_lan_defs.h* for selected device families and creates a specific file for your own controller type. The saving is on average about 20 to 25 kB of flash memory, which can then be used for the (re-)activation of other functions. In case of a controller change (= other device family) the file must of course be regenerated accordingly.  
  This additional step is not very convenient, but may allow users of the Mega to use the software on the old hardware for a while longer.  
  The script runs under Perl, which is installed by default on Mac and Linux computers, but can also be installed on Windows.
   
  Procedure for creating a controller-specific defs file:  
  - Retrieve parameter 6225 "Device family" via BSB-LAN and note the value.  
  - Before executing, copy the file *selected_defs.pl* respectingly *selected_defs.exe* in the same folder, where the file *BSB_lan_defs.h* is located.   
  - Open a terminal, enter the corresponding folder and create the reduced file named *BSB_lan_defs_filtered.h* using the Perl script or the Windows executable, which contains only the parameters relevant for the specific device family/families. If only one controller is connected, for example with device family 162, the command is  
  `./selected_defs.pl 162 > BSB_lan_defs_filtered.h` respectingly  
  `./selected_defs.exe 162 > BSB_lan_defs_filtered.h`.  
  If you have e.g. two devices on the bus with the device families 162 and 90, you can extend the command by the second value:  
  `./selected_defs.pl 162 90 > BSB_lan_defs_filtered.h` respectively  
  `./selected_defs.exe 162 90 > BSB_lan_defs_filtered.h`.    
  - Move the original file *BSB_lan_defs.h* from the "BSB_lan" directory to a different location. Move the newly created file *BSB_lan_defs_filtered.h* to the directory "BSB_lan" (if you didn't already create the file within that directory).  
  - *Important: Now rename the newly created file to "BSB_lan_defs.h"*!  
  
  ***Further notes:***  
  *If you still want to test a newer version than v0.44 on the Mega, make sure that you use the corresponding file BSB_lan_config.h.default and adjust it accordingly:*    
  - With a version of BSB-LAN **before v2.x** it is absolutely necessary to adapt the line `BSB bus(19,18);`: The DUE uses (in contrast to the Mega) the HardwareSerial interface and other RX/TX pins than the Mega, which is already preset here. When used with the Mega, the line must therefore be changed to `BSB bus(68,69);`!  
  - With a version of BSB-LAN **since v2.x** an automatic detection of the used pins is implemented. Therefore BSB-LAN autodetects if a Mega (= software serial) or a Due (= hardware serial) is used.    
  
- ***Can I continue to use the Adapter v2 on a Due?***  
No! The reason for this is that neither the adapter v2 nor the Due has an EEPROM, which is necessary for BSB-LAN.
So if you want to benefit from the new functions of BSB-LAN in the future, you have to get an adapter v3/v4 or make it yourself 
and use it with an Arduino Due.  
  
- ***Can I 'convert' the adapter v2 to an adapter v3/v4?***  
No! The primary reason for this is (among other reasons) again the missing EEPROM of the Due.  
  
- ***Can I continue to use the adapter v3/v4 with my previous Mega 2560?***  
No! Even if it would be possible after some changes to the adapter v3/v4, it would not offer any added value compared to the 
adapter v2. New functions of BSB-LAN would still not be able to be used due to the lack of memory of the Mega 2560. 
So if you want to use the new adapter v3/v4, then only in combination with an Arduino Due.  
  
- ***Why is there an EEPROM on the v3/v4 board?***  
The Arduino Due has no EEPROM, but this is necessary for BSB-LAN.  
  
- ***Can I continue to use the LAN-Shield if I change to the Due?***  
Yes, usually this is possible without any problems. A trouble-free usage of clones can't be guaranteed though.  
  
- ***Can I continue to use my existing housing?***  
Yes, the Due has the same form factor as the Mega 2560, so the dimensions of the case should fit. However, you will probably 
have to adapt your case a bit and add a cutout or a large hole for the middle USB port of the Due ('Programming Port'), 
so that you can continue to connect the corresponding USB cable comfortably.  
  
  

  
