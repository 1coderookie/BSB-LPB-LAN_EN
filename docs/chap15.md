[Back to TOC](toc.md)  
[Back to chapter 14](chap14.md)    
   
---   
    
    
# 15. FAQ 
*Note:*  
*Until the translation of the german faq is complete, the old faq takes place here:*  
<H2>Contents</H2>
<B><A HREF="#can-i-use-the-interface-board-on-a-raspberry-pi">Can I use the interface board on a Raspberry Pi?</A></B><BR>
<B><A HREF="#can-i-run-the-software-on-a-raspberry-pi">Can I run the software on a Raspberry Pi?</A></B><BR>
<B><A HREF="#is-there-a-simple-way-to-log-parameters">Is there a simple way to log parameters?</A></B><BR>
<B><A HREF="#im-using-fhem-and-want-to-process-the-data-from-my-heating-system-how-can-i-do-this">I'm using FHEM and want to process the data from my heating system. How can I do this?</A></B><BR>
<B><A HREF="#i-have-a-relay-shield-added-to-the-arduino-mega-how-can-i-setquery-the-individual-relays">I have a relay shield added to the Arduino Mega, how can I set/query the individual relays?</A></B><BR>
<B><A HREF="#my-heating-system-has-parameters-that-are-not-supported-in-the-software-yet-can-i-help-adding-these-parameters">My heating system has parameters that are not supported in the software yet, can I help adding these parameters?</A></B><BR>

<H2>Can I use the interface board on a Raspberry Pi?</H2>

Yes, by using different pin headers (female instead of male). But please take note that you cannot use the BSB-LPB-LAN software which only runs on Arduino. You would have to use the completely different bsb_gateway software for BSB, which can be found <A HREF="https://github.com/loehnertj/bsbgateway">here</A> or the PPS-monitor software for PPS devices, which can be found <A HREF="https://github.com/dspinellis/PPS-monitor">here</A>. Please contact the author of the corresponding software directly for any support related to it. All information on this website applies to the Arduino version only! No support for bsb_gateway or PPS-monitor can be given here.

<H2>Can I run the software on a Raspberry Pi?</H2>

No. While you can use the interface board also on a Raspberry Pi 2, you would have to use a completely different software (bsb_gateway) which can be found <A HREF="https://github.com/loehnertj/bsbgateway">here</A>. Please contact the author of bsb_gateway directly for any support related to it. All information on this website applies to the Arduino version only! No support for bsb_gateway can be given here.

<H2>Is there a simple way to log parameters?</H2>

Yes, there is, both standalone and remote:

<B>For standalone usage of the device use the following procedure:</B>

Insert a FAT32-formatted micro SD card in the slot on the Ethernet shield before powering up the device. Some devices might not recognize cards larger than 2GB, in that case use a smaller card and format it with FAT16.<BR>
Then edit BSB_lan_config.h and activate the #define LOGGER directive. Then you can add the fields you want to be logged to the variable log_parameters and set the logging period with variable log_interval. You can later change both the interval as well as the parameters during runtime using the URL command "/L=[interval],[parameter1],...,[parameter20]".

Once the setup is done, power-up the device and wait for data coming in. All data is stored in the file datalog.txt file on the card in CSV file format and can be imported easily in Excel and OpenOffice. <BR>
You can watch the content of the file with URL command "/D". To reset the file, use command "/D0". This should also be done after first powering up the device because it initializes the file with a proper CSV file-header.<BR>
Please note that the Arduino is not an exact timepiece, so even though you might set the interval to 60 seconds, the time displayed in the file (taken from the heating system) may differ - this might be up to a second per minute. If exact logging time is essential, find out the average time difference between Arduino time and real time and adjust the logging interval accordingly, e.g. use 59 seconds instead of 60.

<B>For remote logging follow this procedure:</B>

Run this command periodically (e.g. via a cron job):
<pre>
DATE=`date +%Y%m%d%H%M%S`; wget -qO- http://192.168.1.50/1234/8310/720/710 | egrep "(8310|720|710)" | sed "s/^/$DATE /" >> log.txt
</pre>
The resulting log.txt file of this example contains the logged values for parameters 8310, 720 and 710. Just change these parameter numbers in the http-request as well as the egrep command (and of course the IP address and the passcode) and you are set. 
Later on, you can sort the log file based on parameter numbers with the sort command:
<pre>
sort -k2 log.txt
</pre>

