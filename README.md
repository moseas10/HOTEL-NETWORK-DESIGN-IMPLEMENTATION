# HOTEL-NETWORK-DESIGN-IMPLEMENTATION

**PROBLEM SUMMARY**

Design and Implement a Modern Hotel Network. The hotel has three floors; in the first floor there are three departments (Reception, Store and Logistics), in the second floor there are three departments (Finance, HR, and Sales), while the third floor hosts the IT and Admin. Therefore, the following are part of the considerations during the design and implementation. 
1.	There should be three routers connecting each floor (all placed in the server room in IT department)
2.	All routers should be connected to each other using serial DCE cable.
3.	The network between the routers should be 10.10.10.0/30, 10.10.10.4/30. 10.10.10.8/30.
4.	Each floor is expected to have one switch (placed in the respective floor).
5.	Each floor is expected to have WIFI networks connected to laptops and phones.
6.	Each department is expected to have a printer.
7.	Each department is expected to be in different VLAN with the following details.

1st Floor
•	Reception – VLAN 80, Network of 192.168.8.0/24
•	Store – VLAN 70, Network of 192.168.7.0/24
•	Logistics – VLAN 60, Network of 192.168.6.0/24

2nd Floor 
•	Finance – VLAN 50, Network of 192.168.5.0/24
•	HR – VLAN 40, Network of 192.168.4.0/24
•	Sales – VLAN 30, Network of 192.168.3.0/24

3rd Floor
•	Admin – VLAN 20, Network of 192.168.2.0/24
•	IT – VLAN 10, Network of 192.168.1.0/24

8.	Use OSPF as the routing protocol to advertise routes
9.	All devices in the network are expected to obtain IP address dynamically with their respective router configured as the DHCP server.
10.	All the devices in the network are expected to communicate with each other.
11.	Configure SSH in all the routers for remote login.
12.	In IT department, add PC called Test-PC to port fa0/1 and use it to test remote login.
13.	Configure port security to IT-dept switch to allow only Test-PC to access port fa0/1 (use sticky method of obtain mac-address with violation mode of shutdown.)



**DEVICE AND EQUIPMENT**
Routers (3x 2811) — Router_Floor1, Router_Floor2, Router_Floor3 (all placed in IT server room location visually)

Switches (3x 2960) — Switch_Floor1, Switch_Floor2, Switch_Floor3

Wireless APs or Wireless Routers for each floor (one AP per floor)

PCs/Laptops and Phones for departments (one or more per department)

Printers (one per department)

Test-PC in IT department



**TOPOLOGY**


**IP ADDRESSING & VLAN PLAN**

Each department gets a dedicated /24 as specified:

1st Floor
- VLAN 80 — Reception — 192.168.8.0/24
- VLAN 70 — Store — 192.168.7.0/24
- VLAN 60 — Logistics — 192.168.6.0/24

2nd Floor
- VLAN 50 — Finance — 192.168.5.0/24
- VLAN 40 — HR — 192.168.4.0/24
- VLAN 30 — Sales — 192.168.3.0/24

3rd Floor
- VLAN 20 — Admin — 192.168.2.0/24
- VLAN 10 — IT — 192.168.1.0/24

Router Interconnects (point-to-point /30s)
- Link A: 10.10.10.0/30 (between Router_Floor1 and Router_Floor2)
- Link B: 10.10.10.4/30 (between Router_Floor2 and Router_Floor3)
- Link C: 10.10.10.8/30 (between Router_Floor3 and Router_Floor1)

For each VLAN, choose the router subinterface IP as the default gateway (e.g., 192.168.8.1 for Reception). 
DHCP pools will hand out addresses from the respective /24s.
DNS: Set a public DNS like 8.8.8.8 or a local DNS server if available.



**IMPLEMENTATION**
Action 1 - Build Physical Topology 
- Place routers, switches, APs, PCs, and printers.
- Wire serial links between routers (DCE on the side you choose) and Ethernet links from routers to switches.

Action 2 - Configure VLANs on Each Floor Switch
- Create VLANs for the departments present on that floor and assign access ports (PCs, printers, AP) to the appropriate VLANs.

Action 3 - Configure Trunk Link from Switch to its Router
- Switch port to router should be trunking VLANs.
- On the router, create subinterfaces for each VLAN (router-on-a-stick) and assign gateway IPs.

Action 4 - Configure DHCP on Each Router
- Create DHCP pools for the VLANs hosted on that floor, set default-router and DNS, and exclude addresses for static management as needed.

Action 5 - Configure Serial Interfaces on Routers
- Assign the /30 addresses on the serial links, set clock rate on the DCE end(s), and bring interfaces up.

Action 6 - Configure OSPF on all Routers
- Advertise the directly connected VLAN networks and the serial point-to-point networks into OSPF (use area 0 for simplicity).
- Verify OSPF neighborship.

Action 7 - Configure WiFi on each AP
- Set SSID per department
- configure security (WPA2) and ensure AP is connected to the correct VLAN access port.

Action 8 - Enable SSH on Each Router
- Set hostname, domain name, crypto keys, local username(s), and enable ip ssh for secure remote management.

Action 9 - Configure Port-Security on IT Switch
- On Switch_Floor3 apply sticky MAC learning and restrict port fa0/1 to Test-PC; set violation mode to shutdown.

Action 10 - Set Test-PC on IT Port fa0/1
- Plug Test-PC into fa0/1 and confirm it receives DHCP and only it can use that port.

Action 11 - Set all other Hosts to DHCP and Connect to WiFi where Applicable
- Verify they receive addresses from the correct DHCP pools.

Action 12 - Test Connectivity
- Ping between VLANs, across floors, and test remote SSH access to routers (SSH from Test-PC).
- Validate printers and WiFi clients are reachable.


