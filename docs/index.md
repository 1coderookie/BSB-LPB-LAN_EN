## Preface

This manual was written to make the start and the usage of the BSB-LPB-LAN adapter (schematic layout v2, Arduino version) and the BSB-LAN software easier.  

***It is suggested to read the manual completely before starting the installation and usage of the adapter and the software.***    
    
---  
  
The copyright belongs to the author of this manual: Ulf Dieckmann.
  
---  
    
### Straight to the [TOC](toc.md)      
### [PDF version of this manual](https://github.com/1coderookie/BSB-LPB-LAN/raw/master/Handbuch_BSB-LPB-LAN-Adapter.pdf)     
### [Cheatsheet URL commands](https://github.com/1coderookie/BSB-LPB-LAN_EN/raw/master/Cheatsheet_URL-commands_EN.pdf)   


---  
2translate:
***ACHTUNG:  
Es gibt KEINE GARANTIE oder Gewährleistung jeglicher Art, dass dieser Adapter dein Heizungssystem NICHT beschädigt!  
Jegliche Umsetzung der hier beschriebenen Schritte, jeder Nachbau des Adapters sowie jede Verwendung der beschriebenen Hard- und Software erfolgt auf eigene Verantwortung und eigenes Risiko!  
Keiner der Mitwirkenden oder Autoren kann für etwaige Schäden jeglicher Art haftbar gemacht werden!***   

---
  
### BSB-LPB-LAN - ein kurzer Überblick

"BSB-LPB-LAN" ist ein gemeinschaftliches Hard- und Softwareprojekt, welches ursprünglich zum Ziel hatte, mittels PC/Laptop/Tablet/Smartphone Zugriff auf die Steuerungen bzw. Regler von verschiedenen Wärmeerzeugern (Öl- und Gasheizungen, Wärmepumpen, Solarthermie etc.) bestimmter Hersteller (initial hauptsächlich Brötje und Elco) zu erlangen.  
Im weiteren Verlauf sollte es dann möglich sein, Daten auszulesen, sie weiter zu verarbeiten (z.B. loggen und grafisch darstellen) oder gar Einfluss auf die Steuerung/Regelung nehmen zu können und das System in bestehende SmartHome-Systeme einzubinden.  
    
All dies ist mittlerweile umgesetzt worden:  
Mittels eines eigenbaufähigen Adapters, eines Arduino Mega 2560 und eines LAN-Shields kann nun ein entsprechender Wärmeerzeuger kostengünstig ins heimische Netzwerk eingebunden werden.  
Die interne Steuerung bzw. der Regler des Wärmeerzeugers muss dafür mit einem "Boiler System Bus" (BSB), einem "Local Process Bus" (LPB) oder einer "Punkt-zu-Punkt-Schnittstelle" (PPS) ausgestattet sein. Dies sind i.d.R. Systeme, bei denen ein SIEMENS-Regler zum Einsatz kommt (bzw. je nach Heizungshersteller meist eine gebrandete OEM-Version).

Mit Hilfe des Adapters und der BSB-LAN-Software können nun unkompliziert verschiedene Funktionen, Werte und Parameter beobachtet, geloggt und bei Bedarf web-basiert gesteuert und geändert werden.
Eine optionale Einbindung in bestehende SmartHome-Systeme wie bspw. FHEM, openHab, HomeMatic oder Loxone kann mittels HTTPMOD oder JSON erfolgen. 
Darüber hinaus ist der Einsatz des Adapters als Standalone-Logger ohne LAN- oder Internetanbindung bei Verwendung einer microSD-Karte ebenfalls möglich.  
Zusätzlich können Temperatur- und Feuchtigkeitssensoren angeschlossen und deren Daten ebenso geloggt und ausgewertet werden. Durch die Verwendung eines Arduino und die Möglichkeit, eigenen Code in die BSB-LAN-Software zu integrieren, bietet sich darüber hinaus ein weites Spektrum an Erweiterungsmöglichkeiten. 

    
Als erste grobe Orientierung, ob das eigene Heizungssystem komaptibel ist oder nicht, kann in der Bedienungsanleitung der Heizung nach einer Anschlussmöglichkeit für optionale Raumgeräte gesucht werden. Sind dort Raumgeräte des Typs QAA55/QAA75 als kompatibel aufgeführt (bei Brötje werden diese u.a. auch als "RGB Basic" und "RGT B Top" bezeichnet), so ist erfahrungsgemäß der Anschluss des Adapters via BSB möglich und der volle Funktionsumfang von BSB-LAN gegeben. Dies ist bei den meisten Öl-, Gas- und Wärmepumpensystemen der letzten Jahre der Fall.  
Sollten andere Raumgeräte aufgeführt sein, so kann im Kapitel "[Raumgeräte](docs/kap03.md#36-konventionelle-raumgeräte-für-die-aufgeführten-reglertypen)" im BSB-LPB-LAN-Handbuch nachgesehen werden.  
Genauen Aufschluss bietet letztlich aber immer nur die eigentliche Reglerbezeichnung.  
   
---

The following overview shows the most common used controllers of the different heating systems which will work with BSB-LAN. For further and more detailed informations about the different [controllers](chap03.md#32-detailed-listing-and-description-of-the-supported-controllers) and the [connection](chap02.md#23-connecting-the-adapter-to-the-controller) see the corresponding chapters in the [manual](https://1coderookie.github.io/BSB-LPB-LAN_EN).  
   
**Gas-fired heating systems controllers:**  
- [LMU74/LMU75](chap03.md#3211-lmu-controllers) and [LMS14/LMS15](chap03.md#3212-lms-controllers) (latest models), connection via BSB, complete functionality  
- [LMU54/LMU64](chap03.md#3211-lmu-controllers), connection via PPS, limited functionality  
   
**Oil-fired heating systems controllers / solarthermic controllers / zone controllers:**  
- [RVS43/RVS63/RVS46](chap03.md#3222-rvs-controllers), connection via BSB, full functionality  
- [RVA/RVP](chap03.md#3221-rva-and-rvp-controllers), connection via PPS (modelspecific sometimes LPB), limited functionality 
   
**Heat pump controllers:**  
- [RVS21/RVS61](chap03.md#3222-rvs-controllers), connection via BSB, full functionality  
   
**Weishaupt (model WTU):**  
- [RVS23](chap03.md#3222-rvs-controllers), connection via LPB, (nearly) full functionality  
   
**To see a more detailed listing of the reported systems which are sucessfully used with BSB-LAN please follow the corresponding link:**  
- **[Broetje](chap03.md#311-broetje)**  
- **[Elco](chap03.md#312-elco)**  
- **[Other Manufacturers (e.g. Fujitsu, Atlantic, Weishaupt)](chap03.md#313-other-manufacturers)**  

  
### The software is available [here](https://github.com/fredlcore/bsb_lan).

---  

### Authors:

-   Software, schematics v1, first documentation EN, support  
    up to v0.16:  
    *Gero Schumacher (gero.schumacher \[ät\] gmail.com)*

-   Software, PCB schematics v1 & v2, first documentation EN, support  
    since v0.17:  
    *Frederik Holst (bsb \[ät\] code-it.de)*

-   Debugging, manuals, translations, support  
    since v0.17:  
    *Ulf Dieckmann (adapter \[ät\] quantentunnel.de)*

*Based upon the code and the work of many other users and developers! Thanks!*  
      
    
---
    
[Further to the TOC](toc.md)  


