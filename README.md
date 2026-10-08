VLAN Network with DHCP and Web Services

A two-VLAN network designed, configured and tested in Cisco Packet Tracer, with automatic addressing on one VLAN, static addressing on the other, routing between them, and a web service reachable from every host.

University coursework, 4301COMP Computer Systems Architecture, Liverpool John Moores University. Awarded 78%.

The design
          VLAN 10   	VLAN 20
Subnet	192.168.10.0/24	192.168.20.0/24
Gateway	192.168.10.1	192.168.20.1
Addressing	DHCP	Static
Hosts	DHCP Server, PC1, PC2	Web Server (192.168.20.2), PC3, PC4

Hardware: one Cisco 1941 router, one 2960-24TT switch, two servers and four PCs. The switch connects to the router over a single trunk link.

Router on a stick. Rather than using a separate physical interface per VLAN, both VLANs share one physical link to the router, which is subdivided into two sub-interfaces, G0/0.10 and G0/0.20. Each sub-interface acts as the default gateway for its own VLAN. This is what lets two networks that are deliberately isolated at the switch talk to each other again, through a single controlled point.

DHCP pool (VLAN 10): starting address 192.168.10.3, subnet mask 255.255.255.0, gateway 192.168.10.1, DNS 8.8.8.8, maximum 50 clients.

Why VLAN 20 is static. The web server has to be findable. If its address changed on a lease renewal, every client pointing at it would break. Servers get fixed addresses, clients get leased ones.

Testing

The network was verified rather than assumed:

PC1 and PC2 received addresses automatically from the DHCP server
Pings succeeded from VLAN 10 hosts to VLAN 20 hosts, proving inter-VLAN routing works through the sub-interfaces
Both gateway addresses responded to pings
All four PCs loaded the web server's page in a browser, confirming HTTP access across the VLAN boundary
Limitations

One link carries everything. All inter-VLAN traffic passes through a single physical router interface. Fine for a lab, but in a real deployment it becomes a bottleneck under load. A multilayer switch would route between VLANs at switching speed instead.

No redundancy. If the router fails, the VLANs cannot reach each other. If the DHCP server fails, VLAN 10 stops getting addresses. A production network would need a backup for both.

No security policy between the VLANs. Once routing is enabled, every host can reach every other host. The separation is organisational rather than protective. Access control lists or a firewall would be needed to restrict which devices can reach the web server.

What I took from it

The VLANs and the routing are two halves of the same idea. Splitting the network is easy, and so is connecting it. Doing both at once, so the segments stay separate but can still communicate through one deliberate route, is the part that takes thinking about.

The module also covered the systems side, so the report explained how the operating system kernel loads the DHCP and web server programs into memory, assigns them process IDs and schedules CPU time for them, how those processes use system calls to reach hardware rather than touching it directly, and how the CPU works through fetch, decode and execute to turn an incoming request into a response.

Built with

Cisco Packet Tracer, Cisco IOS configuration, VLANs, 802.1Q trunking, DHCP and HTTP.
