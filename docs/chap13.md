[Back to TOC](toc.md)  
[Back to chapter 12](chap12.md)    
   
---      
    
# 13. Excursus: Reaching BSB-LAN Securely from the Internet
    
This section describes the basic possibilities for securely reaching BSB-LAN from the Internet. Due to the large number of available routers, only the most important steps can be described here. For further details, please consult the manual of the respective router. We cannot offer support for the setup of these steps, please ask for advice in appropriate internet forums.  
  
**Basic requirement: Set up (sub-)domain with dynamic DNS**  
To enable external access, you need your own (sub-)domain that can be reached from the Internet via a dynamic DNS service. Some router or NAS providers like AVM or Synology offer such a service directly for their customers, otherwise you have to have your own domain (e.g. `my-home.com`), where you then set up a subdomain (here in the example `bsb-lan.my-home.com`), which then has to be configured accordingly together with the dynamic DNS provider.  
  
**Variant 1: Virtual Private Network (VPN)**  
Many routers provide a server for a virtual private network (VPN) out of the box. This is the most secure variant, because in this way other access to the home network is generally blocked. If such a VPN server is set up and activated on the router, for example, you can access BSB-LAN with a VPN-capable device in the same way as you would otherwise, i.e. normally via your home IP address.  
The disadvantage, however, is that it is not possible to access BSB-LAN without a VPN-enabled device. Likewise, the Internet access with which one uses the Internet while on the road may be configured in such a way that VPN is not possible. In these cases, there is then no way to access the home resources.  
  
**Variant 2: Reverse Proxy**  
A reverse proxy offers, among other things, the possibility to reach several devices in the home network via a single, externally visible device running a reverse proxy server. The following steps are necessary for this:  
  
1. Set up port forwarding  
Port forwarding must be set up on the local network for the device running the reverse proxy. In order to secure this access via SSL/TLS, port 443 must be used for this purpose. Please note that it may not be possible to access the actual router via port 443. With some routers, however, the SSL port can be changed, so that this need not be a fundamental problem. For the use of SSL/TLS / port 443, of course, an appropriate (possibly self-signed) certificate must be installed on the device. Many router or NAS manufacturers already offer the installation of free Let's Encrypt certificates.  

2. Installing and setting up the reverse proxy  
The device on which the reverse proxy runs can be any computer that is permanently accessible, e.g. a file server/NAS. The reverse proxy server is installed and set up on this machine. If you use a Synology NAS for this purpose, such a function is already built in from DSM 7 (see Control Panel / Login Portal / Advanced).  
You now configure the reverse proxy so that it accepts requests for the selected (sub)domain via *HTTPS*(!) on port 443 and then forwards them via *HTTP*(!) to port 80 of the BSB-LAN adapter. The way back is then exactly the other way round: From BSB-LAN via unsecured HTTP to the reverse proxy and from there via HTTPS back out to the Internet.  
Now BSB-LAN can be reached directly via the HTTPS call of the (sub)domain. It is now recommended to enable HTTP authentication in BSB-LAN in any case, otherwise everyone would have access to BSB-LAN.  
      
---

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/U6U5NPB51)    

---

[Further on to chapter 14](chap14.md)      
[Back to TOC](toc.md)   