<H2>I'm using FHEM and want to process the data from my heating system. How can I do this?</H2>

Please note that FHEM is a complex software and this is not the place to provide basic introductions or instructions. But if you have already configured other devices for FHEM, this will hopefully help you:

To access the web interface of the device, you can use FHEM's HTTPMOD module. Here's an example configuration which you can easily adapt to your own needs and parameters. Of course you need to change the IP address as well as the passcode, too.
This code polls parameters 8700, 8743 and 8314 every 300 seconds and assigns these to the device "THISION" (the name of my heating system) to the readings "Aussentemperatur", "Vorlauftemperatur" and "Ruecklauftemperatur". It furthermore provides a reading "Istwert" that can be set via FHEM to provide the current room temperature to the heating system (parameter 10000). Finally, it calculates the difference between "Vorlauftemperatur" and "Ruecklauftemperatur" and assigns this difference to the reading "Spreizung".

<pre>
define THISION HTTPMOD http://192.168.1.50/1234/8700/8743/8314 300
attr THISION userattr reading0Name reading0Regex reading1Name reading1Regex reading2Name reading2Regex readingOExpr set0Name set0URL
attr THISION event-on-change-reading .*
attr THISION reading0Name Aussentemperatur
attr THISION reading0Regex 8700 .*:[ \t]+([-]?[\d\.]+)
attr THISION reading1Name Vorlauftemperatur
attr THISION reading1Regex 8743 .*:[ \t]+([-]?[\d\.]+)
attr THISION reading2Name Ruecklauftemperatur
attr THISION reading2Regex 8314 .*:[ \t]+([-]?[\d\.]+)
attr THISION readingOExpr $val=~s/[\r\n]//g;;$val
attr THISION set0Name Istwert
attr THISION set0URL http://192.168.1.50/1234/I10000=$val
attr THISION timeout 5
attr THISION userReadings Spreizung { sprintf("%.1f",ReadingsVal("THISION","Vorlauftemperatur",0)-ReadingsVal("THISION","Ruecklauftemperatur",0));; }
</pre>

Please note that the Regex must match from the beginning of the string, i.e. from the parameter number (such as 8700) and not from somewhere later in that string.

<H2>I have a relay shield added to the Arduino Mega, how can I set/query the individual relays?</H2>

The following is an example for a FHEM configuration that queries and sets the three relay ports named "Heater", "Fan" and "Bell" attached to GPIO pins 7, 6 and 5 respectively (again, adjust IP and passcode accordingly):

<pre>
define EthRelais HTTPMOD http://192.168.1.50/1234/G05/G06/G07 30
attr EthRelais userattr reading0Name reading0Regex reading1Name reading1Regex reading2Name reading2Regex readingOExpr readingOMap set0Name set0URL set1Name set1URL set2Name set2URL setIMap setParseResponse:0,1 setRegex
attr EthRelais event-on-change-reading .*
attr EthRelais reading0Name Heater
attr EthRelais reading0Regex GPIO7:[ \t](\d)
attr EthRelais reading1Name Fan
attr EthRelais reading1Regex GPIO6:[ \t](\d)
attr EthRelais reading2Name Bell
attr EthRelais reading2Regex GPIO5:[ \t](\d)
attr EthRelais room Heizung
attr EthRelais set0Name Heater
attr EthRelais set0URL http://192.168.1.50/1234/G07=$val
attr EthRelais set1Name Fan
attr EthRelais set1URL http://192.168.1.50/1234/G06=$val
attr EthRelais set2Name Bell
attr EthRelais set2URL http://192.168.1.50/1234/G05=$val
attr EthRelais setParseResponse 1
attr EthRelais setRegex GPIO[0-9]+:[ \t](\d)
attr EthRelais timeout 5
</pre>

<H2>My heating system has parameters that are not supported in the software yet, can I help adding these parameters?</H2>

