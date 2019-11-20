[Back to TOC](toc.md)  
[Back to chapter 9](chap09.md)    
   
---      
        

# 10. Reading Out New Parameter Telegrams  
If your heating system has parameters which aren't implemented in BSB-LAN yet, you can help us to make BSB-LAN even better. For that you have to use the serial monitor of the ArduinoIDE and read out the specific telegram / command ID of each parameter including the different options (if available). Here are some instructions and explanations how to do it, please read it completely before you start.  
   
Before we start with the procedure of reading out, first we have a look at the structure of the BSB data telegrams:  
Byte 1: always 0xDC (start of telegram)  
Byte 2: source device (0x00 = heating system, 0x06 = room device 1, 0x07 = room device 2, 0x0A = display, 0x7F = all) plus 0x80  
Byte 3: destination device (same values as source)  
Byte 4: telegram length (start-of-telegram byte (0xDC) is not counted)  
Byte 5: message type (0x02 = info, 0x03 = set, 0x04 = ack, 0x05 = nack, 0x06 = query, 0x07 = answer, 0x08 = error)  
*Byte 6-9: Command ID (that's what we're interested in!)*  
Byte 10...: Payload data (optional)  
Last two bytes: CRC checksum   

To now read out the telegram / command ID of a new parameter, all you need is to connect your Arduino to a Laptop/PC via USB while it is connected to your heating system and follow these steps (BSB only, LPB is similar, but telegram structure is a bit different):

- Step 1: Start the Arduino IDE and turn on the serial monitor.  

- Step 2: Enable logging to the serial console and turn on verbose output with URL-Parameter /V1 (if you deactivated the verbose mode in the file BSB\_lan\_config.h before) on the Arduino, e.g. http://192.168.178.88/V1. Alternatively, you can log bus telegrams to SD card by using (only) logging parameter 30000 (see logging section above) and set variable `log_unknown_only` to 1 (URL command /LU=1) and follow logging entries with URL command /D.  

- Step 3: On the heating system, switch to the parameter you want to analyze (using the command wheel, arrows or whatever input mode your heating system has).   

- Step 4: Wait for "silence" on the bus and then switch forward one parameter and then back again to the parameter you want. You should now have something like this on the log:  
```
DISP->HEIZ QUR      113D305F
DC 8A 00 0B 06 3D 11 30 5F AB EC
HEIZ->DISP ANS      113D305F 00 00
DC 80 0A 0D 07 11 3D 30 5F 00 00 03 A1 
DISP->HEIZ QUR      113D3063
DC 8A 00 0B 06 3D 11 30 63 5C 33
HEIZ->DISP ANS      113D3063 00 00 16
DC 80 0A 0E 07 11 3D 30 63 00 00 16 AD 0B 
```  
The first four lines are from the parameter you switched forward to, the last four lines are from the parameter you want to analyze (doing the switching back and forth only makes sure that the last message on the bus is really the parameter you are looking for). Instead of DISP you might see RGT1, depending on what device you are using to select the parameter.  
*Note:* If the parameter you want to read out has different optional setting between which you can choose, you should try to read out the command ID for each option. For that you (usually) have to change the optional setting and confirm the change by pressing the OK-button. But: Please only do this if you are sure that these settings are not critical for the function of your heating system or your installation! If you change the settings, then you also have to write down the specific name for the chosen option where the command ID belongs to.  
If the parameter you are decoding has a unit like degrees, volt or so, please also write that one down.  
   
- Step 5: The lower data telegram above has the command ID 0x113D3063. Please note that the first two bytes of the command ID are switched in message type "query" (0x06), so make sure you look at the right telegram (type "answer" (0x07), last line in this example).  

- Step 6: Look up the "global command table" section in file BSB\_lan\_defs.h and check whether an entry for this command already exists (search for STRxxxx where xxxx is the parameter number). If it does exist, go on to step 8.   

- Step 7: If your parameter is not yet listed in the "global command table", you have to add an entry in the "menu strings" section like this:  
`const char STRxxxx[] PROGMEM = STRxxxx_TEXT;`  
Then add the actual text description in the language file (preferrably at least in both LANG_DE.h and LANG_EN.h):  
`#define STRxxxx_TEXT "Dummy description"`  
Now copy a line from the "global command table" section where your new parameter would fit numerically. Proceed with step 8 but rather than replacing CMD_UNKNOW you have to replace the command ID and value type of the copied line of course.  

