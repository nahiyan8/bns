BNS: Bouncing Network Storage
=============================

BNS is a network storage model inspired by [this StackOverflow answer](http://stackoverflow.com/a/13000176/2170682) and criticised by mar771 (>_>).

The idea behind it is basically to bounce around packets of data, e.g.:
1.  A sends "I'm a butterfly!" to B.
2.  Now, B doesn't store it but it just resends it to A.
3.  A resends it to B.
4.  The resending continues indefinitely.

Now, there are some caveats with this design:
* Packet loss, solvable with TCP, but that increases the memory footprint.
* Out-of-orders, solvable with TCP, or including a ID field.
* Bandwidth:
    * A->B is limited by the outbound limit of A and the inbound limit of B; and vice-versa for B->A.
    * If inbound is faster than outbound for a node, then the buffer will fill up to accomodate.
* Latency: If it's too low, too much bouncing will occur, taking up processing power and causing more data to be stored within the buffers.

So, for optimum function and minimum memory usage, these criteria should be followed:
* Higher bandwidth for larger loads, decreases buffer usage.
* Symmetric bandwidth. Atleast don't have outbound limits greater than inbound.
* High latency with minimum packet-loss/out-of-orders.
