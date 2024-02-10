Security level of an iterface - define how much you trust traffic from that interface 0 being least trusted and 100 being most trusted

Network object represents IP addresses, IP ranges or fully qualified domain names (FQDNs). They simplify the configuration of access control policies, NAT and other settings
```cisco
ciscoasa(config)# object network <object-name>
ciscoasa(config-network-object)# host <ip-address>

ciscoasa(config)# show object network <object-name>
```