- Step 8: Replace the CMD_UNKNOWN with the command ID you just found. If the return value type (column 3) is VT\_UNKNOWN try and find out which parameter type from the list at the beginning of the file works. For example, if the parameter should return a temperature value, you can try VT\_TEMP, VT\_TEMP_SHORT, VT\_TEMP\_SHORT5 or VT\_TEMP\_WORD. For parameters which offer multiple options, you have to add a corresponding line in the "ENUM tables" section.  

- Step 9: If the web interface gives you the same value as is displayed on the heating system, you have found the right value type and the parameter is now fully functional (i.e. querying and setting the value will work). Congratulations!  

- Step 10: When you are done, double check that the new command ID is not used somewehere else in the definition file (i.e. search for the command id - only one location should come up). It's possible that command IDs exist for different parameters with different heating systems because the protocol is not standardized and manufacuters don't seem to bother how other manufacturers use the command IDs, at least with less general parameters. If it occurs that a command ID is now existing twice in the BSB\_lan\_defs.h file, please mark it clearly when sending us the update and state which heating system you are using. We will then add a conditional compile flag so that heatingy system X will compile differently than system Y so that in the end both will use the ambiguous command ID for the right parameter.  

- Step 11: Please send only the new/updated lines to `bsb (ät) code-it.de` - if you use a diff-file, please make sure that you download the most recent BSB\_lan\_defs.h from the repository before making the diff because sometimes the file gets updated without an actual new version being released immediately.  
   
<!--- 
---
## 10.4 Beispiel für eine ‚Meldedatei'
Hier ein Beispiel für eine erstellte ‚Meldedatei', die alle notwendigen
Informationen für eine weitere Verarbeitung und Implementierung der
neuen Parameter enthält (*Achtung: Dies ist noch ein altes Beispiel, aktuell rufe bitte /Q sowie /6220-6236 auf! Ein aktuelles Beispiel folgt!*):   
```
Brötje NovoCondens SOB 26 C (Öl)  
Anschluss: BSB   
6220 Konfiguration - Software- Version: 1.3  
6221 Konfiguration - Entwicklungs-Index: error 7 (parameter not supported)  
6222 Konfiguration - Gerätebetriebsstunden: 12345 h  
6223 Konfiguration - Bisher unbekannte Geräteabfrage: unknown type 000014  
6224 Konfiguration - Geräte-Identifikation: RVS43.222/100  
6225 Konfiguration - Gerätefamilie: 96  
6226 Konfiguration - Gerätevariante: 100  
6227 Konfiguration - Objektverzeichnis-Version: 1.0  
6228 Konfiguration - Bisher unbekannte Geräteabfrage: unknown type 000014  
Parameter 2270 Kessel -- Rücklaufsollwert Minimum °C  
→ wird vom Arduino/BSB bei Abfrage mit 60°C angezeigt,
angezeigter Ist-Wert laut RGT-Bedieneinheit: 8°C  
RGT1->HEIZ QUR 053D0908  
DC 86 00 0B 06 3D 05 09 08 B0 E7  
HEIZ->RGT1 ANS 053D0908 00 02 00  
DC 80 06 0E 07 05 3D 09 08 00 02 00 4B 02  
Parameter 5010 Trinkwasserspeicher -- Ladung  
Mögliche Parameteroptionen: [Einmal/Tag | Mehrmals/Tag]  
Ist: Mehrmals/Tag  
RGT1->HEIZ QUR 253D0737  
DC 86 00 0B 06 3D 25 07 37 D2 92  
HEIZ->RGT1 ANS 253D0737 00 FF  
DC 80 06 0D 07 25 3D 07 37 00 FF CE 62  
Parameter 5050 Trinkwasserspeicher -- Ladetemperatur Maximum °C  
Mögliche Einstelloptionen: [8°C - 90°C]  
Ist: 60°C  
RGT1->HEIZ QUR 253D08A3  
DC 86 00 0B 06 3D 25 08 A3 01 91  
HEIZ->RGT1 ANS 253D08A3 00 0F 00  
DC 80 06 0E 07 25 3D 08 A3 00 0F 00 0D 90  
```
-->       
    
---  
   
[Further on to chapter 11](chap11.md)      
[Back to TOC](toc.md)   


