# IPv6

General notes:
--------------

- Tldr; Success with Adguard Home is VERY sensitive to a router firmware's ability to properly delegate v6 responsibilities to it (read: the best routers for this are ones with open firmware, or adguard hoome already built-in). For all other routers, work within their limitations, give them what they want and accept you'll likely have to periodically check in and verify/update their v6 dynamic global prefix for adguard's host interface.

- It's a _potential_ instability, but a slow one.

- Printing a label with a reminder/warning on the device's chassis itself may be instructive, but really you should add a reminder on your calendar or to-do app.

```txt

Every IPv6 interface automatically generates a permanent fe80::... address derived from its MAC address. Because it is tied to the physical NIC and never depends on your ISP, it never changes as long as IPv6 privacy extensions aren't overriding the primary link-local address.

1. Find the Pi's permanent Link-Local address:
Run on the Raspberry Pi:bash
ip -6 addr show eth0 scope link
Use code with caution.
(Look for the fe80::... address—usually ending in your MAC address with ff:fe inserted in the middle. Note: Omit the %eth0 zone index when entering it into router web forms.)

2. Point the Nighthawk to it:
	• Log into the Nighthawk web UI (192.168.1.1) -> Advanced > Advanced Setup > IPv6.
	• Set IPv6 DNS Server to Use These DNS Servers and enter the Pi's fe80::... address in Primary DNS.
	• Important caveat on Nighthawks: Just like IPv4 on the Nighthawk (where the router acts as the DNS forwarder to clients unless DHCP is disabled), the Nighthawk will pass its own LAN IPv6 address (fe80::...) to clients via RDNSS/DHCPv6 and forward their queries upstream to the Pi. AdGuard Home will work, though all IPv6 queries will show up in AdGuard logs as coming from the router's IP unless you disable IPv6 LAN server functions on the Nighthawk or let AdGuard handle DHCP/RA.

```
@ai-generated
