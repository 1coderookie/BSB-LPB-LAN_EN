# Appendix D: Notes For Users Of The Outdated Setup Adapter v2 + Mega 2560

For users of the outdated setup, the following refers to some questions and points that need clarification or that need to 
be considered.  
  
- *Do I have to switch to the new setup adapter v3 + Due?*  
No, if you are satisfied with the outdated setup and the range of functions of BSB-LAN has met your requirements so far, 
you can of course continue to use the old setup.
But: **In this case [BSB-LAN version v0.44](https://github.com/fredlcore/bsb_lan/releases/tag/v0.44) is the last stable and 
tested version for your setup!**  
Later versions may also work, but the Mega 2560 will most likely not have enough memory. You could try to disable certain 
functions (e.g. logging to the microSD card), but there is no guarantee that trouble-free operation will be possible.  
We will not answer any further questions about this.
  
- *Can I continue to use the Adapter v2 on a Due?*  
No! The reason for this is that neither the adapter v2 nor the Due has an EEPROM, which is necessary for BSB-LAN.
So if you want to benefit from the new functions of BSB-LAN in the future, you have to get an adapter v3 or make it yourself 
and use it with an Arduino Due.  
  
- *Can I 'convert' the adapter v2 to an adapter v3?*  
No! The primary reason for this is (among other reasons) again the missing EEPROM of the Due.  
We will not answer any further questions about this.  
  
- *Can I continue to use the adapter v3 with my previous Mega 2560?*  
No! Even if it would be possible after some changes to the Adapter v3, it would not offer any added value compared to the 
adapter v2. New functions of BSB-LAN would still not be able to be used due to the lack of memory of the Mega 2560. 
So if you want to use the new adapter v3, then only in combination with an Arduino Due.  
We will not answer any further questions about this.  
  
- *Why is there an EEPROM on the v3 board?*  
The Arduino Due has no EEPROM, but this is necessary for BSB-LAN.  
  
- *Can I continue to use the LAN-Shield if I change to the Due?*  
Yes, this is possible without any problems.  
  
- *Can I continue to use my existing housing?*  
Yes, the Due has the same form factor as the Mega 2560, so the dimensions of the case should fit. However, you will probably 
have to adapt your case a bit and add a cutout or a large hole for the middle USB port of the Due ('Programming Port'), 
so that you can continue to connect the corresponding USB cable comfortably.  
  
- *The Due's GPIOs are only 3.3V compatible - what do I do with existing components that require 5V?*  
In this case you have to use level converters (3.3V <-> 5V). Some common solutions, do-it-yourself solutions and/or special 
hints I will add to the manual as soon as possible.  
  
- *I have questions that have not been covered here yet - what now?*  
In this case you can of course contact us directly or via the corresponding forum thread. If the questions are relevant for 
other users as well, I will list them here.  
  
