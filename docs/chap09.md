[Back to TOC](toc.md)  
[Back to chapter 8](chap08.md)    
   
---      
        

# 9. Excursus: Reading Out New Parameter Telegrams  
If your heating system has parameters which aren't implemented in BSB-LAN yet, you can help us to make BSB-LAN even better. For that you have to use the serial monitor of the ArduinoIDE and read out the specific telegram / command ID of each parameter including the state or value which is present at the time you are getting that command ID and the different options (if available). Here are some instructions and explanations how to do it, please read it completely before you start.  
   
To now read out the telegram / command ID of a new parameter, all you need is to connect your microcontroller to a Laptop/PC via USB while it is connected to your heating system and follow these steps (BSB only, LPB is similar, but telegram structure is a bit different):

- Start the Arduino IDE and turn on the serial monitor.  

- Enable logging to the serial console and turn on verbose output with URL-Parameter /V1 (if you deactivated the verbose mode in the file BSB\_lan\_config.h before) on the microcontroller, e.g. http://192.168.178.88/V1. Alternatively, you can log bus telegrams to SD card by using (only) logging parameter 30000 (see logging section above) and set variable `log_unknown_only` to 1 (URL command /LU=1) and follow logging entries with URL command /D.  

- On the heating system, switch to the parameter you want to analyze (using the command wheel, arrows or whatever input mode your heating system has).   

- Wait for "silence" on the bus and then switch forward one parameter and then back again to the parameter you want. You should now have something like this on the log:  
```
DSP1->HEIZ QUR      113D305F
DC 8A 00 0B 06 3D 11 30 5F AB EC
HEIZ->DSP1 ANS      113D305F 00 00
DC 80 0A 0D 07 11 3D 30 5F 00 00 03 A1 
DSP1->HEIZ QUR      113D3063
DC 8A 00 0B 06 3D 11 30 63 5C 33
HEIZ->DSP1 ANS      113D3063 00 00 16
DC 80 0A 0E 07 11 3D 30 63 00 00 16 AD 0B 
```  
The first four lines are from the parameter you switched forward to, the last four lines are from the parameter you want to analyze (doing the switching back and forth only makes sure that the last message on the bus is really the parameter you are looking for). Instead of DSP1 you might see RGT1, depending on what device you are using to select the parameter.  
*Note:*  
If the parameter you want to read out has different optional setting between which you can choose, you should try to read out the command ID for each option. For that you (usually) have to change the optional setting and confirm the change by pressing the OK-button. **But:** Please only do this if you are sure that these settings are not critical for the function of your heating system or your installation! If you change the settings, then you also have to write down the specific name for the chosen option where the command ID belongs to. At the end you should go back to the preset setting. 
If the parameter you are decoding has a unit like degrees, volt or so, please also write that one down. 

*Important:*  
Write down the command ID with the specific name and unit (if available) of the new function. If you also read out the different optional settings for a new parameter, don't forget to also write it down.
In the end you should have a list which exactly shows the specific command ID together with the name of the parameter, the state or shown value at the time you read it out and (if available) the unit (like degrees or so). The same goes for the optional settings. Your work will be useless if e.g. you just write down the command ID togehter with the name of the new parameter - because then we still don't know what the specific command ID means (in terms like on/off etc.).  
   

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
→ wird vom Mikrocontroller/BSB bei Abfrage mit 60°C angezeigt,
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
   
[Further on to chapter 10](chap10.md)      
[Back to TOC](toc.md)   


