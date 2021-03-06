Date: 2018-12-04
Tags: lokinet
Category: development
Title: Quick Lokinet Update December 2018
Authors: jeff

A very large amount of work happened with lokinet development since the last blog update in august.
As of writing, exit traffic works, hidden services work and service node traffic is wired up but untested.
I also took the liberty of refactoring the dns code used in lokinet. 

If lokinet is to thrive you want to make the transition to it as painless as possible, 
hence why I chose DNS as the primary mechanism of controlling when to look up things on the network.
Most if not all network aware programs use DNS first when trying to figure out how to connect to something, 
excpet if it looks like an IP address. 
It's far more complex under the hood but from the end user's point of view it's effectively so.

By having lokinet expose IP and DNS only, everything written should trivially work with little or no application porting needed. 

My use of DNS in Lokinet can be explained as a forwarding, non caching DNS MiTM proxy that intercepts 2 TLDs, (.loki and .snode)
It intercepts dns queries and if the TLDs (top layer domain) is NOT loki or snode it will blindly forward any queries it gets 
to an upstream provider randomly chosen from list configured at runtime 
(I currently use [dnscrypt proxy](https://dnscrypt.info) as my upstream resolver on my workstation). 
Any replies from the upstream DNS provider are forwarded to the requester.

Right now the assumption is that Lokinet's DNS is on a LAN or loopback address and is properly firewalled from any kind of bogon traffic. 
This is probably not a safe thing to expect as a default as people can do really weird and dumb things if you let them.
I am toying with the idea of mapping the i2p and onion using via transparent proxy but such is a low priority right now and would explode the complexity of what I am doing at this moment.
I may toy with per IP endpoint request isolation, 
where address A and B both get their own set of network paths for lookups and ip traffic. 
This may entail poking firewall rules into the system to prevent A and B from using each other's paths. Further research is required, they are just ideas I had.
