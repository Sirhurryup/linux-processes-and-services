ip route tells Linux how to deliver packets. If the destination belongs to my local subnet, Linux delivers it directly. If the destination is outside my subnet and no more specific route exists, Linux forwards the packet to the default gateway, which knows the next hop.

# think in dependencies
```
Can I reach myself?

↓

Can I reach my gateway?

↓

Can I reach another host?

↓

Can I resolve DNS?

↓

Can I reach an application?
```
