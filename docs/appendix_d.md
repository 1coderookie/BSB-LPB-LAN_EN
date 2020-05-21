# Appendix D: Notes For Users Of The Outdated Setup Adapter v2 + Mega 2560

For users of the outdated setup, the following refers to some questions and points that need clarification or that need to 
be considered. If further questions occur, please post your questions in the [corresponding thread of the german FHEM forum](https://forum.fhem.de/index.php/topic,29762.0.html).  
Please understand, however, that we will not answer any questions that may arise, for example, about building an adapter v2 after the changeover to adapter v3.  
**PCBs v2 are no longer available, state of the art is the combination adapter v3 + Due.**  
  
- *Do I have to switch to the new setup adapter v3 + Due?*  
No, if you are satisfied with the outdated setup and the range of functions of BSB-LAN has met your requirements so far, 
you can of course continue to use the old setup.
But: **In this case [BSB-LAN version v0.44](https://github.com/fredlcore/bsb_lan/releases/tag/v0.44) is the last stable and 
tested version for your setup!** Within the zip-file there you'll also find the last 'Mega-compatible' version of the manual (PDF), which refers to the adapter v2 + Mega.  
Later versions may also work, but the Mega 2560 will most likely not have enough memory. You could try to disable certain 
functions (e.g. logging to the microSD card), but there is no guarantee that trouble-free operation will be possible.  
  
- *Can I continue to use the Adapter v2 on a Due?*  
No! The reason for this is that neither the adapter v2 nor the Due has an EEPROM, which is necessary for BSB-LAN.
So if you want to benefit from the new functions of BSB-LAN in the future, you have to get an adapter v3 or make it yourself 
and use it with an Arduino Due.  
  
- *Can I 'convert' the adapter v2 to an adapter v3?*  
No! The primary reason for this is (among other reasons) again the missing EEPROM of the Due.  
  
- *Can I continue to use the adapter v3 with my previous Mega 2560?*  
No! Even if it would be possible after some changes to the adapter v3, it would not offer any added value compared to the 
adapter v2. New functions of BSB-LAN would still not be able to be used due to the lack of memory of the Mega 2560. 
So if you want to use the new adapter v3, then only in combination with an Arduino Due.  
  
- *Why is there an EEPROM on the v3 board?*  
The Arduino Due has no EEPROM, but this is necessary for BSB-LAN.  
  
- *Can I continue to use the LAN-Shield if I change to the Due?*  
Yes, usually this is possible without any problems. A trouble-free usage of clones can't be guaranteed though.  
  
- *Can I continue to use my existing housing?*  
Yes, the Due has the same form factor as the Mega 2560, so the dimensions of the case should fit. However, you will probably 
have to adapt your case a bit and add a cutout or a large hole for the middle USB port of the Due ('Programming Port'), 
so that you can continue to connect the corresponding USB cable comfortably.  
  
- *The Due's GPIOs are only 3.3V compatible - what do I do with existing components that require 5V?*  
In this case you have to use level converters (3.3V <-> 5V). Some common solutions, do-it-yourself solutions and/or special 
hints I will add to the manual as soon as possible.  

  
