[Back to TOC](toc.md)  
[Back to chapter 11](chap11.md)    
   
---      
    

# 12. Hardware in Conjunction with the BSB-LPB-LAN Adapter
    
    
---
    
## 12.1 The Arduino Mega 2560
*In general, the use of an original Arduino Mega 2560 (Rev3) is recommended.*  
From experience, however, cheap replicas ("clones") of the Arduino Mega 2560 can also be used, the use of these clones is usually possible without any problems. But: It should be paid attention if a modified board layout (e.g. changed pin assignments) is described in the prduct description. If this is the case and you still want to buy it, you may need to make specific adjustments in the file *BSB_lan_config.h*.
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/mega_clone.jpg">  
   
*A compatible clone of the Arduino Mega 2560 (Rev3).*  
   
***Note:***  
Regarding to the [tech specs of the Arduino Mega 2560](https://store.arduino.cc/arduino-mega-2560-rev3), it is recommended to use an external power source at the intended connection of the Arduino (e.g. 9V/1000mA).  
    
    
---
    
## 12.2 The LAN Shield
*In general, the use of an original Arduino LAN shield (v2) is recommended.*  
From experience, however, cheap replicas ("clones") of these LAN shields can also be used, the use of these clones is usually possible without any problems. But: It should be paid attention if a modified board layout (e.g. changed pin assignments) is described in the prduct description. If this is the case and you still want to buy it, you may need to make specific adjustments in the file *BSB_lan_config.h*.  
   
There are / have been two different versions of LAN shields available on the market: one with a WIZnet W5100 chip (v1) and one with a W5500 chip (v2). The usage of a v2-shield is recommended, it's also available at the official [Arduino store](https://store.arduino.cc/arduino-ethernet-shield-2).  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/lanshield_clone.jpg">  
   
*A compatible clone of a LAN shield with a W5100 chip.*  
       
***Notes:***     
After the installation of the Arduino IDE it should be checked that the current version of the Ethernet Library (min. v2) is installed.   
As a LAN cable one should preferably use a S/FTP type with a minimum length of one metre.  
   
    
---
   
## 12.3 Usage of Optional Sensors: DHT22 and DS18B20
*Sorry, not yet translated.. :(*  

Es besteht die Möglichkeit, zusätzliche Sensoren des Typs DS18B20
(OneWire-Temperatursensor) und DHT22 (Temperatur- und
Feuchtigkeitssensor) direkt an bestimmte Pins des Adapters bzw. Arduino
anzuschließen. Die entsprechenden Bibliotheken für die Arduino IDE sind
bereits im Softwarepaket des Adapters integriert.

Der Anschluss der Sensoren kann i.d.R. an GND und +5V des Adapters / Arduino
(unter zusätzlicher Verwendung der fühlerspezifischen
PullUp-Widerstände!) stattfinden.

Zur Nutzung dieser Sensoren muss lediglich die *Konfiguration in der
Datei BSB\_lan\_config.h entsprechend angepasst werden*: Es sind die
jeweiligen Definements zu aktivieren und die für DATA genutzten
Digitaleingänge bzw. Pins festzulegen (s. hierzu auch Kap. [5](kap05.md)).

Auf die Daten der Sensoren kann nach erfolgter Installation über das
Webinterface (jeweilige Links im oberen Bereich) oder mittels des
URL-Befehls /T zugegriffen werden.  
   
Darüber hinaus werden sie unter `<URL>/ipwe.cgi` standardmäßig mit angezeigt. Voraussetzung hierfür ist jedoch, dass die IPWE-Erweiterung in der Datei *BSB\_lan\_config.h* durch das entspr. Definement aktiviert wurde (s. Kap. [5](kap05.md)).
   
Sollen die gemessenen Werte geloggt werden oder sind
24h-Mittelwertsberechnungen gewünscht, so kann dies mit den jeweiligen
Anpassungen in der Datei *BSB\_lan\_config.h* (s. Kap. [5](kap05.md)) ganz einfach realisiert
werden.

***Tipp:***  
*Werden DS18B20-Sensoren verwendet, so werden unter /T (und -falls aktiviert- ebenfalls unter `<URL>/ipwe.cgi`) die jeweils **spezifischen internen Hardwarekennungen (SensorID) der DS18B20-Sensoren** aufgeführt. Diese SensorID ist für eine spätere eindeutige Unterscheidung der einzelnen Sensoren notwendig und sollte bspw. bei der weitergehenden Verwendung mit externen Programmen wie FHEM berücksichtigt werden (Stichwort RegEx).  
Es ist empfehlenswert, die jeweilige SensorID zu notieren und den entspr. Sensor zu beschriften. Dazu kann ein einzelner Sensor kurz erwärmt oder abgekühlt und durch einen erneuten Aufruf von /T anhand der Temperaturschwankung identifiziert werden.  
Werden Sensoren ausgetauscht, hinzugefügt oder entfernt, so ändert sich meist auch die Reihenfolge, in der sie unter /T angezeit werden (da diese auf der SensorID basiert). Wird das Reading also nicht auf die individuelle SensorID ausgelegt, sondern lediglich auf die Bezeichnung "temp\[x\]" wie sie bei /T angezeigt werden, so kommt es früher oder später dazu, dass die entsprechend gemachten Zuordnungen (bspw. VL, RL, Puffer) nicht mehr übereinstimmen.  
Die folgenden Screenshots verdeutlichen das Geschilderte.*  

*Ausgabe von /T mit zwei installierten Sensoren:*  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN/master/docs/pics/DS18B20_2sensoren_T.jpg">  
   
*Nach dem Hinzufügen eines dritten Sensors und erfolgtem Neustart des Arduino ändert sich die dargestellte Reihenfolge:*     
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN/master/docs/pics/DS18B20_3sensoren_T.jpg">  
   
*Hinweis:  
Werden Änderungen an der Sensorinstallation vorgenommen (Austausch, Hinzufügen, Entfernen), so muss der Arduino neu gestartet werden, damit die Sensoren initial neu eingelesen werden.*  


    
---
    

### 12.3.1 Notes on DHT22 Temperature/Humidity Sensors
*Sorry, not yet translated.. :(*  

DHT22-Sensoren werden häufig als „1 wire“ beworben, jedoch handelt es 
sich hierbei NICHT um den OneWire-Bus von Maxim Integrated oder eine andere Form 
eines ‚echten‘ Bussystems, bei dem jeder Sensor eine spezifische Adresse aufweist! 
Die DHT22-Sensoren sind demzufolge auch nicht mit den ‚echten‘ 
Maxim-OneWire-Sensoren/-Komponenten kompatibel.   
   
Die einzelnen DHT22-Sensoren weisen i.d.R. vier Anschlusspins auf, von denen jedoch der dritte Pin von links (bei Ansicht auf die Oberseite des Sensors) meistens nicht belegt ist. Im Zweifelsfall sollte dies jedoch nochmal nachgemessen werden! Die Belegung der Pins ist üblicherweise wie folgt:  
Pin 1 = VCC (+)  
Pin 2 = DATA  
Pin 3 = i.d.R. nicht belegt  
Pin 4 = GND (-)  

Bei Anschluss der Sensoren muss ein PullUp-Widerstand zwischen VCC (Pin 1) und DATA (Pin 2) in der Größe von etwa 4,7kΩ bis 10kΩ hinzugefügt werden. Meist werden 10kΩ empfohlen, die richtige Größe muss im Zweifelsfall ermittelt werden.  
   
***Bitte beachte:***    
*Kommen mehrere DHT22-Sensoren zum Einsatz, so muss für jeden 
DATA-Anschluss ein eigener Pin am Arduino genutzt und in der Datei
BSB\_lan\_config.h definiert werden.*  
        
Neben den 'nackten' Sensoren gibt es auch noch Ausführungen, die bereits auf einer kleinen Platine angebracht und bei der die drei notwendigen Anschlusspins abgeführt und beschriftet sind. Die folgende Abbildung zeigt ein solches Modell des baugleichen Sensors AM2302.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/AM2302.jpg">  
   
*Tipp:*  
*Im Internet finden sich zahlreiche Tutorials, Leitfäden und Anwendungsbeispiele für die Anwendung von DHT22-Sensoren.*
        
---
    
### 12.3.2 Notes on DS18B20 Temperature Sensors
*Sorry, not yet translated.. :(*  
DS18B20-Sensoren sind 'echte' 1-Wire-/OneWire-Komponenten der Firma Maxim Integrated (ursprünglich Dallas Semiconductor).  
Jeder Sensor weist eine spezifische interne SensorID auf, die es insbesondere bei größeren Installationen deutlich einfacher macht, einzelne Sensoren zu identifizieren, sofern man vor der finalen Installation die ID ausgelesen und gut sichtbar auf/an den Sensoren angebracht hat (siehe Tipp in Kap. [12.3](kap12.md#123-verwendung-optionaler-sensoren-dht22-und-ds18b20)).  
Neben der üblichen Bauart TO-92 sind die Sensoren auch in wasserdicht
gekapselten Ausführungen mit verschiedenen Kabellängen erhältlich.  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/DS18B20.jpg">  

Die gekapselte Ausführung macht den Einsatz gerade im Bereich der Heizungssteuerung
sehr interessant, da hiermit schnell und kostengünstig eine individuelle
Installation für diverse Temperaturmessungen realisiert werden kann.  
   
***Tipps für die elektrische Installation:***  
Die einzelnen Sensoren weisen i.d.R. drei Pins auf: VCC, DATA und GND.  
Bei den gekapselten Versionen ist die Farbwahl der bereits angeschlossenen Kabel meist wie folgt:  
Rot = VCC (+5V)  
Gelb = DATA  
Schwarz = GND (-)  
   
Kommen mehrere DS18B20-Sensoren und/oder größere Leitungslängen zum
Einsatz, hat es sich bewährt, pro Sensor je einen 100nF-Keramikkondensator (und
ggf. noch einen 10µF-Tantalkondensator zusätzlich) möglichst nah am
Sensor in die Leitung zwischen GND und VCC (+5V) zu positionieren, um
einen Spannungsabfall bei der Abfrage zu kompensieren.  
   
*Anmerkungen:*  
- *Kommen die üblichen gekapselten und bereits verkabelten Sensoren zum Einsatz, so reicht es i.d.R. aus, den Kondensator dort anzuschließen, wo auch die Kabel angeschlossen werden - ein Auftrennen des Kabels nahe des Sensors ist -zumindest bei den Versionen mit 1m und 3m Kabellängen- erfahrungsgemäß nicht nötig.*  
- *Im Gegensatz zu Keramikkondensatoren ist bei der (zusätzlichen) Verwendung von Tantalkondensatoren auf die Polarität zu achten!*  

Der Wert des PullUp-Widerstandes am Adapterausgang zwischen DATA und VCC
(+5V) ist für einen problemlosen Betrieb u.U. kleiner als die
üblicherweise empfohlenen 4,7kΩ zu wählen. 

Von der Verwendung des sogenannten ‚parasitären Modus' ist abzuraten.  
Die Verwendung einer geschirmten Steuerleitung ist zu empfehlen. Die Schirmung sollte dabei einseitig an Masse (GND) angeschlossen werden.  
Um etwaige von der Versorgungsspannung des Arduino-Netzteils ausgehende
Störeinflüsse zu minimieren, kann die Zuleitung der Stromversorgung
arduinoseitig etwa vier bis fünfmal durch einen Ferritring geführt
werden.
   
Kommen *große* Kabellängen zum Einsatz, so ist insbesondere auf eine korrekte Netzwerktopologie zu achten. Hier ist die Lektüre des vom Hersteller herausgegebenen Tutorials "[Guidelines for Reliable Long Line 1-Wire Networks](https://www.maximintegrated.com/en/design/technical-documents/tutorials/1/148.html)" zu empfehlen.  
In diesem Fall sind außerdem weitere Dinge zu beachten, wie bspw. eine empfehlenswerte Hin- und Rückleitung für den Datenkanal, der möglicherweise notwendige Einsatz von zusätzlichen Spannungsquellen, die Verwendung eines dedizidierten Busmasters etc.  
Als vereinfachte Faustregel kann man sagen, je größer die Leitungslängen und je komplexer die DS18B20-Installationen ausfallen, desto kritischer ist die vorhergehende Planung zu betrachten. 
   
*Tipp:*  
*Im Internet finden sich zahlreiche Tutorials, Leitfäden und Anwendungsbeispiele zum Thema 1-Wire/OneWire/DS18B20.*  
   
***Zusammenfassung benötigter Bauteile für eine Installation:***  
- Dreiadriges Kabel, idealerweise geschirmt (Schirmung ist dann einseitig an GND anzuschließen)  
- PullUp-Widerstand 4,7kΩ oder ggf. kleiner, nur einer notwendig, adapter-/arduinoseitig zwischen VCC und DATA positionieren   
- Keramikkondensator 100nF, pro Sensor einer, zwischen VCC und GND nahe am Sensor positionieren  
- optional: Tantalkondensator 10µF, pro Sensor einer (zusätzlich zum Keramikkondensator!), zwischen VCC und GND nahe am Sensor positionieren (bei Tantalkondensatoren bitte die Polarität beachten!)  
- optional: Schraublemmen o.ä., Streifen-/Lochrasterplatine, Gehäuse, ...   
   
***Tipps für die Verwendung im Bereich der Heizungsinstallation:***  
- Werden die gekapselten und bereits mit einem Kabel versehenen Sensoren eingesetzt, so kann es sich bei größeren und verzweigteren Heizungsanlagen lohnen, die Versionen mit 3m anstatt 1m Kabellänge zu nehmen. Sie kosten zwar etwas mehr, bieten jedoch deutlich mehr Spielraum und Bewegungsfreiheit bei der Platzierung der Sensoren.  
   
- Sollen die Sensoren für Temperaturmessungen an Rohren zum Einsatz kommen
(bspw. HK-VL/-RL), so ist es empfehlenswert, ein Bett aus Wärmeleitpaste
für den Kontaktbereich zu verwenden.  
Darüber hinaus haben Tests gezeigt, dass die Positionierung nach einem
Knick an der Außenseite eines Rohres ideal zu sein scheint, da hier die
Kerntemperatur des Strömungsmediums aufgrund der auftretenden
Verwirbelungen nah an die Rohrwand gelangt.  
Die Metallhülse der gekapselten Bauform sollte möglichst mit einer
metallenen Rohrschelle am Rohr fixiert werden. Das Kabel selbst sollte
zusätzlich mit einem Kabelbinder fixiert werden, um Zugkräfte an der
Fühlerhülse sowie ein Verrutschen des Fühlers zu vermeiden.  
Die Rohrdämmung sollte nach Anbringen des Fühlers (unter der Dämmung)
wieder gewissenhaft verschlossen werden. Löcher, Einschnitte o.ä. in
Fühlernähe sind zu vermeiden. Werden Fühler an bisher ungedämmten Rohren
montiert, so ist zumindest für den Bereich des Fühlers eine zusätzliche
Rohrisolierung empfehlenswert, um Messwertverfälschungen durch bspw.
Raum- oder Zugluft zu vermeiden.

- Kommen die Fühler in Tauchhülsen oder Klemmschienen zum Einsatz, ist
ggf. auch hier die Verwendung von zusätzlicher Wärmeleitpaste zu
empfehlen.

- Im Allgemeinen sollten die Fühler etwa ein bis zwei Meter von einer
zusätzlichen Wärmequelle (wie bspw. Heizkessel, Pufferspeicher o.ä.)
entfernt montiert werden.

***Bitte beachte:***  
***Bereits installierte Fühler (bspw. in Tauchülsen von Mischern, 
Pufferspeichern etc.), die an einen Heizungs- oder
Solarregler angeschlossen sind, haben immer Vorrang! Keinesfalls sollte
deren Installation oder der Kontakt mit dem zu messenden Element durch
eine zusätzliche Montage von DS18B20-Sensoren leiden!***  
        
***Bauvorschlag:***  
Bei kleineren DS18B20-Installationen im Heizungsbereich mit übersichtlichen Kabellängen kann man sich einen kleinen 'Verteilerkasten' bauen. Dazu kann man die gekapselten Sensoren nacheinander samt vorgeschalteter Kondensatoren auf einer Streifenplatine anschließen. Lötet man die Kabel der Sensoren nicht an, sondern verwendet statt dessen kleine Schraubklemmen, so kann man im Bedarfsfall problemlos einzelne Sensoren austauschen oder auch das System erweitern. Am Anfang dieser Verteilerplatine wird das Kabel angeschlossen, was zum BSB-LAN-Adapter bzw. zum Arduino geführt wird. Wenn die Optik nicht stört, kann das gesamte Konstrukt kostengünstig in einer Feuchtraum-AP-Verteilerdose untergebracht werden.   
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Verteiler_klein.jpg">  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Verteiler_groß.jpg">  


        
---
    
## 12.4 Relays and Relayboards
In general it's possible and within BSB-LAN already implemented to connect and query a relay which is connected to the Arduino. By this one couldn't only change the state of a relay by sending a specific command, it's also possible to just query the state.  
***It is NOT possible to connect the Arduino directly with the multifunctional inputs of the controller!***  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/relaisboards.jpg">  

*A single and a 4-channel relaymodule for the usage with an Arduino.*  
       
The often cheap relaymodules available for the usage with an Arduino are often already supplied with a relay which can handle high voltage like 125V or 230V. However, due to poor quality or just an overload, different risky damage can occur. Because of that one should consider to (additionally) use common couple or solid state relays which are used by electricians. in that case one should see the specific data sheet to confirm that the electrical current of the Arduino is strong enough to trigger the swithcing process of the relay.  
   
***WATCH OUT:***  
***Electrical installations should only be done by an electrician! High voltage like 230V or 125V can be deadly!*** *It's adviseable to already include an electrician at the state of planning.*   
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/koppelrelais.jpg">  
   
*A common coupling relay. At this specific type, the corresponding pins at the Arduino have to be connected with "14" and "13".*  
      

*Example:*  
If the controller of a solarthermic installation isn't already connected with the controller of the heating system, it's possible to query the state of the pump by installing a coupling relay parallel to the pump and connect the other 'side' of the relay with the specific pins of the Arduino. Now you can query the state of the relay and therefore the state of the pump with the Arduino.  
    
---
     
## 12.5 MAX! Components
BSB-LAN is already prepared for the usage of MAX! heating system components. MAX! thermostats that shall be included into BSB-LAN, have to be entered with their serial number (printed on a small label, sometimes in the battery compartment) in the file *BSB\_lan\_config.h* into the array `max_device_list[]`. After starting BSB-LAN, the pairing button has to be pressed on the thermostats in order to establish a connection between BSB-LAN and the thermostats.  
  
In *BSB\_lan\_custom.h* you can use the following variables for using MAX! devices:  
  
- `custom_timer`  
This variable is set to the value of millis() with each iteration of the loop() function.  
  
- `custom_timer_compare`  
This variable can be used in conjunction with `custom_timer` to create timed executions of tasks, for example to execute a function every x milliseconds.  
  
In addition to that, all global variables from *BSB\_lan.ino* are available. In regard to MAX! functionality, these are most notably:  
  
- `max_devices[]`  
This array contains the DeviceID of each paired MAX! device. You can use this for example to exclude specific thermostats from calculations etc.  
  
- `max_cur_temp[]`  
This array contains the current temperature of each thermostat. However, only temperatures from wall thermostats are reliable because they transmit their temperature constantly and regularly. Other thermostats do this only when there is a change in the valve opening or upon a new time schedule.   
  
- `max_dst_temp[]`  
This array contains the desired temperature of each thermostat.  
  
- `max_valve[]`  
This array contains the current valve opening of a thermostat (wall thermostats only carry this value when they are paired with a heater thermostat).  
  
The order inside of these arrays is always the same, i.e. if `max_devices[3]` is wall thermostat with ID xyz in the living room, then `max_cur_temp[3]` contains the current temperature in the living room, `max_dst_temp[3]` the desired temperature in the living room etc.  
  
The order inside `max_devices[]` depends on how the devices have been paired with BSB-LAN and remains the same after restarts of BSB-LAN since they are stored in EEPROM until this is erased by calling `http://<IP-Adresse>/N`. However, one should not completely rely on this and rather compare the ID stored in `max_device[]` for example when planning to ignore a specific thermostat in some kind of calculations. You can obtain this ID from the second column of `http://<IP-Adresse>/X` and take note that this is not the same as the ID printed on the label.  
  
Important note for those users who use a Max!Cube that has been flashed to CUL/CUNO (see information [here](https://forum.fhem.de/index.php/topic,38404.0.html)):  
If BSB-LAN was not running (or was busy otherwise) when the CUNO was set up, then you have to press the pairing button again on these devices, because only in that specific pairing process the ID printed on the devices label is sent together with the internally used device ID (and is also used by FHEM).  
   
You can also use the MAX! thermostats to calculate a weighted or average current or desired temperature (see [here](https://wiki.fhem.de/wiki/MAX) for configuring MAX devices under FHEM and [here](https://forum.fhem.de/index.php/topic,60900.0.html) for using the average temperature in FHEM).  
  
FHEM forum user *„Andreas29"* has created an example on how to use MAX! thermostats with BSB-LAN without using FHEM. A detailed description can be found in this forum post [here](https://forum.fhem.de/index.php/topic,29762.msg851382.html#msg851382). The "Arduino room controller light" is described in chapter [12.6.2](chap12.md#1262-room-temperature-sensor-wemos-d1-mini-dht22-display).  
    
---
    
## 12.6 Own Hardwaresolutions
The following solutions have been developed by BSB-LAN users. They should not only be a stimulation for re-building but also an example what's possible with additional own built hardware solutions in combination with BSB-LAN.  
   
If you also created something by your own of which you think that it could be interesting for other users, please feel free to contact me (Ulf) via email at `adapter (at) quantentunnel.de`, so that I eventually can present it here in the manual. Thanks!  
    
---
    
### 12.6.1 Substitute for a Room Unit (Arduino Uno, LAN Shield, DHT22, Display, Push Button Switch)
The member *„Andreas29"* of the German FHEM forum has built a substitute for a room unit, based on an Arduino Uno. Besides the data from a DHT22 sensor, the current state of function of the heating system is displayed on a 4x20 LCD. With a little push button he imitates the function of the presence button of a common room unit.  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Raumgerät_light_innen.jpg">
    
*The 'inside' of his substitute of a room unit.*  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/Raumgerät_light_Display.jpg">
    
*The display of his own built room unit.*  
    
A more detailed description including the circuit diagram and the software is available [here](https://forum.fhem.de/index.php/topic,91867.0.html) in the German FHEM forum.
   
Also, he expanded the functionality and implemented push messaging for certain error cases. The description and the software can be found [here](https://forum.fhem.de/index.php/topic,29762.msg878214.html#msg878214) in the German FHEM forum.
    
---
    
    
### 12.6.2 Room Temperature Sensor (Wemos D1 mini, DHT22, Display)
The member *„Gizmo\_the\_great"* of the FHEM forum has built a room temperature sensor based on a Wemos D1 mini and a DHT22 sensor. The current temperatures on the heating circuits 1 and 2 are additionally displayed at an OLED display. The Wemos D1 ist running ESPeasy.  

A more detailed description of his project you can find in [his GitHub Repo](https://github.com/DaddySun/Smart_Home_DIY).
     
---
    
## 12.7 LAN Options for the BSB-LPB-LAN Adapter
Even though the wired LAN connection is definitely the best option for integrating BSB_LAN into your network, it could be necessary to create an alternative way of connection, because a full-range wired connection (bus cable or LAN cable) just isn't possible.  
Therefore some options will be mentioned in the following subchapters.  
    
---
    
### 12.7.1 Usage of a PowerLAN / dLAN
The use of powerline adapters for expanding the LAN is an option, which could be the best and most reliable solution.  
However, sometimes powerline installations can cause trouble because of possible interferences they may cause. If you have separated phases within your electrical installation, it may just not work though. In that case ask an electrician about a phase coupler that he may could install.    
    
---
    
### 12.7.2 WLAN: Usage of an Additional Router
Another option is to connect the Arduino via LAN with an old router and integrate the router in your network via WLAN as a client. The speed of transmission usually is fast enough for the use of BSB-LAN. If the WLAN signal is weak, you can probably try to change the antennas and mount bigger ones.  
   
However, a stable and reliable WLAN connection should be achieved. Especially, if you are using additional smart home software to create logfiles, if you are using additional hardware like thermostats or if you want to control and influence the behaviour of your heating system.  
    
---
    
### 12.7.3 WLAN: Usage of an Additional ESP or a 'WLAN-Arduino'
Another option for WiFi connectivity is adding an ESP8266 Wifi module instead of a LAN-Ethernet-Shield. However, this requires additional efforts in configuration and assembly (level shifter etc.) The ESP has to be flashed with the origignal AT firmware by Espressif (further information see below). Keep in mind that without the Ethernet-Shield's microSD slot, logging on SD card won't be possible.

In addition to that, boards are available that incorporate an ESP8266 alongside the Arduino Mega. Due to the same size and same pin layout, it is fully compatible to Arduino shields as well as the BSB-LPB-LAN adapter.

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/WLAN_Ardu.jpeg">

*A 'WLAN-Arduino Mega 2560'.*

These boards can be configured via DIP switches (for example Arduino<->USB, Arduino<->ESP, ESP<->USB), so make sure you have the board configured correctly during installation and later use.
Again, without the Ethernet-Shield, the microSD card slot is also no longer available, therefore no logging on SD card is possible. Furthermore, transmission rates as well as latency is worse compared to a LAN connection because all data to and from the ESP will have to be channelled through the Arduino's serial pins and are limited to 115kpbs.

See below the installation instructions by FHEM user "freetz" (orignal in German [here](https://forum.fhem.de/index.php/topic,29762.msg867911.html#msg867911)):

**Installation of the AT firmware**

- Download Firmware from Espressif:  
(https://www.espressif.com/en/support/download/at)  
Instructions here are based on version 1.7.

- Flashing firmware  
Shown here is the usage of esptool.py which should be available for all operating systems. Only the serial port (here: /dev/wchusbserial1420) has to be adjusted.  
At first, the DIP switches have to be set as follows from left to right:  
OFF OFF OFF OFF ON ON ON (ON)  
  
- Then switch to the directory where the firmware has been extracted to and run  
`esptool.py -p /dev/tty.wchusbserial1420 erase_flash`  
and then  
```
esptool.py -p /dev/tty.wchusbserial1420 --chip esp8266 write_flash -fm dio -ff 26m --flash_size 2MB-c1 0x00000 ./bin/boot_v1.7.bin 0x01000 ./bin/at/1024+1024/user1.2048.new.5.bin 0x1fc000 ./bin/esp_init_data_default_v08.bin 0xfe000 ./bin/blank.bin 0x1fe000 ./bin/blank.bin 0x1fb000 ./bin/blank.bin
```
   
**Testing the firmware**   
  
- Set the DIP switches to  
OFF OFF OFF OFF ON ON OFF (ON)  
  
- Then open a terminal program on the serial port with settings 115200 bps, 8 data bits, no parity, one stop bit (8N1). When entering "AT" (without quotes) followed by Enter, the response should be "OK"  
  
- To speed up transmission rate, you can try and issue the command `AT+UART_CUR=230400,8,1,0,1` and double the transmission rate. You also have to change the baudrate in the setup() function of BSB_lan.ino accordingly. However, I would only try this when you are sure the 115200 work reliably.  
  
**Flashing BSB-LAN**  
  
- Now that the ESP is correctly flashed and configured, set the DIP switches to  
ON ON ON ON OFF OFF OFF (ON)  
Also, move the second switch to the bottom right of the DIP switches to "RXD3/TXD2".  
  
- Now the Wemos Mega behaves like a classic AtMega2560 and communicates internally with the ESP8266 via its Serial3 port.  
  
**Configuring BSB-LAN**  
  
- In BSB_lan_config.h you now have to activate WiFI via the `#define WIFI` definement. Furthermore, the variables `ssid` und `pass` have to be filled with the credentials of your local WLAN. If the variable `IPAddr` is set, a static IP will be used. If commented out, DHCP will be used. Manually assigning a gateway is not supported by the WiFiEsp library and will thus be ignored.  
    
---  
   
## 12.8 Housing
The market offers just a small range of housings which are compatible for an Arduino Mega 2560 plus additional shields. Besides commercial products and creative own built solutions, a 3D printer could be used to create a great housing.  
**The member "EPo" of the German FHEM forum was so kind to create and offer STL datafiles for two different housings.**  
**Thanks a lot!**  
       
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/ArduinoBSB.jpg">  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/ArduinoBSB-H.jpg">  
   
You can download a zip-file containing the pictures and the STL datafiles [here](https://github.com/1coderookie/BSB-LPB-LAN_EN/raw/master/case/3D_case_bsb-lan.zip).  
   
---  
## 12.9 Raspberry Pi 2

The adapter v2 could also be used in conjunction with a Raspberry Pi 2. Therefore you have to make sure you are using different pin headers, the additional circuits and parts. For more detailed informations see the [circuit diagram](appendix_a1.md) and the [notes on the circuit diagram](appendix_a2.md).  
   
**BUT:**  
In that case you also have to use a different software than BSB-LAN: ["bsb_gateway"](https://github.com/loehnertj/bsbgateway) by J. Loehnert.  
**Here, no support is given for "bsb_gateway", this manual is only about BSB-LAN!**  
   
For those users who want to use the adapter with an RPi2 and an old controller with PPS, D. Spinellis wrote a Python script: [PPS-monitor](https://github.com/dspinellis/PPS-monitor).   
   

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-adapter-komplett-rpi.jpeg">  
    
*The BSB-LPB-LAN adapter v2 mounted to a Raspberry Pi 2.*  
   
---  
   
[Further on to chapter 13](chap13.md)      
[Back to TOC](toc.md)   