Yes, you can :)! All you need is to connect your Arduino to a Laptop/PC via USB while it is connected to your heating system and follow these steps (BSB only, LPB is similar, but telegram structure is a bit different):

1. Start the Arduino IDE and turn on the serial monitor
2. Enable logging to the serial console and turn on verbose output with URL-Parameter /V1 on the Arduino, e.g. http://192.168.1.50/1234/V1. Alternatively, you can log bus telegrams to SD card by using (only) logging parameter 30000 (see logging section above) and set variable log_unknown_only to 1 (URL command /LU=1) and follow logging entries with URL command /D.
3. On the heating system, switch to the parameter you want to analyze (using the command wheel, arrows or whatever input mode your heating system has).
4. Wait for "silence" on the bus and then switch forward one parameter and then back again to the parameter you want. You should now have something like this on the log:
<pre>
DISP->HEIZ QUR      113D305F
DC 8A 00 0B 06 3D 11 30 5F AB EC
HEIZ->DISP ANS      113D305F 00 00
DC 80 0A 0D 07 11 3D 30 5F 00 00 03 A1 
DISP->HEIZ QUR      113D3063
DC 8A 00 0B 06 3D 11 30 63 5C 33
HEIZ->DISP ANS      113D3063 00 00 16
DC 80 0A 0E 07 11 3D 30 63 00 00 16 AD 0B 
</pre>
The first four lines are from the parameter you switched forward to, the last four lines are from the parameter you want to analyze (doing the switching back and forth only makes sure that the last message on the bus is really the parameter you are looking for). Instead of DISP you might see RGT1, depending on what device you are using to select the parameter.
Each data telegram has the following structure:<BR><BR>
Byte 1: always 0xDC (start of telegram)<br>
Byte 2: source device (0x00 = heating system, 0x06 = room device 1, 0x07 = room device 2, 0x0A = display, 0x7F = all) plus 0x80<BR>
Byte 3: destination device (same values as source)<BR>
Byte 4: telegram length (start-of-telegram byte (0xDC) is not counted)<BR>
Byte 5: message type (0x02 = info, 0x03 = set, 0x04 = ack, 0x05 = nack, 0x06 = query, 0x07 = answer, 0x08 = error)<BR>
Byte 6-9: Command ID (that's what we're interested in!)<BR>
Byte 10...: Payload data (optional)<BR>
Last two bytes: CRC checksum<BR><BR>

5. The lower data telegram above has the command ID 0x113D3063. Please note that the first two bytes of the command ID are switched in message type "query" (0x06), so make sure you look at the right telegram (type "answer" (0x07), last line in this example).
6. Look up the "global command table" section in file BSB_lan_defs.h and check whether an entry for this command already exists (search for STRxxxx where xxxx is the parameter number). If it does exist, go on to step 8.
7. If your parameter is not yet listed in the "global command table", you have to add an entry in the "menu strings" section like this:
<pre>const char STRxxxx[] PROGMEM = STRxxxx_TEXT;</pre>
Then add the actual text description in the language file (preferrably at least in both LANG_DE.h and LANG_EN.h):
<pre>#define STRxxxx_TEXT "Dummy description"</pre>
Now copy a line from the "global command table" section where your new parameter would fit numerically. Proceed with step 8 but rather than replacing CMD_UNKNOW you have to replace the command ID and value type of the copied line of course.

8. Replace the CMD_UNKNOWN with the command ID you just found. If the return value type (column 3) is VT_UNKNOWN try and find out which parameter type from the list at the beginning of the file works. For example, if the parameter should return a temperature value, you can try VT_TEMP, VT_TEMP_SHORT, VT_TEMP_SHORT5 or VT_TEMP_WORD. For parameters which offer multiple options, you have to add a corresponding line in the "ENUM tables" section. 
9. If the web interface gives you the same value as is displayed on the heating system, you have found the right value type and the parameter is now fully functional (i.e. querying and setting the value will work). Congratulations!
10. When you are done, double check that the new command ID is not used somewehere else in the definition file (i.e. search for the command id - only one location should come up). It's possible that command IDs exist for different parameters with different heating systems because the protocol is not standardized and manufacuters don't seem to bother how other manufacturers use the command IDs, at least with less general parameters. If it occurs that a command ID is now existing twice in the BSB_lan_defs.h file, please mark it clearly when sending us the update and state which heating system you are using. We will then add a conditional compile flag so that heatingy system X will compile differently than system Y so that in the end both will use the ambiguous command ID for the right parameter.
11. Please send only the new/updated lines to bsb (ät) code-it.de - if you use a diff-file, please make sure that you download the most recent BSB_lan_defs.h from the repository before making the diff because sometimes the file gets updated without an actual new version being released immediately.
    
