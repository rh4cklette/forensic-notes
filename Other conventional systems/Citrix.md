Clearly as custom Linux server, FreeBSD observed on some (confirm what are the other possible distros)

Can be linked to Active Directory, so AD users can connect.

![[maxresdefault.jpg]]



Multiple modules that interact together : 

Citrix ADC : 

Citrix ADC <u><b>IS</b></u> NetScaler, ADC is the old name
Citrix NetScaler is a web application delivery controller (ADC). Can be the same appliance that does the ADC part and the Gateway part.

Citrix Gateway : 

SSL VPN appliance, provides a "secure" point of access.
Can be a Virtual Machine.


Citrix StoreFront : 

Authenticates users and manages stores of desktops and applications that users access

Citrix SD-WAN : 

In deployments where virtual desktopars are delivered to users at remote locations such as branch officiles, Citrix SD-WAN optimizes performance for LAN-like perf over the WAN.

Delivery Controller : 

Central management component of a site. Installed at least on one server in the data center. If the deployment includes a hypervisor or other service, the Controller services communicate with it to : 
- Distribute applications and desktops
- Authenticate and manage user access
- Broker connections between users and their desktop and apps
- Other optimizations for connections

Ressource : https://github.com/B0lg0r0v/citrix-netscaler-forensics
