[Back to TOC](toc.md)  
[Back to chapter 7](chap07.md)    
   
---      

# 8. URL Commands and Special Functions
Because the webinterface basically is just set 'on top' to achieve access without further programs like FHEM or openHAB it's possible in general to access the functions and parameters with external programs.  

*A short and printer friendly listing of the available URL commands is availabe [here](https://github.com/1coderookie/BSB-LPB-LAN_EN/raw/master/commandref/Cheatsheet_URL-commands_EN.pdf).*  
    
---
    
## 8.1 Listing and Description of the URL Commands
*Note:*  
The values and parameters in the following list of the URL commands must be written without the brackets. E.g.: URL command `/<x>` for the simple query of parameter 8700 = `/8700`.  
   
   

| URL Command           | Effect                                                                    |
|:----------------------|:------------------------------------------------------------------------------|
|  **/\<x\>**               | **Query value/setting of parameter \<x\>**  
|  **/\<x\>/<y\>/<z\>**     | **Query values/settings of parameters \<x\>, \<y\> and \<z\>**   
|  **/\<x\>-\<y\>**         | **Query values/settings of parameters \<x\> to \<y\>**  
|  **/A**                   | **Query 24h average values** <br /> Shows the 24h average values of the parameters, which were defined before within the variable `avg_parameters` in the file *BSB\_lan\_config.h*.  
|  **/A=\<x\>,\<y\>,\<z\>**       | **Change 24h average value calculation of parameters \<x\>, \<y\>, \<z\>** <br /> During runtime up to 20 new parameters can be defined for the 24h average calculation. These parameters are kept until the next reboot of the Arduino.  
|  **/B**                   | **Query accumulated burner-runtimes (in seconds) and -cycles (including DHW)** <br /> These values are calculated by the incoming broadcasts from the controller. Due to maybe missed broadcast, these values should be considered as orientational values. <br /> Certain controllers of oil fired burners support a two stage counter, most of the controllers just show values for stage one.  
|  **/B0**                  | **Reset counter of burner-runtime and -cycles**  
|  **/C**                   | **Display configuration of BSB-LAN**  
|  **/D**                   | **Display logfile from the microSD-card** <br /> Displays the logfile *datalog.txt* which contains the values of the logged parameters defined in the file *BSB_lan_config.h*.
|  **/DG**                  | **Graphical display of the logfile from microSD-card** <br /> Shows graphical output (graphs) of the logged values. <br /> *Note:* If you use Javascript blockers, make sure you allow access to d3js.org, because the Arduino just loads the csv-file into the browser and the D3-framework converts the data.     
|  **/D0**                  | **Reset logfile & create new header** <br /> This command deletes the content of the file *datalog.txt* and creates a new csv-header. This command should be executed before first logging.     
|  **/E\<x\>**              | **Display ENUM-values of parameter \<x\>** <br /> At this command the adapter doesn't communicate with the controller, it's a software sided internal function of BSB-LAN. This command is only available for parameters of the type VT\_ENUM.  
|  **/G\<x\>**              | **GPIO: Query pin \<x\>** <br /> Displays the actual state of GPIO pin \<x\>, where \<y\>=0 is *LOW* and \<y\>=1 is *HIGH*.  
|  **/G\<x\>,\<y\>**        | **GPIO: Set pin \<x\> to high (\<y\> = 1) or low (\<y\> = 0)** <br /> Sets GPIO pin \<x\> to *LOW* (\<y\>=0) or *HIGH* (\<y\>=1). <br /> Reserved pins which shouldn't be allowed to be set can be defined previously at `GPIO_exclude` in the file *BSB\_lan\_config.h*. 
|  **/G\<x\>,I**            | **GPIO: Query pin \<x\> while setting to INPUT** <br /> If e.g. a coupling relay is connected to a GPIO pin and the state should just be queried, this command should be used. It sets the GPIO pin to input (default they are set to output) and keeps this as long until it's changed by using `/G<x>=<y>`. After that, it's set to output again until the next "I" sets it to input again.  
|  **/I\<x\>=\<y\>**        | **Send INF-message to parameter \<x\> with value \<y\>** <br /> Some values can't be set directly, the controller gets these values by a TYPE_INF-message. As an example, the room temperature of 19.5°C should be transmitted: `http://<ip-address>/I10000=19.5`.  
|  **/JK=\<x\>**         	| **JSON: Query all parameters of category \<x\>**  
|  **/JK=ALL**          	   | **JSON: List all categories with corresponding parameter numbers**  
|  **/JQ=\<x\>,\<y\>,\<z\>**      | **JSON: Query parameters \<x\>, \<y\> and \<z\>**  
|  **/JQ**                  | ***→ with JSON-structure (see [chapter 8.2.4](chap08.md#824-retrieving-and-controlling-via-json)) via HTTP-POST request:* Query parameters**
|  **/JS**                  | ***→ with JSON-structure (see [chapter 8.2.4](chap08.md#824-retrieving-and-controlling-via-json)) via HTTP-POST request:* Set parameters**
|  **/K**                   | **List all categories** <br /> At this command the adapter doesn't communicate with the controller, it's a software sided internal function of BSB-LAN.  
|  **/K\<x\>**              | **Query all parameters and values of category \<x\>** <br /> At this command the adapter doesn't communicate with the controller, it's a software sided internal function of BSB-LAN.  
|  **/L=0,0**               | **Deactivate logging to microSD-card temporarily** <br /> Prinzipiell erfolgt das Aktivieren/Deaktivieren der Log-Funktion durch das entsprechende Definement in der Datei *BSB\_lan\_config.h* vor dem Flashen. Während des Betriebes kann das Loggen jedoch mit diesem Befehl deaktiviert werden. Zum Aktivieren werden dann wieder das Intervall und die gewünschten Parameter eingetragen. Bei einem Reset/Neustart des Arduino werden die Einstellungen aus der Datei *BSB\_lan\_config.h* verwendet - eine dauerhafte Umstellung der Logging-Parameter sollte also dort erfolgen.  
|  **/L=\<x\>,\<y1\>,\<y2\>,\<y3\>**       | **Set logging interval to \<x\> seconds with (optional) logging parameter \<y1\>,\<y2\>,\<y3\>** <br /> This command can be used to change the logging interval and the parameters that should be logged during runtime. All parameters that should be logged have to be set. After a reboot of the Arduino, again only the parameters are logged which has been defined in *BSB_lan_config.h* initially.   
|  **/LB=\<x\>**            | **Configure logging of bus-telegrams: only broadcasts (\<x\>=1) or all (\<x\>=0)** <br /> When logging bus telegrams (log parameter 30000 as the only parameter), only the broadcast messages (\<x\>=1) or all telegrams (\<x\>=0) are logged.   
|  **/LU=\<x\>**            | **Configure logging of bus-telegrams: only unknown (\<x\>=1) or all (\<x\>=0)** <br /> When logging bus telegrams (log parameter 30000 as the only parameter), only unknown command ids (\<x\>=1) or all telegrams (\<x\>=0) are logged.  
|  **/M\<x\>**              | **Activate (\<x\> = 1) or deactivate (\<x\> = 0) bus monitor mode** <br /> By default bus monitor mode is deactivated (\<x\>=0). <br /> When setting \<x\> to 1, all bytes on the bus are monitored. Each telegram is displayed in hex format with a timestamp in miliseconds at the serial monitor. The html output isn't affected though. <br /> To deactivate the monitor mode, set \<x\> back to 0: `/M0`.  
|  **/N**                   | **Reset & reboot arduino (takes approx. 15 seconds)** <br /> Reset and reboot of the Arduino. <br /> *Note:* Function must be activated in *BSB\_lan\_config.h*: `#define RESET`!  
|  **/NE**                  | **Reset & reboot arduino (takes approx. 15 seconds) and erase EEPROM** <br /> Reset and reboot of the Arduino with additional erasing of the EEPROM. <br /> *Note:* Function must be activated in *BSB\_lan\_config.h*: `#define RESET`!  
|  **/P\<x\>**              | **Set bus type/protocol (temporarily): \<x\> = 0 → BSB \| 1 → LPB \| 2 → PPS** <br />    Changes between BSB (\<x\>=0), LPB (\<x\>=1) and PPS (\<x\>=2). After a reboot of the Arduino, the initially defined bus type will be used again. To change the bus type permanently, adjust the setting `setBusType config` in *BSB\_lan\_config.h* accordingly.     
|  **/P\<x\>,\<y\>,\<z\>**  | **Set bus type/protocol \<x\>, own address \<y\>, target address \<z\> (temporarily)** <br /> Temporarily change of the set bus type and addresses: <br /> \<x\> = bus type (0 = BSB, 1 = LPB, 2 = PPS), <br /> \<y\> = own address and <br /> \<z\> = destination address <br /> Empty values leave the address as it is already set.  
|  **/Q**                   | **Check for unreleased controller-specific parameters**  
|  **/R\<x\>**              | **Query reset-value of parameter \<x\>** <br /> Within the integrated operational unit of the heating system there are reset options available for some parameters. A reset is done by asking the system for the reset value and setting it afterwards.  
|  **/S\<x\>=\<y!z\>**        | **Set value \<y\> for parameter \<x\> with optional destination address \<z\>** <br /> Command for setting values (therefore, write-access must be defined previously in *BSB_lan_config.h*!). Additionally a destination address can be set by using \<z\>. If \<!z\> isn't used, the standard destination address will be used. <br /> To set a parameter to 'off/deactivated', just use an empty value: `http://<ip-address>/S<x>=`.  
|  **/T**                   | **Query optional sensors (DS18B20/DHT22)** <br /> Queries the values of the optional sensors. <br /> DS18B20: the specific sensor id and the measured temperature are displayed. <br /> DHT22: the measured temperature, the relative and the absolute humidity are displayed.  
|  **/V\<x\>**              | **Activate (\<x\> = 1) or deactivate (\<x\> = 0) verbose output mode** <br /> The preset verbosity level is 1, so the bus is 'observed' and all data are displayed in raw hex format additionally. <br /> If the mode should be deactivated, \<x\> has to be set to 0: `/V0` <br /> Verbosity mode affects the output of the serial monitor as well as the (optional) logging of bus data to microSD card. Therefore the card could run out of space quickly, so it's advisable to deactivate the verbosity mode already in the *BSB_lan_config.h*: `byte verbose = 0` <br /> The html output isn't affected by /V1.  
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



