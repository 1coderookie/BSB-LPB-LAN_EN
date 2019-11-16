[Back to TOC](toc.md)  
[Back to chapter 12](chap12.md)    
   
---      
    
# 13. Etwaige Fehlermeldungen und deren mögliche Ursachen
*Sorry, not yet translated.. :(*  
    
---
    

## 13.1 Fehlermeldung „unknown type \<xxxxxxxx\>"
*Sorry, not yet translated.. :(*  

Dieser Fehler sagt aus, dass für diesen Parameter keine
Umrechnungsanweisung vorliegt, um die Rohdaten in eine entsprechende
Einheit (Zeit, Temperatur, Prozent, Druck etc.) umzuwandeln.

Um den Fehler zu beheben, sollte das jeweilige Telegramm / die Command
ID des betreffenden Parameters sowie der zugehörige Wert ausgelesen und
gemeldet werden. Sollten mehrere Einstellungsoptionen für einen
Parameter verfügbar sein, muss zusätzlich jede Option ausgelesen werden,
damit eine eindeutige Zuordnung stattfinden kann.  
    
---
    

## 13.2 Fehlermeldung „error 7 (parameter not supported)"
*Sorry, not yet translated.. :(*  

Die zugehörige Command ID wird nicht erkannt oder der entsprechende
Parameter wird vom Regler nicht unterstützt (bspw. spezifische
Parameter, die eine Gasheizung betreffen und bei einer Ölheizung
dementsprechend nicht verfügbar sind).

Fehlermeldungen dieses Typs werden seit v0.41 der Übersichtlichkeit
halber per default ausgeblendet (die entsprechenden Parameter bspw. bei
einer Komplettabfrage aber dennoch abgefragt). Möchtest du sie dennoch
angezeigt bekommen, so ist das entsprechende Definement `#define
HIDE_UNKNOWN` in der Datei *BSB\_lan\_config.h* auszukommentieren
(`//#define HIDE_UNKNOWN`).

Zur Überprüfung, ob die CommandID vom Regler prinzipiell unterstützt
wird, jedoch für diese Gerätefamilie nicht freigegeben ist, aktiviere
bitte vorübergehend das Definement `#define DEBUG` in der Datei
*BSB\_lan\_config.h* und führe dann den Befehl /Q aus (s. hierzu auch Kap. [8.2.5](kap08.md#825-überprüfen-auf-nicht-freigegebene-reglerspezifische-command-ids)).\
Bei diesem Befehl werden trotz aktivem ‚HIDE\_UNKNOWN'-Definement etwaige error7-Fehlermeldungen angezeigt.  
    
---
    

## 13.3 Fehlermeldung „query failed"
*Sorry, not yet translated.. :(*  

Diese Meldung erscheint, wenn auf die Anfrage des Adapters keine
(sinnvolle) Antwort des Reglers kommt.

Mögliche Ursachen sind meist hardwareseitig zu suchen (bspw. fehlerhafte
RX- und/oder TX-Verbindung, falsch verbaute Komponenten oder auch ein
timeout aufgrund eines ausgeschalteten oder nicht angeschlossenen
Reglers).  
    
---
    

## 13.4 Fehlermeldung „FEHLER: Setzen fehlgeschlagen! - Parameter ist nur lesbar"
*Sorry, not yet translated.. :(*  

Diese Meldung erscheint bei dem Versuch, Werte zu schreiben bzw. zu
übermitteln (bspw. die Raumtemperatur) oder Parameter zu verändern,
während der Zugriff des Adapters nur auf Lesen beschränkt ist
(`FL_RONLY`).  
Der gewünschte Parameter (oder auch generell alle Parameter) muss in
diesem Fall als schreibbar definiert werden. Die hierfür notwendige
Vorgehensweise ist in Kap. [5](chap05.md) (vorletzter Punkt) ausführlich beschrieben.
     
---  

[Further on to chapter 14](chap14.md)      
[Back to TOC](toc.md)   

