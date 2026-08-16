# CCNA Ethernet LAN Switching 

## Both switches have an empty MAC address table, and all PCs have an empty ARP table.

1. If PC1 pings to PC3, what messages will be sent over the network, 
     and which devices will receive them?

2. Send the ping and use Packet Tracer's 'simulation mode' to verify your answer.

3. Use pings to generate network traffic and allow the switches to learn the MAC addresses 
     of all PCs on the network.

4. Use 'show' commands on the switches to identify the MAC address of each PC.

5. Clear the dynamic MAC addresses from the MAC address table of each switch.

===================================================================================================
## 1. PC1 pings PC3: What happens?

Assume:

PC1 = 192.168.1.1
PC3 = 192.168.1.3
Both switches start with empty MAC address tables
All PCs start with empty ARP tables

Before PC1 can send an ICMP ping, it needs PC3's MAC address.

The process is:

ARP Request
PC1 broadcasts an ARP request: “Who has 192.168.1.3?”
Destination MAC: FF:FF:FF:FF:FF:FF
SW1 floods it.
PC2, SW2, and then PC3/PC4 receive it.
PC2 and PC4 ignore it.
ARP Reply
PC3 sends an ARP reply back to PC1.
This is unicast, not broadcast.
Path: PC3 → SW2 → SW1 → PC1
PC1 learns PC3's MAC address in its ARP table.
ICMP Echo Request
PC1 can now send the actual ping.
It is a unicast frame addressed to PC3's MAC.
### Path: PC1 → SW1 → SW2 → PC3
ICMP Echo Reply
PC3 responds to PC1.
Also unicast.
### Path: PC3 → SW2 → SW1 → PC1

### So the key sequence is:

### ARP Request → ARP Reply → ICMP Echo Request → ICMP Echo Reply

Why does the ARP request reach every PC?

ARP requests are broadcasts.

Ethernet switches flood broadcast frames out all appropriate ports except the port on which the frame was received.

### Therefore:

### PC1 → SW1 → PC2 + SW2 → PC3 + PC4

PC2 and PC4 receive the ARP request but do not respond because the requested IP belongs to PC3.

The ARP reply is different because it is unicast:

PC3 → SW2 → SW1 → PC1

## 2. Important Packet Tracer observation

When PC1 starts the ping, the ICMP packet initially cannot be sent because PC1 doesn't know PC3's MAC address.

The ARP process:

Looks for PC3's IP in the ARP table.
Doesn't find it.
Creates an ARP request.
Buffers the ICMP packet.
Sends the ICMP packet after receiving the ARP reply.

In the Ethernet frame, the ARP message has:

Field	Value/Meaning
Destination MAC	FFFF.FFFF.FFFF — broadcast
Source MAC	PC1's MAC
Type	0x0806 — ARP
Data	ARP packet
FCS	Frame Check Sequence

The Ethernet Type field is particularly important:

0x0806 = ARP
0x0800 = IPv4
## 3. How switches learn MAC addresses

A switch learns MAC addresses from the source MAC address of incoming Ethernet frames.

For example, when PC1 sends a frame into SW1:

Source MAC = PC1's MAC

SW1 records something like:

PC1 MAC → Fa0/1

When the frame travels through SW2, SW2 can learn PC1's MAC on its connection toward SW1.

This is called dynamic MAC address learning.

## 4. show mac address-table

The command used to view the switch's MAC address table is:

show mac address-table

From SW1:

PC1 is connected to Fa0/1
PC2 is connected to Fa0/2
PC3 and PC4 are reached through Gi0/1

Therefore, SW1 can identify PC1 and PC2 directly.

However, SW1 cannot determine whether a particular MAC learned on Gi0/1 belongs to PC3 or PC4.

You need to check SW2.

On SW2:

PC3 → Fa0/1
PC4 → Fa0/2

Therefore, SW2 lets you identify which MAC belongs to PC3 and which belongs to PC4.

## 5. Clearing the MAC address table

The command is:

clear mac address-table dynamic

This removes dynamically learned MAC addresses.

In Packet Tracer, unlike some real Cisco environments/GNS3 IOS versions, you may not have the option to clear only a specific MAC address or interface. The Packet Tracer implementation clears the dynamic entries.

You can verify the result with:

### show mac address-table
Key CCNA Concepts to Remember

### ARP

Resolves IPv4 address → MAC address
ARP Request = broadcast
ARP Reply = unicast

### ICMP

Ping uses ICMP.
Echo Request = unicast in this scenario
Echo Reply = unicast

### Ethernet switching

Switches learn from the source MAC address.
Unknown unicast frames are flooded.
Broadcast frames are flooded.
Known unicast frames are forwarded only toward the appropriate port.

## Commands

enable
show mac address-table
clear mac address-table dynamic
The most important flow
PC1
 │
 │ ARP Request (Broadcast)
 ▼
SW1 ─────► PC2
 │
 ▼
SW2 ─────► PC4
 │
 ▼
PC3
 │
 │ ARP Reply (Unicast)
 ▼
SW2 → SW1 → PC1
 │
 │
 ▼
ICMP Echo Request
PC1 → SW1 → SW2 → PC3
 │
 │
 ▼
ICMP Echo Reply
PC3 → SW2 → SW1 → PC1

## Lab takeaway: 
ARP finds the destination MAC; switches use MAC addresses to forward Ethernet frames; switches learn those MAC addresses from the source MAC of incoming frames.
