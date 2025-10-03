# Postrouting and IP Masquerading in Linux

>IPTables uses NAT table to forward packets to another node.

---

## What is POSTROUTING?

>A Postrouting chain in NAT table means altering the IP packet after the routing is completed. Logically, a postrouting can be used to change the Source Address. As the routing is completed and destination has his own address, the only unknown address that can be masked is the Source. This is why postrouting is used for SNAT.