---
    

## 15.1 Kann ich Adapter & Software mit einem Raspberry Pi nutzen?

Ja und nein.  
Der Adapter kann mit einem Raspberry Pi 2 verwendet werden, wenn andere
Pinheader genutzt werden (weibliche statt männliche) und die Platine
entsprechend mit den Komponenten für die RPi-Nutzung bestückt ist
(R11-13, Q11+12, SJ2+3).

Die BSB-LAN-Software kann NICHT mit einem RPi verwendet werden, sie ist
ausschließlich auf dem hier vorgestellten Arduino-System lauffähig!  
Zur Nutzung des Adapters mit einem RPi muss eine vollkommen andere
Software genutzt werden. Weitere Informationen diesbezüglich sind am
Ende von Kap. [1](kap01.md) zu finden.  
    
---
    

## 15.2 Kann ich einen Adapter gleichzeitig an zwei Regler anschließen?

Nein, das geht leider nicht.

Derzeit benötigt man für jeden Regler einen Adapter bzw. ein komplettes
Hardware-Setup (Arduino, Ethernet-Shield, Adapter), um die jeweiligen
reglerspezifischen Parameter via BSB abrufen zu können.  
Sollten jedoch mehrere Regler vorhanden und bereits miteinander via LPB
verbunden sein, beachte bitte die folgende FAQ.  
    
---
    

