## How can a node have IP address?
- Static IP address assignment which is very manual process
- through DHCP: dynamic host IP assignment

## How DHCP works?
- When a device wants access to a network that’s using DHCP, it sends a request for an IP address that is picked up by a DHCP server. The server responds be delivering an IP address to the device, then monitors the use of the address and takes it back after a specified time or when the device shuts down. The IP address is then returned to the pool of addresses managed by the DHCP server to be reassigned to another device as it seeks access to the network.

## Benefits of DHCP
- Reliable IP address configuration: two users cannot have the same IP address
- Reduced network administration
- Mobility: DHCP efficiently handles IP address changes for users on portable devices who move to different locations on wired or wireless networks

## DHCP components
- DHCP server
> This is a networked device running the DCHP service that holds IP addresses and related configuration information. This is most typically a server or a router but could be anything that acts as a host, such as an SD-WAN appliance.
- DHCP client
> endpoint software requests and receives configuration information from a DHCP server. This can be installed on a computer, mobile device, IoT endpoint or anything else that requires connectivity to the network
- IP address pool
- Subnet
- Lease
> The length of time for which a DHCP client holds the IP address information is known as the lease. When a lease expires, the client must renew it.
- DHCP relay
> This can be used to centralize DHCP servers instead of having a server on each subnet.

## Default gateway
- This gateway is responsible for transferring data back and forth between the local network and Internet, or between local subnets.


## DNS 
- naming the host -> mapping between IP and name

[DHCP defined and how it works](https://www.networkworld.com/article/3299438/dhcp-defined-and-how-it-works.html)