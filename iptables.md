[//]: # (tags: iptables listing firewall rules chain flush clear reset)

# IPTABLES

## Listing commands
### To list the active rules by specification (input, forward, output, drop accept, etc):
`iptables -S`

### To list for example only the chain (INPUT, OUTPUT, FORWARD) you want do this:
`iptables -S INPUT`

### To list every chain (INPUT, OUTPUT, FORWARD) this:
`iptables -L`

### To list the packet counts:
`iptables -L -v`

### To list the iptables with line numbers:
`iptables -L --line-numbers`

## Delete packet counters commands
### To clear the packet counts counters for all chains:
`iptables -Z`

### or for a specific chain:
`iptables -Z INPUT`

### or for a specific line number of a specific chain:
`iptables -Z INPUT 1`

## Flushing (mass delete)
### To flush a specific chain:
`iptables -F INPUT`

### To flush all chains:
`iptables -F`

### To flush everything (reset your firewall and accept all, but enable it):

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

### Deleting rules
To delete rules by rule definition. Use `iptables -S` to use its output:

`iptables -D INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT`

To delete rules by number (and chain):

`iptables -D INPUT 12`


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

### iptables general info:
By default there are **three** tables in the kernel that contain sets of rules:

1. The **filter table** that is used for packtet filtering. This is the "normal"
output with the three default chains that we see usually, INPUT, FORWARD,
OUTPUT.

`filter table` has three chains (sets of rules):

a. INPUT: for packets comming in the system

b. OUTPUT: for packets going out of the system

c. FORWARD: for packets that are routed through the system

To list the filter table and all its rules:

`iptables -t filter -nL`

Note that the ACCEPT is the default behavior if nothing is set.

e.g.: to allow access from a whole subnet only from a specific iface:

`iptables -A INPUT -i ens36 -s 10.0.0.0/24 -p tcp -j ACCEPT`

`iptables -A OUTPUT -o ens36 -d 10.0.0.0/24 -p tcp -j ACCEPT`

or, to allow `ping` to or from your machine:

`iptables -A INPUT -p icmp --icmp-type any -j ACCEPT`

`iptables -A OUTPUT -p icmp --icmp-type any -j ACCEPT`

Note: this doesn't allow other computers to route `ping` messages through your
router, it only handles INPUT and OUTPUT. For routing `ping`, you need to enable
it on the FORWARD chain, like so:

`iptables -A FORWARD -p icmp --icmp-type any -j ACCEPT`


2. The **nat table** that is used for natting. It includes the 3 default chains:
PREROUTING, POSTROUTING, OUTPUT

The `nat table` in iptables adds two new chains: PREROUTING that allows altering
of the packets before they reach the INPUT chain and POSTROUTING that allows
altering packets after they exit the OUTPUT chain.

**snat** (source NAT): changes the source address inside a packet before it
leaves the system (e.g. to the internet). The destination will return the packet
to the NAT-device. This means that our NAT-device will need to keep a table in
memory of all the packets it changed, so it can deliver the packet to the
original source (e.g. in the private network).

Since `snat` is about packets leaving the system, it uses the POSTROUTING chain.

e.g.: packets coming from 10.1.0.0/24 network and exiting via ens36 will get the
source ip-address set to 11.12.13.14:

`iptables -t nat -A POSTROUTING -o ens36 -s 10.1.0.0/24 -j SNAT --to-source
11.12.13.14`

**IP masquerading** is very similar to SNAT, but is meant for dynamic IP
addresses, e.g.: routers/modems connected to the internet and receiving a
different ip-address from the ISP, each time they are cold-booted:

`iptables -t nat -A POSTROUTING -o ens36 -s 10.1.0.0/24 -j MASQUERADE`

**DNAT** (destination NAT): is used to allow packets from the Internet to be
redirected to an internal server in your DMZ and in a private address range that
is inaccessible directly from the Internet. Think of it like this: a packet
arrives from the internet with a public IP address as the destination address
(the router's one) and the router changes the Destination IP address of the
packet so as to match the desired host within its private network that resides
"behind" it. Then the packet with the changed dest IP address has an internal IP
address as a destination and gets "forwarded" to the INPUT chain of the `filter`
table. The `filter` table's INPUT chain rules then are applied on this packet.

e.g.: to allow Internet to reach your internal SSH server on host 192.168.1.99
on port 22:

`iptables -A FORWARD -i eth1 -o eth0 -p tcp --dport 22 -j ACCEPT`

`iptables -t nat -A PREROUTING -i eth1 -p tcp --dport 22 -j DNAT 
--to-destination 192.168.1.99`

3. The **mangle table** that is used for special-purpose processing of packets.
4. The **security table** that is used for security DAC/MAC
5. The **raw table** mainly for connection tracking and exemptions.

### Logging
Use the LOG action to log certain things that can be later seen at the kernel
log messages, e.g. `journalctl -f`.

To log actions that relate to the INPUT chain rule:

`iptables -I INPUT 1 -j LOG`

The above command uses the -I to **Insert** the rule at position "1". If we had
used -A it would **Append** the rule at the end and if the other `filter` table
rules were before it, it would essentially do nothing.

Similarly to log the FORWARD and OUTPUT chain:

`iptables -I FORWARD 1 -j LOG`

`iptables -I OUTPUT 1 -j LOG`

Following the `nat` discussion above, to log network activity on the NAT table,
we need to use the respective NAT chains:

`iptables -t nat -I PREROUTING 1 -j LOG`

`iptables -t nat -I POSTROUTING 1 -j LOG`

`iptables -t nat -I OUTPUT 1 -j LOG`

### Policies
The -P rules create default "policies" on what the default action should be,
DROP or ACCEPT. The correct fw should have DROP, so after the rules we want each
time to have, we should do:

`iptables -P INPUT DROP`

`iptables -P OUTPUT DROP`

`iptables -P FORWARD DROP`

*sources*: 

http://linux-training.be/networking/ch14.html

http://linux-ip.net/pages/diagrams.htm 

https://www.digitalocean.com/community/tutorials/a-deep-dive-into-iptables-and-netfilter-architecturel
