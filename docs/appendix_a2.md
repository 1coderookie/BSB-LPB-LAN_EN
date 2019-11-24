[Back to TOC](toc.md)  
[Back to appendix A1](appendix_a1.md)    
   
--- 
    
# Appendix A2: Notes on the Circuit Diagram  
    
---  

## A2.1 Short Explanation of the Circuit Diagram

D1 = LED (red)  
D2 = Diode  
Q(x) = Transistor  
R(x) = Resistor  
U(x) = Optocouplers  
ARD = Arduino  
RPI = Raspberry Pi  
CL+/- = BSB connectors  
DB/MB = LPB connectors  
TXD = Digital pin: transmit  
RXD = Digital pin: receive
    
---
        
## A2.2 Parts List

For the usage with an ***Arduino***:  
1x LED (red) (operating voltage max. 2,8V, reverse voltage 5V) (→ D1)  
1x Diode 1N4148 (→ D2)  
1x Transistor BC547A (→ Q1)  
Resistors, each 1x: 560kΩ (→ R1), 1,5kΩ (→ R2), 300Ω (→ R3), 4,7kΩ (→ R4)  
2x Optocouplers 4N25 (→ U1, U2) (plus optional 2x socket)
    
**Additionally** for the usage with a ***Raspberry Pi 2***:  
1x Resistor 4,7kΩ (→ R11)  
2x Resistor 10kΩ (→ R12, R13)  
1x Transistor BC557A (→ Q11)  
1x Transistor BC547A (→ Q12)  
    
Optional:  
Connection terminals, pinheader etc. for the cables.  
    
---
    


## A2.3 General Notes

***Before soldering: study the circuit diagram attentively!***

***Important:***  

*If you use the PCB of the adapter from Frederik in conjunction with an* ***Arduino***, *you MUST make the connection at solder jumper SJ1 - but NOT at SJ2 and SJ3!*  
*The assembly of R11-13 and Q11&12 isn't necessary!*

*But: If you want to use the adapter in conjunction with a* ***Raspberry Pi2***, *you MUST make the connections at the solder jumpers SJ2 and SJ3 - but NOT at SJ1!*  
*In addition to that, you also have to assemble R11-13 and Q11&12!*  
    
If you want to make the whole circuit of the adapter by yourself starting off from a plain board and you want to use it with an Arduino, then remember that you don't have to build the optional circuits for the use with a Raspberry Pi 2. 
    
The following instructions do not replace any fundamental
electronic knowledge, but maybe one or two could 
be helpful for electronics beginners.

Generally it is helpful to position the components, bend the 'legs' of them a little bit to hold them in position on the board and check again the positioning of each component. Pay attention to the correct alignment of parts like diodes, transistors and ICs! If everything seems fine, you can turn around the board and start soldering. Be careful that you just solder where you should so that you don't produce shortcuts by accident.  
A previous breadboard test setup (if you don't use the PCB) of course could be an option, though
due to non-exclude problem sources (usage of a wrong
plug row, possible loose contacts and so on) not necessarily
recommended.  

Please make sure that the components do not get too hot during soldering,
since they can be damaged. Therefore it is appropriate to use corresponding IC sockets for the optocoupler ICs
U1 and U2. Once you are done with the soldering, you can just plug in the ICs. Pay attention to the correct alignment of the
sockets and optocouplers as well as of the diodes and transistors!  

Before putting the adapter into operation, it is advisable to thoroughly check the assembly again and (if possible)
measure the circuit with a multimeter. Cold solder joints, accidentally bridged contacts,
non-set solder joints ("SJ" on the PCB) and so on can cause inexplicable and difficult to diagnose misconduct
of the adapater up to an eventual controller defect!

Good luck!
    
---  

[Further on to appendix B](appendix_b.md)      
[Back to TOC](toc.md)   
