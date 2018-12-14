[//]: # (tags: iptables listing firewall rules chain flush clear reset)

## IPTABLES

### Listing commands
To list the active rules by specification (input, forward, output, drop accept, etc):

`iptables -S`

To list for example only the chain (INPUT, OUTPUT, FORWARD) you want do this:

`iptables -S INPUT`

To list every chain (INPUT, OUTPUT, FORWARD) this:

`iptables -L`

To list the packet counts:

`iptables -L -v`

To list the iptables with line numbers:

`iptables -L --line-numbers`

### Delete packet counters commands
To clear the packet counts counters for all chains:

`iptables -Z`

or for a specific chain:
`iptables -Z INPUT`

or for a specific line number of a specific chain:

`iptables -Z INPUT 1`

### Flushing (mass delete)
To flush a specific chain:

`iptables -F INPUT`

To flush all chains:

`iptables -F`

To flush everything (reset your firewall and accept all, but enable it):

`iptables -P INPUT ACCEPT`

`iptables -P FORWARD ACCEPT`

`iptables -P OUTPUT ACCEPT`

`iptables -t nat -F`

`iptables -t mangle -F`

`iptables -F`

`iptables -X`

### Routing/forwarding packets from one interface to the other:
source is `eth1` and destination interface is `eth0`:

`iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT`

`iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT`

`iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE`

