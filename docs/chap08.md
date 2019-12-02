[Back to TOC](toc.md)  
[Back to chapter 7](chap07.md)    
   
---      

# 8. URL Commands and Special Functions
Because the webinterface basically is just set 'on top' to achieve access without further programs like FHEM or openHAB it's possible in general to access the functions and parameters with external programs.  

*A short and printer friendly listing of the available URL commands is availabe [here](https://github.com/1coderookie/BSB-LPB-LAN_EN/raw/master/commandref/Cheatsheet_URL-commands_EN.pdf).*  
    
---
    
## 8.1 Listing and Description of the URL Commands
*Sorry, not yet translated.. :(*  
*Note:*  
The values and parameters in the following list of the URL commands must be written without the brackets. E.g.: URL command `/<x>` for the simple query of parameter 8700 = `/8700`.  
   
   

| URL Command           | Effect                                                                    |
|:----------------------|:------------------------------------------------------------------------------|
|  **/\<x\>**               | **Query value/setting of parameter \<x\>**  
|  **/\<x\>/<y\>/<z\>**     | **Query values/settings of parameters \<x\>, \<y\> and \<z\>**   
|  **/\<x\>-\<y\>**         | **Query values/settings of parameters \<x\> to \<y\>**  
|  **/A**                   | **Query 24h average values** <br /> Zeigt rollierende 24h-Durchschnittswerte ausgewählter Parameter an. Die Initiale Festlegung dieser Parameter erfolgt in der Datei *BSB\_lan\_config.h* in der Variable `avg_parameters`.  
|  **/A=\<x\>,\<y\>,\<z\>**       | **Change 24h average value calculation of parameters \<x\>, \<y\>, \<z\>** <br /> Während der Laufzeit kann `/A=[parameter1],...,[parameter20]` verwendet werden, um (bis zu 20) neue Parameter zu definieren.  
|  **/B**                   | **Query accumulated burner-runtimes (in seconds) and -cycles (including DHW)** <br /> Fragt sowohl die akkumulierte Brennerlaufzeit (in Sekunden) und die Brennerstarts/-takte als auch die Anzahl und die Dauer der Ladungen (in Sekunden) des Trinkwasserspeichers ab, die anhand von Broadcast-Nachrichten ermittelt wurden. <br /> Bei bestimmten zweistufigen Ölbrennern wird zudem zwischen Stufe 1 und 2 differenziert und die jeweiligen Starts und Laufzeiten werden angezeigt.  
|  **/B0**                  | **Reset counter of burner-runtime and -cycles**  
|  **/C**                   | **Display configuration of BSB-LAN**  
|  **/D**                   | **Display logfile from the microSD-card** <br /> Zeigt den Inhalt der Datei *datalog.txt* an, die sich auf der microSD-Karte im Slot des Ethernet-Shields befindet. 
|  **/DG**                  | **Graphical display of the logfile from microSD-card** <br /> Wer Parameter auf SD-Karte loggt, hat neben der reinen Textform auch die Möglichkeit, einen Graphen angezeigt zu bekommen. <br /> *Hinweis:* Für `/DG` muss bei Javascript-Blockern die Domain d3js.org freigegeben werden, da der Arduino weiterhin nur die CSV-Datei in den Browser lädt und diese dann mit dem D3-Framework grafisch aufbereitet wird. <br /> Wird die Log-Datei via Webinterface mittels Klick auf „Anzeige Logdatei" aufgerufen, erfolgt standardmäßig zuerst die grafische Darstellung.    
|  **/D0**                  | **Reset logfile & create new header**  
|  **/E\<x\>**              | **Display ENUM-values of parameter \<x\>** <br /> At this command the adapter doesn't communicate with the controller, it's a software sided internal function of BSB-LAN. This command is only available for parameters of the type VT\_ENUM.  
|  **/G\<x\>**              | **GPIO: Query pin \<x\>** <br /> Gibt den momentanen Status von GPIO Pin \<x\> an, wobei \<y\>=0 *LOW* und \<y\>=1 *HIGH* ist.  
|  **/G\<x\>,\<y\>**        | **GPIO: Set pin \<x\> to high (\<y\> = 1) or low (\<y\> = 0)** <br /> Setzt GPIO Pin \<x\> auf  \<y\>=0 *LOW* oder \<y\>=1 *HIGH*. <br /> Reservierte Pins, die nicht gesetzt werden dürfen, können in der *BSB\_lan\_config.h* unter dem Parameter GPIO\_exclude gesperrt werden. 
|  **/G\<x\>,I**            | **GPIO: Query pin \<x\> while setting to INPUT** <br /> Für die reine Abfrage eines externes Gerätes, das an einen GPIO angeschlossen ist (z.B. ein einfaches Koppelrelais), da die Pins per default auf ‚output' gesetzt sind. Der Pin bleibt nach diesem Befehl so lange auf ‚input', bis das nächste Mal mit `/G<xx>=<y>` ein Wert geschrieben wird - ab da ist er dann bis zum nächsten „I" wieder auf ‚output'.  
|  **/I\<x\>=\<y\>**        | **Send INF-message to parameter \<x\> with value \<y\>** <br />  Einige Werte können nicht direkt gesetzt werden. Das Heizungssystem wird mit einer TYPE\_INF-Nachricht informiert, bspw. bei der Raumtemperatur: `http://<ip-address>/I10000=19.5` → Raumtemperatur beträgt 19.5°C  
|  **/JK=\<x\>**         	| **JSON: Query all parameters of category \<x\>**  
|  **/JK=ALL**          	   | **JSON: List all categories with corresponding parameter numbers**  
|  **/JQ=\<x\>,\<y\>,\<z\>**      | **JSON: Query parameters \<x\>, \<y\> und \<z\>**  
|  **/JQ**                  | ***→ with JSON-structure (see [manual](https://1coderookie.github.io/BSB-LPB-LAN_EN/chap08.html#824-retrieving-and-controlling-via-json)) via HTTP-POST request:* Query parameters**
|  **/JS**                  | ***→ with JSON-structure (see [manual](https://1coderookie.github.io/BSB-LPB-LAN_EN/chap08.html#824-retrieving-and-controlling-via-json)) via HTTP-POST request:* Set parameters**
|  **/K**                   | **List all categories** <br /> At this command the adapter doesn't communicate with the controller, it's a software sided internal function of BSB-LAN.  
|  **/K\<x\>**              | **Query all parameters and values of category \<x\>** <br /> At this command the adapter doesn't communicate with the controller, it's a software sided internal function of BSB-LAN.  
|  **/L=0,0**               | **Deactivate logging to microSD-card temporarily** <br /> Prinzipiell erfolgt das Aktivieren/Deaktivieren der Log-Funktion durch das entsprechende Definement in der Datei *BSB\_lan\_config.h* vor dem Flashen. Während des Betriebes kann das Loggen jedoch mit diesem Befehl deaktiviert werden. Zum Aktivieren werden dann wieder das Intervall und die gewünschten Parameter eingetragen. Bei einem Reset/Neustart des Arduino werden die Einstellungen aus der Datei *BSB\_lan\_config.h* verwendet - eine dauerhafte Umstellung der Logging-Parameter sollte also dort erfolgen.  
|  **/L=\<x\>,\<y1\>,\<y2\>,\<y3\>**       | **Set logging interval to \<x\> seconds with (optional) logging parameter \<y1\>,\<y2\>,\<y3\>** <br /> Setzt während der Laufzeit das Logging-Intervall auf \<x\> Sekunden und (optional) die Logging-Parameter auf \<y1\>, \<y2\>, \<y3\> etc. <br /> Dabei sind stets alle zu loggenden Parameter anzugeben - also auch (falls gewünscht) diejenigen, die evtl. bereits in der Datei *BSB_lan_config.h* definiert wurden. Nach einem Neustart werden dann wieder nur die Parameter geloggt, die in der Datei *BSB_lan_config.h* definiert wurden. <br /> *Hinweis:* Das Logging muss durch das Definement `#define LOGGING` in der Datei *BSB\_lan\_config.h* aktiviert werden und kann initial anhand der Variablen `log_parameters` und `log_interval` konfiguriert werden.  
|  **/LB=\<x\>**            | **Configure logging of bus-telegrams: only broadcasts (\<x\>=1) or all (\<x\>=0)** <br /> Wenn Bus-Telegramme geloggt werden (Parameter 30000 als einzigen Parameter loggen), logge nur die Broadcasts (\<x\>=1) oder alle (\<x\>=0) Telegramme.  
|  **/LU=\<x\>**            | **Configure logging of bus-telegrams: only unknown (\<x\>=1) or all (\<x\>=0)** <br /> Wenn Bus-Telegramme geloggt werden (Parameter 30000 als einzigen Parameter loggen), logge nur die unbekannten Command IDs (\<x\>=1) oder alle (\<x\>=0) Telegramme.  
|  **/M\<x\>**              | **Activate (\<x\> = 1) or deactivate (\<x\> = 0) bus monitor mode** <br /> Standardmäßig ist der Monitor-Modus deaktiviert (\<n\>=0). <br /> Wenn \<n\> auf 1 gesetzt wird, werden alle Bytes auf dem Bus überwacht. Telegramme werden durch Umbruchzeichen als solche erkannt. Jedes Telegramm wird im Hex-Format auf der seriellen Konsole mit einem Zeitstempel in Millisekunden dargestellt. Die Ausgabe der Überwachung betrifft nur die serielle Konsole des Arduino, die html-Ausgabe bleibt unverändert. <br /> Zum Deaktivieren des Monitor-Modus ist \<n\> wieder auf 0 zu setzen (URL-Befehl: `/M0`).  
|  **/N**                   | **Reset & reboot arduino (takes approx. 15 seconds)** <br /> Reset and reboot of the Arduino. <br /> *Note:* Function must be activated in *BSB\_lan\_config.h*: `#define RESET`!  
|  **/NE**                  | **Reset & reboot arduino (takes approx. 15 seconds) and erase EEPROM** <br /> Reset and reboot of the Arduino with additional erasing of the EEPROM. <br /> *Note:* Function must be activated in *BSB\_lan\_config.h*: `#define RESET`!  
|  **/P\<x\>**              | **Set bus type/protocol (temporarily): \<x\> = 0 → BSB \| 1 → LPB \| 2 → PPS** <br />    Wechselt zwischen BSB (\<x\>=0), LPB (\<x\>=1) und PPS (\<x\>=2). Nach einem Reset/Neustart des Arduino wird die Einstellung aus der Datei *BSB\_lan\_config.h* verwendet. Um den Bus-Typ dauerhaft festzulegen, sollte die Option `setBusType config` in der Datei *BSB\_lan\_config.h* entsprechend angepasst werden.     
|  **/P\<x\>,\<y\>,\<z\>**  | **Set bus type/protocol \<x\>, own address \<s\>, target address \<d\> (temporarily)** <br /> \<x\> = bus type (0 = BSB, 1 = LPB, 2 = PPS), <br /> \<y\> = own address and <br /> \<z\> = destination address <br /> Empty values Leerwerte bei den Adressen belassen den bisherigen Wert (= Adresse).  
|  **/Q**                   | **Check for unreleased controller-specific parameters**  
|  **/R\<x\>**              | **Query reset-value of parameter \<x\>** <br /> Im Display der integrierten Heizungssteuerung gibt es für einige Parameter eine Reset-Option. Ein Reset wird vorgenommen, indem das System nach dem Reset-Wert gefragt wird und dieser anschließend gesetzt wird.  
|  **/S\<x\>=\<y!z\>**        | **Set value \<y\> for parameter \<x\> with optional destination address \<z\>** <br /> Die gewünschte Gerätezieladresse ist als \<z\> einzufügen, wenn \<!z\> nicht eingegeben wird, wird die Standardzieladresse verwendet. Um einen Parameter auf \'abgeschaltet/deaktiviert\' zu setzen, muss lediglich ein leerer Wert eingefügt werden: `http://<ip-address>/S<x>=`.  
|  **/T**                   | **Query optional sensors (DS18B20/DHT22)** <br /> Gibt die jeweiligen Werte von optional angeschlossenen Sensoren aus. Bei DS18B20-Sensoren wird die spezifische SensorID und die Temperatur angezeigt, bei DHT22-Sensoren die Temperatur sowie die relative und absolute Luftfeuchtigkeit.  
|  **/V\<x\>**              | **Activate (\<x\> = 1) or deactivate (\<x\> = 0) verbose output mode** <br /> Der voreingestellte Verbositäts-Level ist 1. Somit wird standardmäßig der Bus überwacht und alle Daten werden zusätzlich im Raw-Hex-Format dargestellt. <br /> Soll der Modus deaktivert werden, so ist \<n\> auf 0 zu setzen (URL-Befehl: `/V0`). <br /> Der Verbositäts-Level betrifft sowohl die serielle Konsole des Arduino als auch (optional) das Loggen der Bus-Daten auf die microSD-Karte, so dass die Speicherkarte u.U. sehr schnell voll wird! Im Fall des Loggens auf die interne microSD-Karte ist es daher empfehlenswert, den Verbostitätsmodus bereits in der Datei *BSB\_lan\_config.h* zu deaktivieren (byte verbose = 0). <br /> Die html-Ausgabe bleibt mit /V1 unverändert.  
|  **/X**                   | **Query optional MAX!-thermostats** <br /> Queries and displays the temperatures of optional MAX!-thermostats. <br /> *Note:* MAX!-components have to be defined in *BSB\_lan\_config.h* before!  
   

---
    
## 8.2 Special Functions
    
---
    
### 8.2.1 Transmitting a Room Temperature
*Sorry, not yet translated.. :(*  

Mittels einer INF-Nachricht kann eine Raumtemperatur an den Regler
gesendet werden, um einen Raumeinfluss bei der Berechnung der
VL-Temperatur geltend zu machen.  
Um die Funktion zu nutzen, muss BSB-LAN Schreibzugriff gewährt werden (s. [Kap. 5](https://github.com/1coderookie/BSB-LPB-LAN/blob/master/docs/kap05.md)).  
   
Für die Raumtemperatur HK1 ist der Spezialparameter 10000, für den
HK2 der Parameter 10001 zu nutzen.

***Beispiel:***  
*Der URL-Befehl für den HK1, um eine Raumtemperatur von
19.5°C zu übermitteln, lautet: `http://<IP-Adresse>/I10000=19.5`*

***Hinweis:***  
*Um diese Funktion zu nutzen, muss die Funktion ‚Raumeinfluss' vorher im
Regler aktiviert und der Einflussfaktor prozentual festgelegt werden
(Parameter 750 für HK1, Parameter 1050 für HK2).  
Wird nur ein Temperaturwert als Einflussfaktor gemessen und übermittelt,
ist die Temperaturmessung in einem Führungs- / Referenzraum zu
empfehlen, in dem sich keinerlei weitere Wärmequelle (bspw. Kaminofen,
große Fenster in Südlage etc.) befindet.*  
    
***Note: Room Influence Regarding the Room Temperature***   
*FHEM forum user "freetz" has decoded the model behind the "room influence" (parameter 750), so that the effects on the flow temperature became more clear. Thanks a lot for this!*  
His article as well as an Excel spreadsheet can be found [here](https://forum.fhem.de/index.php/topic.29762.msg754102.html#msg754102).
    
---
    
### 8.2.2 Simulating the Presence Function
Die Funktion der Präsenztaste ist mit dem Spezialparameter 701 (für HK1)
und 1001 (für HK2) implementiert und als SET-Befehl auszuführen. Die
genannten Parameter müssen schreibbar sein (s. Kap. [5](kap05.md)). Der Parameter (701) ist NICHT abrufbar.

Bei aktivem Automatikprogramm ist dabei `http://<IP-Adresse>/S701=1` für
den Wechsel auf ‚Betriebsart Reduziert' und `http://<IP-Adresse>/S701=0`
für den Wechsel auf ‚Betriebsart Komfort' zu setzen.  
Der jeweilige Wechsel ist bis zur nächsten Betriebsart-Umschaltung laut
Zeitprogramm gültig. ***Die Präsenztaste ist nur im Automatikbetrieb wirksam!***

    
---
    
### 8.2.3 Triggering a Manual DHW-Push
*Sorry, not yet translated.. :(*  

Bei einigen Reglern ist die (nahezu undokumentierte) Funktion eines
manuellen Trinkwasser-Pushs verfügbar. Um einen manuellen TWW-Push
auszulösen, muss dazu die TWW-Taste an der ISR-Bedieneinheit gedrückt
und für etwa drei Sekunden gehalten werden, bis im Display eine
entsprechende Meldung erscheint.

Bei einigen Reglern kann diese Funktion mittels eines SET-Befehls
erfolgen. Dieser lautet `http://<IP-Adresse>/S1601=1` - der
Spezialparameter 1601 muss dazu schreibbar sein (s. Kap. [5](kap05.md)).
    
---
    
### 8.2.4 Retrieving and Controlling via JSON
***Hinweis:***    
*Sorry, not yet translated.. :(*  

*Diese Funktion ist derzeit noch in der (Weiter-)Entwicklung,
es kann also noch Veränderungen hinsichtlich der Befehle und/oder
Funktionen geben!*

Parameterabfragen sowie das Setzen von Werten kann ebenfalls mittels
JSON erfolgen.

-   **Abfrage von Kategorien:**

    `http://<IP-Adresse>/JK=<xx>`  
    Abfrage einer spezifischen Kategorie (\<xx\> = Kategorienummer)

    `http://<IP-Adresse>/JK=ALL`  
    Abfrage aller Kategorien (samt Min. und Max.)

-   **Abfragen und Setzen von Parametern per HTTP POST:**

    Hierbei ist der Aufruf der URL  
    `http://<IP-Adresse>/JQ` für eine Abfrage und   
    `http://<IP-Adresse>/JS` für das Setzen von Parametern zu verwenden.

    Folgende Parameter sind dabei möglich:
    
    ```
    http://<IP-Adresse>/JQ
    Senden: "Parameter"
    Empfangen: "Parameter", "Value", "Unit", "DataType" (0 = Zahl, 1 = ENUM, 2 = Bit-Wert (Dezimalwert gefolgt von Bitmaske gefolgt von ausgewählter Option), 3 = Wochentag, 4 = Stunde/Minute, 5 = Datum/Uhrzeit, 6 = Tag/Monat, 7 = String, 8 = PPS-Uhrzeit (Wochentag, Stunde:Minute))  
    
    http://<IP-Adresse>/JS  
    Senden: "Parameter", "Value" (nur numerisch), "Type" (0 = INF, 1 = SET)  
    Empfangen: "Parameter", "Status" (0 = Fehler, 1 = OK, 2 = Parameter read-only)  
    ```   
      
    Die Abfrage mehrerer Parameter mit einem Befehl ist ebenfalls möglich:  
    Der Befehl `http://<IP-Adresse>/JQ=<x>,<y>,<z>` fragt die Parameter \<x\>, \<y\> und \<z\> ab.  
       
       
-   **Setzen von Parametern per Linux-Kommandozeile oder „[Curl for Windows](https://curl.haxx.se/windows/)“**   
    Exemplarisch am Parameter 700 (Betriebsart HK1) → Setzen auf 1 (automatisch):
    
    Linux-Kommandozeile:   
    ```
    curl -v -H "Content-Type: application/json" -X POST -d '{"Parameter":"700", "Value":"1", "Type":"1"}' http://<IP-Adresse>/JS
    ```

    Curl for Windows:   
    ```
    curl -v -H "Content-Type: application/json" -X POST -d "{\"Parameter\":\"700\", \"Value\":\"1\", \"Type\":\"1\"}" http://<IP-Adresse>/JS
    ```
    
---
    
### 8.2.5 Checking for Non-Released Controller Specific Command IDs
*Sorry, not yet translated.. :(*  

*Hinweis: Es ist empfehlenswert, diese Abfrage einmalig auszuführen.*

`http://<IP-Adresse>/Q`  

Diese Funktion geht alle Command IDs durch, die in der Datei
*BSB\_lan\_defs.h* hinterlegt sind und schickt diejenigen, die nicht für
den eigenen Reglertyp hinterlegt sind, als Anfrage-Parameter (Typ QUR,
0x06) an den Regler.  
Das passiert bei Parametern, bei denen bisher nur eine Command ID
bekannt ist, ständig und erzeugt die bekannten „error 7 (parameter not
supported)"-Fehlermeldungen.

Sobald aber mehr als eine Command ID bekannt ist, bleibt die bisherige
Command ID i.d.R. auf \"DEV\_ALL\", ist also für alle Regler der
Standard, und die neue Command ID wird erst einmal nur für die Therme
freigeschaltet, die diese Command ID gemeldet hat.

Da es aber auch genauso gut umgekehrt sein kann, dass die \"neue\"
Command ID der Standard ist, und die \"alte\" Command ID der Sonderfall,
geht /Q nun die Command IDs durch, die nicht dem eigenen Regler
zugewiesen sind. Häufig können dort noch eine Reihe \"neuer\" Parameter
freigeschaltet werden.

***Zur Info:***  
*Es wird hierbei immer nur eine Anfrage mit einer Command ID an den
Regler geschickt!  
Der Regler beantwortet diese entweder mit einer Fehlermeldung (Typ ERR,
0x08) oder einer Antwort mit einem Datenpaket (Typ ANS, 0x07).  
In keinem Fall werden dabei Werte gesetzt oder Reglereinstellungen
verändert! Dafür müsste ein ganz anderer Telegramm-Typ gesetzt werden
(entweder Typ SET, 0x03 oder Typ INF, 0x02) - das macht /Q explizit
nicht!*  

Wenn bereits alle Parameter für den Reglertyp bekannt und freigegeben
sind, sieht die auf `http://<IP-Adresse>/Q`
folgende Webausgabe exemplarisch so aus (*Anmerkung: Hier handelt es sich noch um die Ausgabe mit einer veralteten BSB-LAN-Version, eine Beispielausgabe mit der aktuellen BSB-LAN-Version folgt!*):
    
```
Gerätefamilie: 90  
Gerätevariante: 100  
Start Test...  
Test Ende.  
```
    
Eine entsprechende Webausgabe bei bisher nicht-freigegebenen Parametern
für den spezifischen Regler hingegen sieht exemplarisch so aus (*Anmerkung: Hier handelt es sich noch um die Ausgabe mit einer veralteten BSB-LAN-Version, eine Beispielausgabe mit der aktuellen BSB-LAN-Version folgt!*):
    
```
Gerätefamilie: 90  
Gerätevariante: 100  
Start Test...  
2214  
DC 86 00 0B 06 3D 0D 08 EB E7 3A  
DC 80 06 0E 07 0D 3D 08 EB 00 0F 00 1B 94 5024  
5024 Trinkwasserspeicher - TWW Schaltdifferenz 1 ein: error 7 (parameter not supported)  
DC 86 00 0B 06 3D 31 07 1D C8 19  
DC 80 06 0E 07 31 3D 07 1D 00 01 40 A6 94 6031  
6031 Konfiguration - Relaisausgang QX22 Modul 1: error 7 (parameter not supported)  
DC 86 00 0B 06 3D 05 07 86 E3 AE  
DC 80 06 0D 07 05 3D 07 86 00 00 2C 55 8314  
8314 Diagnose Erzeuger - Kesselrücklauftemperatur Ist: error 7 (parameter not supported)  
DC 86 00 0B 06 3D 11 05 1A 58 5A  
DC 80 06 0E 07 11 3D 05 1A 01 00 00 91 09  
Test Ende.  
```  
    
In diesem Fall sollte die Webausgabe bitte kopiert und im [FHEM-Forum](http://forum.fhem.de/index.php/topic,29762.0.html) oder via Email an Frederik oder mich (Ulf) gemeldet werden,
damit eine entsprechende Anpassung vorgenommen werden kann.  
        
---
    
### 8.2.6 Gas Fired Heaters: Activate Internal Gas Energy Measurement (if Available)
*Sorry, not yet translated.. :(*  

Bei einigen Gasthermen-Modellen (vermutlich nur mit Reglertyp LMS14 und LMS15) ist eine interne (überschlägige) Gasenergiezählung unter den Parametern 8378-8383 verfügbar. Diese ist jedoch i.d.R. ab Werk nicht aktiviert.  
Eine Aktivierung muss bei *Parameter 2550* (Menü "Kessel", Fachmann-Ebene) vorgenommen werden. (Wie immer bei Parametern in der Fachmann-Ebene sollte dies nur von einem Heizungsfachmann durchgeführt werden.)  
      
Des Weiteren kann die interne Gasenergiezählung mit einem Korrekturfaktor an die Messung des Gaszählers angepasst werden. Dieser Faktor steht ab Werk auf 1,0 ist unter *Parameter 2551* einzustellen. 
Der Faktor ist wie folgt (in etwa) zu errechnen:  

*Angezeigter Verbrauch des internen Zählers zu hoch*  
Angezeigter Verbrauch des Gaszählers (a): 5000kWh  
Angezeigter Verbrauch der internen Zählung (b): 5500kWh  
Berechnung: `a/b = Korrekturfaktor`  
Diesem Beispiel folgend: 5000kWh/5500kWh=0,90909. Einzustellen ist also 0,9 oder 0,91.  

*Angezeigter Verbrauch des internen Zählers zu niedrig*  
Angezeigter Verbrauch des Gaszählers (a): 1300kWh  
Angezeigter Verbrauch der internen Zählung (b): 1000kWh  
Berechnung: `b/a = Korrekturfaktor`  
Diesem Beispiel folgend: 1300kWh/1000kWh=1,3. Einzustellen ist also 1,3.  
    
Im Zuge der Aktivierung von 2550 sollte der *Parameter 1630* "TWW-Ladevorrang" auf 'absolut' eingestellt werden, da ansonsten bei einer TWW-Ladung mit gleichzeitiger Heizbetriebsanforderung der Verbrauch nur für den Zähler des Heizbetriebs berücksichtigt wird.  
    
***Hinweise:***  
    
*Bei der heizungsseitigen, internen Gasenergiezählung handelt es sich um eine überschlägige Berechnung, sie ist also nich so genau wie der angezeigte Verbrauch auf dem Gaszähler. Für einen Vergleich der beiden Verbrauchswerte und die daraus resultierende Einstellung des Korrekturfaktors sollte der Messzeitraum mindestens vier Wochen betragen. Auch danach sollten die Werte immer mal wieder verglichen und der Korrekturfaktor ggf. angepasst werden.*  
    
*Der Verbrauch des Gaszählers wird üblicherweise in Kubikmetern (m³) angezeigt, nicht in Kilowattstunden (kWh). Zur Berechnung der kWh aus den verbrauchten m³ muss folgende Formel angewendet werden:*  
`kWh = m³ x Brennwert x Zustandszahl`  
*Die m³ werden vom Gaszähler abgelesen, der Brennwert sowie die Zustandszahl sind i.d.R. auf der Gasrechnung vermerkt oder beim Energieversorger zu erfragen.*  
    
    
--- 
    
### 8.2.7 Changing the Date, Time and Time Programs
*Sorry, not yet translated.. :(*     
Das Verändern der Uhrzeit und der Zeitprogramme ist nur über einen speziellen URL-Befehl möglich, es ist *nicht* über das Webinterface möglich.  
Um die Funktion zu nutzen, muss BSB-LAN Schreibzugriff gewährt werden (s. Kap. [5](kap05.md)).  
  
*Datum und Uhrzeit verändern*  
Der folgende Befehl stellt das Datum auf den 04.01.2019 und die Uhrzeit auf 20:15 Uhr:  
`/S0=04.01.2019_20:15:00`  
Mit dieser Funktion ist es möglich, die Uhrzeit- und Datumseinstellungen bspw. mit einem NTP Zeitserver abzugleichen. 
   
*Zeitprogramme verändern*  
Der folgende Befehl setzt das Zeitprogramm für *Mittwoch* beim Heizkreis 1 (Parameter 502) auf 05:00-22:00 Uhr:  
`/S502=05:00-22:00_xx:xx-xx:xx_xx:xx-xx:xx`  
     
---  
   
### 8.2.8 Transmitting an Alternative Outside Temperature
*Sorry, not yet translated.. :(*     
Bei bestimmten Reglermodellen ist es möglich, diverse Funkkomponenten anzuschließen, u.a. auch einen Funk-Außentemperaturfühler. Mittels BSB-LAN ist es bei diesen kompatiblen Reglern möglich, dem Heizungsregler eine anderweitig ermittelte Außentemperatur (AT) zu übermitteln. Dies ist insbesondere für Nutzer komplexerer Hausautomationsinstallationen interessant, die bspw. eine Wetterstation an einem günstigeren Standort als dem des heizungsseitigen Außentemperaturfühlers installiert haben.  
   
Als kompatible Regler sind bisher einige Reglermodelle der Reihen [LMS](kap03.md#3212-lms-regler) und [RVS](kap03.md#3222-rvs-regler) gemeldet worden (Stand Oktober 2019). Ältere Reglergenerationen wie bspw. [LMU](kap03.md#3211-lmu-regler) oder [RVA](kap03.md#3221-rva--und-rvp-regler) sind anscheinend nicht kompatibel.  
   
Um zu testen, ob der eigene Regler kompatibel ist, kann -zusätzlich neben der Überprüfung des Reglertyps- im Vorfeld `<ip>/Q` oder gezielt ein Abruf der Parameter `<ip>/10003/10004` ausgeführt werden.  
Wenn als Rückmeldung bei Parameter 10003 die Außentemperatur (oder "---") angezeigt wird, so ist die Funktion nach bisherigem Kenntnisstand verfügbar.  
Wenn hingegen ein "error 7" gemeldet wird, so ist die Funktion leider nicht verfügbar.  
   
Im Zweifelsfall sollte einfach versucht werden, eine alternative AT wie nachfolgend beschrieben zu senden. Ein nachfolgender Abruf des Parameters 8700 gibt Aufschluss darüber, ob der zuvor gesendete Wert übernommen wurde.   
      
Für die Verwendung der Funktion der alternativen Außentemperaturübermittlung mittels BSB-LAN muss der kabelgebundene Außentemperaturfühler der Heizung zwingend vom Regler getrennt werden (da der Regler die alternative AT ansonsten scheinbar nicht annimmt). Die darauf folgende Fehlermeldung des Heizungsreglers "Fehler 10: Aussenfühler" scheint den Betrieb zwar nicht zu stören, kann/sollte aber abgeschaltet werden. Dazu führt man den Parameter 6200 "Fühler speichern" einmal aus (auf JA stellen und bestätigen). Soll der kabelgebundene Fühler irgendwann wieder zum Einsatz kommen, so sollte nach erfolgtem Anschluss erneut Parameter 6200 "Fühler speichern" (-> JA -> bestätigen) ausgeführt werden. Somit ist der kabelgebundene AT-Fühler wieder im Heizungsregler registriert.  
    
Der Funk-Außentemperaturfühler scheint die gemessene AT ca. minütlich zu übermitteln. Bleibt diese Meldung aus, so scheint der Regler nach etwa 10-11 Minuten auf einen intern hinterlegten Wert zurückzugreifen. Zusätzlich erscheint die o.g. Fehlermeldung erneut. Es ist also empfehlenswert, die alternative AT via BSB-LAN etwa alle ein bis zwei Minuten zu übertragen.  
   
Um die Funktion zu nutzen, muss BSB-LAN Schreibzugriff gewährt (s. Kap. [5](kap05.md)) und die AT mit dem Befehl  
`<ip>/I10003=xx`  
übermittelt werden, wobei xx die betreffende AT in °C ist. Nachkommawerte sind möglich, als Komma ist ein Punkt einzufügen.  
   
*Beispiel:*  
Mit `<ip>/I10003=16.4` wird dem Heizungsregler die AT von 16.4°C mitgeteilt; `<ip>/I10003=9` übermittelt 9°C AT.  
   
*Hinweis:*  
Wird nur bei Parameter 10004 die Außentemperatur angezeigt, so ist die Funktion nach bisherigem Kenntnisstand nicht verfügbar. Das Übermitteln der alternativen AT kann in diesem Fall aber trotzdem wie beschrieben getestet werden, allerdings muss dann der Parameter 10004 anstelle von 10003 verwendet werden: `<ip>/10004=xx`.  
   
---  
   
[Further on to chapter 9](chap09.md)      
[Back to TOC](toc.md)   



