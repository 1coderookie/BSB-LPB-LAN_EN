[Back to TOC](toc.md)  
[Back to appendix A1](appendix_a1.md)    
   
--- 
    
# Appendix A2: Notes on the Circuit Diagram  
    
---  

## A2.1 Short Explanation of the Circuit Diagram

D1 = LED (red)  
D2 = Diode  
EEPROM = EEPROM  
OK(x) = Optocouplers  
Q(x) = Transistor  
R(x) = Resistor  

ARD = Arduino  
RPI = Raspberry Pi  
  
CL+/- = BSB connectors  
DB/MB = LPB connectors  

TXD = Digital pin: transmit  
RXD = Digital pin: receive
    
---
        
## A2.2 Parts List

- 1x LED (red) (operating voltage max. 2,8V, reverse voltage 5V) (→ D1)  
- 1x Diode 1N4148 (→ D2)  
- 1x EEPROM 24LC32A-I/P (→ EEPROM) → *Note: Not needed for the ESP32 version of the PCB!*  
- 2x Optocouplers 4N25 (→ OK1, OK2)    
- 1x Transistor BC547 (→ Q1)  
- 1x Transistor BC557 (→ Q2)  
- 3x Resistor 330kΩ (→ R1, R4, R7) 
- 1x Resistor 1.5kΩ (→ R2) 
- 1x Resistor 300Ω (→ R3) 
- 2x Resistor 4.7kΩ (→ R5, R6)  
    

***Arduino Due:***  
Connectors, *pin header*, optional IC sockets for optocouplers and/or EEPROM..  
  
For the usage of the adapter v4 in conjunction with an *Arduino Due* you basically only need to assemble the pins for RX1, TX1, SDA, SCL, GND and pin 53. Other pins could be assembled due to a better stability and/or other usage.  
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/bsb-adapter-v4-unbestueckt_pins.jpeg">  
  
*Absolutely necessary pins for the usage in conjunction with an Arduino Due.*  
  
***Raspberry Pi:***  
Connectors, *female header*, optional IC sockets for optocouplers and/or EEPROM..  
  
For the usage of the adapter v4 in conjunction with a *Raspberry Pi* you have to put your attention on different things, which are collectively named within the [chapter 12.9](chap12.md#129-raspberry-pi).    
        
***ESP32:***  
Connectors, *pin header*, optional IC sockets for optocouplers..  
  
For the use of the ESP32 specific adapter v4 on the recommended *ESP32 NodeMCU from Joy-It* only the pins RX2, TX2, GND and 3.3V are needed and must be equipped with corresponding pin headers. However, for stability reasons it is recommended to equip both sides completely with one row of pin headers each.   
  
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN_EN/master/docs/pics/ESP32-PCB.jpeg">  
  
*The unpopulated ESP32 specific adapter board.*          
    
---
    
## A2.3 General Notes

***Important: Before soldering, study the circuit diagram attentively!***

The following instructions do not replace any fundamental
electronic knowledge, but maybe one or two of these hints could
be helpful for electronics beginners.

Generally it is helpful to position the components, bend the 'legs' of them a little bit to hold them in position on the board and check again the positioning of each component. Start with the smallest parts first. Pay attention to the correct alignment of parts like diodes, transistors and ICs! If everything seems fine, you can turn around the board and start soldering. Be careful that you just solder where you should so that you don't produce shortcuts by accident.  
A previous breadboard test setup (if you don't use the PCB) of course could be an option, though
due to non-exclude problem sources (usage of a wrong
plug row, possible loose contacts and so on) not necessarily
recommended.  

Please make sure that the components do not get too hot during soldering,
since they can be damaged. Therefore it is appropriate to use corresponding IC sockets for the optocoupler ICs and the EEPROM. Once you are done with the soldering, you can just plug in the ICs. Pay attention to the correct alignment of the
sockets and optocouplers/EEPROM as well as of the diodes and transistors!  

Before putting the adapter into operation, it is advisable to thoroughly check the assembly again and (if possible)
measure the circuit with a multimeter. Cold solder joints, accidentally bridged contacts and so on can cause inexplicable and difficult to diagnose misconduct
of the adapater up to an eventual controller defect!

Good luck!
    
---  

[Further on to appendix B](appendix_b.md)      
[Back to TOC](toc.md)   
