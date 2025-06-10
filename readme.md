# Satisfactory 1.1 Pterodactyl Egg

Satisfactory is a first-person open-world factory building game with a dash of exploration and combat. Play alone or with friends, explore an alien planet, create multi-story factories, and enter conveyor belt heaven!

## TO NOTE
New server variables have been added to the egg.
* `ReliablePort` - Allows you to specify a port for the Reliable Messaging TCP Factory.


## Dedicated Server - Port Forwarding Updates
See [Patch Notes](https://questions.satisfactorygame.com/post/68484d076b7c57319638344f) for more information on how to port forward.

If you Host a dedicated server, you should read the next block as many new improvements have been added to allow for extra flexibility since the last update

We have updated the Port Allocation Strategy in Reliable Messaging

New features:

**Explicit Port Configuration**

* A new -ReliablePort= command-line parameter allows explicit port selection.
* The value must be an integer between 0 and 65535.
* If specified, the server will attempt to bind to this port and fail to initialize if the port is unavailable.



**Default and Configurable Port Ranges**

The following settings in Engine.ini control port allocation:

[/Script/ReliableMessaging.ReliableMessagingTCPFactory]

PortRangeBegin=8888

PortRangeLength=512

ExternalPortRangeBegin=-1



* The server will attempt to bind within [PortRangeBegin, PortRangeBegin + PortRangeLength).
* By default, the server starts at port 8888 and tries up to 512 ports until it finds an available one.


**Client Awareness & NAT Handling**

* Clients must connect to the correct port, but port remapping (e.g., via NAT/firewall rules) can break this.
* To address this, the server now communicates the listening port to clients during the initial handshake.
* If external port remapping is used, the server must be aware of the external port via:
  * The ExternalPortRangeBegin config setting (for remapped ranges).
  * The -ExternalReliablePort= command-line parameter (for explicitly mapped ports)


**Server Host Requirements (TL;DR)**

* If hosting a single server, port 8888 TCP must be open by default.
* If hosting multiple servers, a range of ports starting from 8888 TCP (by default) must be open.
* The server will attempt up to 512 ports before failing (configurable).
* If port remapping (NAT/firewall) is used, the server must be configured accordingly; otherwise, clients won’t be able to connect.
* Logging is in place to help server maintainers verify the allocated ports.
