[Back to TOC](toc.md)  
[Back to chapter 14](chap14.md)    
   
---   
    
    
# 15. FAQ 

## 15.1 Can I Use the Adapter & Software with a Raspbery Pi?

Yes and no.  
The adapter itself can be used in conjuction with a Raspberry Pi 2, if you make certain adjustments and add certain parts. Please see the following chapters for further informations: [chap. 12.9](chap12.md#129-raspberry-pi-2), [appendix a1](appendix_a1.md) and [appendix a2.2](appendix_a2.md#a22-parts-list). 

The BSB-LAN software can NOT be used with a RPi, it is only usable with the described Arduino. Further informations are available in [chap. 12.9](chap12.md#129-raspberry-pi-2).  
    
---
    

## 15.2 Can I Connect One Adapter to Two Controllers at the Same Time?

No, this isn't possible. If you want to connect the hardware setup (arduino, ethernet-shield, adapter) to the BSB of the controllers, you have to use one hardware setup for each controller. If the contollers are already connected via LPB though, please see the following FAQ.  
    
---
    
## 15.3 Can I Connect an Adapter via LPB And Query Different  Controllers?
Yes, if the existing controllers are already connected with each other via LPB. This LPB setup of the controllers already  has to work without any problems, so the setup has to be done correctly (e.g. device and segment addresses have to be set right).  
For querying data of each controller, the specific address has to be set within BSB-LAN. See chapter [8.1](chap08.md#81-listing-and-description-of-the-url-commands) for further informations.  
    
---
    

## 15.4 Is a Multifunctional Input of the Controller Directly Switchable via Adapter?

No!

The multifunctional inputs of the controllers (e.g. H1, H2, H3 etc.) are not connectable directly to the adapter/arduino!  
  
These inputs must be switched potential free, so you have to use a relay in conjunction with the adapter/arduino. See [chapter 12.4](chap12.md#124-relays-and-relayboards) for further informations.  
    
---
    
## 15.5 Can an Additional Relayboard Be Connected And Controlled by the Ardunio?

Yes. See [chapter 12.4](chap12.md#124-relays-and-relayboards) for further informations.  
    
---
    
## 15.6 Can I Query the State of a Connected Relay?

Yes. See the specific URL command in chapter [8.1](chap08.md#81-listing-and-description-of-the-url-commands).  
    
---
    

## 15.7 Can I Be Helpful to Add Yet Unknown Parameters?

Yes! Please see chapter [10](chap10.md) for further instructions.  
    
---
    

## 15.8 Why Do Some Parameters Appear Doubly Within a Complete Query?

When you do a complete query of all parameters via URL command (`http://<ip-address>/0-10000`) it can happen, that some parameters or program numbers are displayed doubly in the output. This happens because the same command id can occur within different parameters. Anyway, this doesn't have any negative influence the functionality in any way.   
    
---
    
## 15.9 Why Aren't Certain Parameters Displayed Sometimes?

First of all, not all of the parameters which are known by BSB-LAN are available within every type of controller. Certain parameters which are controller / heating system specific just aren't available. E.g.: specific paramaters of a gas-fired heating system aren't available within an oil-fired system.  
  
Besides that, if the controller has been powered on after the arduino already started, the automatic detection of the connected controller doesn't work. In this case just restart the arduino, so that the connected controller can be recognized by BSB-LAN.  

If still certain parameters don't appear which are available via the operational unit of the heating system, please do a query of /Q (see chapter [8.2.5](chap08.md#825-checking-for-non-released-controller-specific-command-ids)).  
If the desired parameters still don't appear there (reported as 'error 7' parameters), please follow the instructions in chapter [10](chap10.md).    
    
---
    
## 15.10 Why Isn't Access to Connected Sensors Possible?
If you connected DHT22 and/or DS18B20 sensors correctly to the adpater/arduino but the corresponding link in the webinterface doesn't work, you probably didn't adjust the belonging parameter in the file *BSB\_lan\_config.h*.  
See the description in the file *BSB\_lan\_config.h* and the chapter [12.3](chap12.md#123-usage-of-optional-sensors-dht22-and-ds18b20).  
    
---
    
## 15.11 I'm Using a W5500 LAN-Shield, What Do I Have to Do?

Make sure you are using the latest ethernet library within the Arduino IDE (min. version 2.0).   
    
---
    
## 15.12 Can States Or Values Be Sent As Push-Messages?

No, not by only using BSB-LAN. For this, you have to use additional software (e.g. FHEM) to query the desired parameters and process the data.   
    
---
   
## 15.13 Can (e.g.) FHEM 'Listen' to Certain Broadcasts?

Well, FHEM can 'listen' - but BSB-LAN can't send any messages by itself.   
    
---
    

## 15.14 Why Sometimes Timeout Problems Occur Within FHEM?

This could be due to the length of the send / receive process. You should calculate the timeout value in FHEM in the way, that you estimate approx. two seconds for a query and one second for a setting of a parameter.  

Besides that it could be helpful to put as many BSB-LAN specific readings as possible in one group for a query, so that collisions could be avoided.   
    
---
    
## 15.15 Is There a Module For FHEM?

Yes and no. The member *„justme1968"* develops a module:
[https://forum.fhem.de/index.php/topic,84381.0.html](https://forum.fhem.de/index.php/topic,84381.0.html)  
But: the development isn't done yet, so that an unproblematic and reliable usage can't be guaranteed at this time.  
    
---
    
## 15.16 Why Aren't Any Values Displayed At Stage 2 Within /B?

If you are using a gas-fired heating system, it's most likely modulating and doesn't even have a two-staged burning system. These systems mostly are only available within oil-fired heating systems. But even within these systems not all controllers spread the differentiation between stage one and two over the bus by broadcast messages, so that BSB-LAN doesn't recognize either it's stage one or two which is active.  
    
---
    

## 15.17 It Appears to Me That the Displayed Values of /B Aren't Correct.

This could be. The specific starts and runtimes are determined by the detection of the belonging broadcasts sent by the controller. Sometimes it can occur that a broadcast doesn't reach the other bus members, e.g. when a query is done at the same time.     
    
---
    
## 15.18 What Is the Exact Difference Between /M1 and /V1?
*Sorry, not yet translated.. :(*
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
    
## 15.19 Can I Implement My Own Code In BSB-LAN?

Yes, for this you can and should use the designated file *BSB\_lan\_custom.h*. Here you can add own code which will be initiated at each loop function.  
    
---
    
## 15.20 Can I Integrate MAX! Thermostats?

Yes, that's possible. You have to activate and adjust the specific definement in the file *BSB\_lan\_config.h*.  
For further informations see the corresponding chapter [12.5](chap12.md#125-max-components).  
    
---
    
## 15.21 Why Isn't the Adapter Reachable After a Power Failure?

This behaviour was noticed sometimes with cheap clones of the lan shields. Just press the reset button of the arduino, after that everything should work fine again. If this happens more often at your home, you can probably add a little emergency power supply to prevent this. 
    
---
    
## 15.22 Why Isn't the Adapter Reachable Sometimes (Without a Power Failure)?

This problem only occured in rare cases, there is no clear solution for this behaviour. The only solution was a reset and reboot of the arduino.  
   
---
    
## 15.23 Why Do 'Query Failed' Messages Occur Sometimes?

If this occurs within a system which is usually working fine, then it could probably be due to hardware issues. Some cheap arduino clones produce unspecific problems. Therefore you should try another arduino clone or buy an original arduino.
    
---
    
## 15.24 I Don't Find a LPB or BSB Connector, Only L-BUS And R-BUS?!

In this case you have a controller which isn't compatible with BSB-LAN - please DON'T try to connect the adapter!  
You can see chapter [3.3](chap03.md#33-new-type-not-supported-controller-from-broetje) for further informations.  
    
---
    
## 15.25 Is There An Alternative Besides Using LAN?

Yes, please see chapter [12.7](chap12.md#127-lan-options-for-the-bsb-lpb-lan-adapter).

## 15.26 I Have Further Questions, Who Can I Contact?

The best option is to create an account at the german FHEM forum ([https://forum.fhem.de/](https://forum.fhem.de/)) and ask your questions in the specific BSB-LAN thread: [https://forum.fhem.de/index.php/topic,29762.0.html](https://forum.fhem.de/index.php/topic,29762.0.html). 
    
---  

[Further on to chapter 16](chap16.md)      
[Back to TOC](toc.md)   