## 15.3 Kann ich einen Adapter via LPB anschließen und mehrere Regler abfragen?
Ja, wenn die vorhandenen Regler bereits korrekt via LPB miteinander
verbunden und entsprechend konfiguriert sind (korrekte
LPB-Adressvergabe).  
Die Möglichkeit, Abfragen fallweise an unterschiedliche Regler zu
senden, ist mittlerweile gegeben, jedoch ist diese Funktion noch nicht
ausgiebig getestet. Siehe hierzu den entsprechenden Punkt in Kapitel [8.1](kap08.md#81-auflistung-und-beschreibung-der-url-befehle).  
    
---
    

## 15.4 Ist ein multifunktionaler Eingang des Reglers direkt via Adapter schaltbar?

Nein!

Die multifunktionalen Eingänge der Regler (bspw. H1, H2, H3 etc.) sind
nicht direkt an den Adapter anzuschließen!

Soll bspw. eine Betriebsartumschaltung oder Erzeugersperre mittels H1
als Arbeitskontakt realisiert werden, so muss der jeweilige Eingang den
Herstellerangaben entsprechend parametriert und belegt werden. Eine
Steuerung dieser Art muss mittels eines anzuschließenden Relais
erfolgen, dessen reglerseitiger Ausgang unbedingt potentialfrei sein
muss, d.h. es darf keinerlei Fremdspannung anliegen! Das Relais hat in
dem Fall lediglich die Aufgabe, den Kontakt zu schließen (oder zu
öffnen).

Das Relais wiederum kann jedoch unter bestimmten Umständen vom Arduino
gesteuert werden (bspw. mittels eines Relaisboards). Siehe hierzu auch Kap. [12.4](kap12.md#124-relais-und-relaisboards).

Entsprechende Relais findest du im Internet, bei Unsicherheiten solltest
du deinen Elektriker und/oder Heizungsinstallateur zu Rate ziehen. Eine
falsche Belegung und/oder Parametrierung kann den Regler u.U. zerstören!  
    
---
    

## 15.5 Ist zusätzlich ein Relaisboard am Arduino anschließ- und steuerbar?

Ja. Siehe diesbezüglich den entsprechenden Punkt in Kap. [8.1](kap08.md#81-auflistung-und-beschreibung-der-url-befehle) sowie Kap. [12.4](kap12.md#124-relais-und-relaisboards).  
    
---
    

## 15.6 Kann ich bspw. den Zustand eines angeschlossenen Koppelrelais abfragen?

Ja. Siehe diesbezüglich den entsprechenden Punkt in Kap. [8.1](kap08.md#81-auflistung-und-beschreibung-der-url-befehle).  
    
---
    

## 15.7 Kann ich behilflich sein, um bisher nicht unterstützte Parameter hinzuzufügen?

Ja! Wenn dein Heizungssystem über Parameter verfügt, die von der
Software bisher nicht unterstützt werden, würden wir uns sehr freuen,
wenn du uns unterstützt! Genauere Informationen zur Vorgehensweise sind
in Kap. [10](kap10.md) zu finden.  
    
---
    

## 15.8 Warum erscheinen bei einer Komplettabfrage einige Parameter doppelt?

Wenn du eine Komplettabfrage aller Parameter via URL-Befehl machst  
(`http://<IP-Adresse>/0-10000`) kann es sein, dass sich einige Parameter
bzw. Programmnummern in der Auflistung wiederholen. Dies kommt daher,
dass es es zwar unterschiedliche Parameter sind, diese aber die gleiche
Command ID haben. Dies stellt nur einen ‚optischen Mangel' dar, der die
Funktionalität nicht negativ beeinflusst.  
    
---
    

## 15.9 Warum werden manchmal bestimmte Parameter nicht angezeigt?

Wenn der Regler nach erfolgtem Adapteranschluss angeschaltet wird und
der Arduino zu diesem Zeitpunkt bereits lief, funktioniert die
automatische Reglererkennung nicht. Der Arduino muss dann lediglich
resettet bzw. aus- und wieder angeschaltet werden.  
  
Sollten dann bestimmte Parameter noch immer nicht erscheinen, so sollte
bitte einmal /Q ausgeführt und die Webausgabe gemeldet werden.  
    
---
    

## 15.10 Warum ist kein Zugriff auf angeschlossene Sensoren möglich?
Wenn du DHT22- und/oder DS18B20-Sensoren korrekt am Arduino/Adapter
angeschlossen hast, die entsprechenden Menüs im Webinterface jedoch
nicht anwählbar sind, hast du vermutlich die betreffenden Einträge in
der Datei *BSB\_lan\_config.h* nicht entsprechend angepasst.  
Siehe hierzu auch die Kapitel [5](kap05.md), [11](kap11.md) & [13](kap13.md).  
    
---
    

## 15.11 Ich nutze ein W5500-LAN-Shield, was muss ich tun?

Darauf achten, dass die aktuelle Version der Ethernet Bibliothek 
(mindestens Version 2.0) in der Arduino IDE vorhanden ist.   
    
---
    

## 15.12 Können Stati oder Werte als Push-Mitteilungen abgesetzt werden?

Nein, nicht ohne weitere Software wie z.B. FHEM. Dafür müsste ansonsten
die Therme ständig abgefragt werden, was den Bus (und die Erreichbarkeit
des Arduino) stark belasten würde. Die sinnvollere Variante wäre,
bestimmte Werte z.B. alle 60 Sekunden abzurufen und dann anhand
bestimmter Kriterien weitere Aktionen auszulösen.  
Bei FHEM wäre das mit DOIF oder NOTIFY möglich.  
    
---
    

## 15.13 Kann bspw. FHEM auf bestimmte Broadcasts ‚lauschen'?

FHEM kann zwar lauschen, aber BSB-LAN kann bisher keine eigenständigen
Nachrichten absetzen. Dazu müsste ein Hintergrundprozess die
auflaufenden Broadcast-Meldungen anhand konfigurierbarer Schwellenwerte
auswerten und über einen HTTP-Client-Aufruf an eine definierbare
Zieladresse absetzen. Ob dies mit dem begrenzten Speicherplatz des
Arduino noch umsetzbar ist, wäre fraglich. Wer sich aber daran probieren
möchte, ist herzlich eingeladen, dies zu tun!  
    
---
    

## 15.14 Warum kommt es manchmal zu timeout-Problemen bei FHEM?

Das könnte an der Dauer des Sende-/Empfangsvorgangs liegen. Man sollte
den timeout-Wert in FHEM so bemessen, dass für jeden Parameter pro
Setzbefehl zwei und pro Abfragebefehl eine Sekunde angesetzt werden.

Sind mehrere einzelne (BSB-LAN-spezifische) HTTPMOD-Abfragen definiert
und werden diese zum gleichen Zeitpunkt ausgeführt, kann es außerdem
vorkommen, dass es zu Kollisionen kommt und sie sich somit gegenseitig
‚behindern', da der Bus bereits von einer Abfrage belegt ist. Als
Abhilfe können hier entweder unterschiedliche Abfrageintervalle gewählt
oder alle Abfragen in eine HTTPMOD-Abfrage gelegt werden.

FHEM-Forumsmitglied *„frank"* hat den [Tipp](https://forum.fhem.de/index.php/topic,29762.msg841039.html#msg841039) 
gegeben, bei der Einbindung in FHEM ‚attr alignTime' zu nutzen, 
um Kollisionen bei den Abfragen zu verhindern.   
    
---
    


## 15.15 Gibt es ein Modul für FHEM?

Jein. Ein Modul wird gerade vom FHEM-Forumsmitglied *„justme1968"*
entwickelt:
[https://forum.fhem.de/index.php/topic,84381.0.html](https://forum.fhem.de/index.php/topic,84381.0.html)  
Die Entwicklung ist jedoch noch nicht abgeschlossen, so dass ein
zuverlässiger und problemloser Einsatz bisher noch nicht garantiert
werden kann.  
    
---
    


## 15.16 Warum werden unter /B bei Stufe 2 keine Werte angezeigt?

Wenn du einen Gasbrenner hast, so wird dieser höchstwahrscheinlich
modulieren und generell nicht über ein zweistufiges Brennersystem
verfügen; zweistufige Brenner kommen meist nur bei Ölbrennern zum
Einsatz. Die Unterscheidung der Brennerstufen wird mittels spezifischer
Broadcasts vorgenommen, die jedoch nicht jeder Regler sendet. In dem
Fall werden die Brennerstarts und -laufzeiten kumuliert unter Stufe 1
dargestellt. Bitte beachte diesbezüglich auch den Hinweis unter „/B" in Kap. [8.1](kap08.md#81-auflistung-und-beschreibung-der-url-befehle).  
    
---
    


## 15.17 Ich habe den Eindruck, die angezeigten Werte bei /B sind nicht korrekt.

Das kann durchaus sein. Die jeweiligen Starts und Laufzeiten werden
anhand von Broadcasts ermittelt, die automatisch vom Regler gesendet
werden. Manchmal kann es vorkommen, dass einzelne BCs nicht ankommen,
bspw. wenn zeitgleich eine Abfrage gestartet wird oder der Arduino das
Logfile lädt.  
    
---
    


## 15.18 Was ist der genaue Unterschied zwischen /M1 und /V1?

Mit dem URL-Befehl /M1 aktivierst du den Monitor-Modus, mit /V1 den
Verbositäts-Modus.

Mit aktivierter Monitor-Funktion (/M1) werden alle Daten, die über den
Bus gehen und nicht von BSB-LAN aus initiiert wurden, „roh" auf dem
seriellen Monitor ausgegeben.  
Dies kann sinnvoll sein, um Fehlfunktionen in der Datenübertragung
ausfindig zu machen, da ansonsten nur Meldungen von BSB-LAN verarbeitet
werden, die von ihrem Aufbau her korrekt sind. Das schließt auch die
Verarbeitung von Broadcast-Nachrichten ein, d.h. mit aktivierter
Monitor-Funktion findet keine Auswertung dieser Nachrichten statt.

Die Monitor-Funktion erlaubt es z.B. bei Fehlermeldungen genauer zu
sehen, ob eine Nachricht schlichtweg nicht auf dem Bus angekommen ist
oder ob BSB-LAN sie wegen fehlerhafter Übertragung verworfen hat. Die
volle Kontrolle hätte man mit einem zweiten BSB-LAN-Adapter, der auf dem
Bus lauscht und dann alle ein- und ausgehenden Nachrichten
protokolliert.

Mit (seit v0.41 per default) aktiviertem Verbositäts-Modus (/V1) werden
zu jedem von BSB-LAN initiierten Aufruf und der entsprechenden Antwort
neben dem Klartext auch die entsprechenden Rohdaten auf dem seriellen
Monitor ausgegeben, wenn die Nachricht von ihrem Aufbau her korrekt sind
und fehlerfrei übertragen wurden.  
Eine Auswertung von (fehlerfreien) Broadcasts findet hier weiterhin
statt. Es werden hier beim Senden aber nur die Daten ausgegeben, die
BSB-LAN vorbereitet hat. Dies muss nicht bedeuten, dass diese Daten -
z.B. bei Hardwarefehlern - auch auf dem Bus ankommen. Umgekehrt werden
beim Auswerten der Rückmeldung auf einen Befehl zwar die Daten
ausgegeben, die auf dem Bus zurück gekommen sind, aber nur dann, wenn
die Nachricht auch korrekt aufgebaut war.

Eine Kombination aus beiden Parametern ist möglich und führt dazu, dass
im Monitor-Modus auch bei von BSB-LAN initiierten Nachrichten die
Rohdaten ausgegeben werden - mit den bereits erwähnten Einschränkungen
des Verbositäts-Modus bezüglich des Verwerfens von nicht korrekt
aufgebauten Nachrichten.  
    
---
    


## 15.19 Kann ich eigenen Code in BSB-LAN einbinden?

Ja, dafür gibt es die Datei *BSB\_lan\_custom.h*. Hier können eigene
Programmteile geschrieben werden, die bei jedem Schleifendurchlauf
(loop) aufgerufen werden. Damit kann man z.B. unabhängig von externen
Home-Automations-Systemen Sensoren auswerten und/oder Relais schalten.  
Die Beispieldatei wertet z.B. zwei DHT22-Feuchtigkeits-/Temperatursensoren 
aus und schaltet beim Unter- bzw. Überschreiten ein Relais, das an einem 
digitalen Ausgang angeschlossen ist.  
    
---
    


## 15.20 Kann ich MAX!-Thermostate einbinden?

Ja, das ist möglich. Dazu musst du das entsprechende Definement in der
Datei *BSB\_lan\_config.h* aktivieren und anpassen. Mittels
entsprechender Modifikationen in der Datei *BSB\_lan\_custom.h* können
weitere Funktionen realisiert werden, mit der derzeitigen Programmierung
ist eine eigenständige Raum-Ist-Wert-Übermittlung (ohne FHEM) möglich.
Siehe auch die jeweiligen Punkte in den Kapiteln [5](kap05.md), [8.1](kap08.md#81-auflistung-und-beschreibung-der-url-befehle) sowie [12.5](kap12.md#125-max-komponenten).  
    
---
    


## 15.21 Warum ist der Adapter nach einem Stromausfall nicht mehr erreichbar?
Dieses Verhalten wurde des Öfteren bei den günstigen LAN-Shield-Clones
beobachtet, mit einem originalen Arduino-LAN-Shield scheint dieses
Problem nicht aufzutreten.  
Nach Drücken des Reset-Knopfes am Arduino ist der Adapter wieder wie
gewohnt erreichbar. Abhilfe könnte eine kleine USV für den Arduino
schaffen, so dass der Arduino nicht stromlos wird. Andere Lösungen sind
bisher nicht bekannt.  
    
---
    


## 15.22 Warum ist der Adapter (ohne Stromausfall) manchmal nicht mehr erreichbar?
Dieses Problem ist bisher nur vereinzelt aufgetreten, eine eindeutige
Lösung oder Erklärung für dieses Verhalten gibt es bisher noch nicht.
Bei dem Nutzer, der davon berichtete, kam ebenfalls ein günstiger
LAN-Shield-Clone zum Einsatz. Laut Mitschnitt des Seriellen Monitors
lief der BSB-LAN-Sketch ohne Probleme weiter, lediglich das LAN-Shield
war nicht mehr erreichbar. Abhilfe schaffte nur ein Reset. Sollte dieses
Verhalten auftreten, ist das Testen eines weiteren LAN-Shields zu
empfehlen, da Hardwareprobleme des betroffenen LAN-Shields nicht
auszuschließen sind. Der Einsatz eines originalen Arduino-LAN-Shields
ist selbstverständlich eine weitere Option.  

Des Weiteren kann es bei Clones mit einem W5100-Chip aufgrund fehlerhafter Bauteilbestückung zu diffusen Problemen kommen. Siehe hierzu Kap. [12.2](kap12.md#122-das-lan-shield).  


---
    


## 15.23 Warum kommen beim Senden manchmal ‚query failed'-Meldungen?

Wenn Befehle, die in der Regel problemlos gesendet werden können,
plötzlich ‚query failed'-Fehlermeldungen auslösen, könnte dies in der
eingesetzten Hardware begründet sein. Es scheint, als wenn einige
günstige Arduino-Mega-Clones zeitweise unzuverlässig arbeiten und
diffuse Probleme verursachen. Abhilfe könnte ein Austausch des Arduino
schaffen, der Einsatz eines originalen Arduino ist selbstverständlich
eine weitere Option.

Ein Nutzer berichtete von erfolgreichen Änderungen an der
Adapter-Hardware selbst, die er zur Eingrenzung des Problems vornahm.

Dieses Problem wird aktuell verfolgt und es wird aktiv nach einer Lösung
gesucht. Sollte sich der Austausch von Hardwarekomponenten des Adapters
bei solchen ‚Problem-Clones' als dauerhaft erfolgreich zeigen, so wird
dies kommuniziert und im Platinenlayout berücksichtigt werden.  
    
---
    


## 15.24 Ich finde keinen LPB- oder BSB-Anschluss, nur L-BUS und R-BUS?!

In diesem Fall schließe bitte den Adapter *NICHT* an und beachte das Kap. [3.3](kap03.md#33-hinweis-neue-modellgeneration---nicht-unterstützter-regler-von-brötje).  
    
---
    
## 15.25 Gibt es eine (W)LAN-Option für den Adapter?

Ja, s. Kap. [12.7](kap12.md#127-lan-optionen-für-den-bsb-lpb-lan-adapter).

## 15.26 Ich habe weitere Fragen, an wen kann ich mich wenden?
Das Beste wäre, wenn du dich dafür im FHEM-Forum
([https://forum.fhem.de/](https://forum.fhem.de/))
anmelden würdest, da dort speziell für diesen Adapter ein eigener Thread
existiert und sich dort eine nette und hilfsbereite Community findet.
Hier findet ein reger Austausch über die Hard- und Software statt,
Fragen werden meist zügig beantwortet und auf Updates wird hingewiesen.

Hier findest du den entsprechenden Thread:
[https://forum.fhem.de/index.php/topic,29762.0.html](https://forum.fhem.de/index.php/topic,29762.0.html)

Wenn du dich mit deinen Fragen vorstellst, gib uns bitte zuerst genaue
Informationen bzgl. des von dir verwendeten Heizungstyps, des Reglers,
des verwendeten Bus-Typs etc.

Wenn du den Adapter bereits erfolgreich angeschlossen und in Verwendung
hast, rufe bitte einmal /Q (`http://<IP-Adresse>/Q`) auf und schreibe die Ausgabe
zusätzlich mit in deine Beschreibung.

Prinzipiell kann man sagen: Lieber erst einmal zu viele Informationen,
als zu wenige.

Fragen, deren Antworten sich aus dem gründlichen Lesen dieses Handbuchs
ergeben, werden ab einem gewissen Punkt lediglich mit einem Verweis
hierauf beantwortet. Bitte bedenke, dass dies für jeden von uns nur ein
Hobby-Projekt ist.  
    
---  

[Further on to chapter 16](chap16.md)      
[Back to TOC](toc.md)   
