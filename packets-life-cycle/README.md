# CCNA PACKETS LIFE CYCLE

## Overview

This Packet Tracer lab tests your understanding of how a packet travels from a source device to a destination device through a network.

The main focus is identifying the **source and destination MAC addresses** at different points along the route.

No device configuration is required. Instead, the lab uses Packet Tracer Simulation Mode and CLI commands to verify the answers. 

## Learning Objectives

By completing this lab, you should understand:

- How Ethernet source and destination MAC addresses are selected.
- How MAC addresses change when a packet crosses a router.
- How switches forward Ethernet frames.
- The difference between communication within the same network and communication between different networks.
- How to use `ipconfig /all` to identify a Windows PC's MAC address.
- How to use `show interface` on Cisco routers to view interface MAC addresses.
- How to use Packet Tracer Simulation Mode to inspect Layer 2 information.

## Important Concept

The **source and destination IP addresses do not change** as the packet travels from the source to the destination.

For the PC1 → PC4 example:

- Source IP: `192.168.1.1`
- Destination IP: `192.168.3.1`

The MAC addresses change at each router hop because a new Ethernet frame is created for each network segment.

## Question 1 — PC1 to PC4

PC1 and PC4 are on different networks:

- PC1: `192.168.1.0/24`
- PC4: `192.168.3.0/24`

### MAC Address Path

| Segment | Source MAC | Destination MAC |
|---|---|---|
| PC1 → SW1 | PC1 (`1111`) | R1 G0/0 (`AAAA`) |
| SW1 → R1 | PC1 (`1111`) | R1 G0/0 (`AAAA`) |
| R1 → R2 | R1 G0/1 (`BBBB`) | R2 G0/0 (`CCCC`) |
| R2 → R3 | R2 G0/1 (`DDDD`) | R3 G0/0 (`EEEE`) |
| R3 → SW2 | R3 G0/1 (`FFFF`) | PC4 (`4444`) |
| SW2 → PC4 | R3 G0/1 (`FFFF`) | PC4 (`4444`) |

Switches do not change the source or destination MAC addresses. Routers create a new Layer 2 frame for the next network segment.

## Question 2 — PC1 to PC3

PC1 and PC3 are on the same network.

Because the destination is on the same network, PC1 sends the frame directly to PC3 instead of sending it to its default gateway.

| Segment | Source MAC | Destination MAC |
|---|---|---|
| PC1 → SW1 | PC1 (`1111`) | PC3 (`3333`) |
| SW1 → PC3 | PC1 (`1111`) | PC3 (`3333`) |

The switch forwards the frame without modifying its source or destination MAC addresses.

## Useful Commands

### Windows PC

Use the following command to view the MAC address:

```text
ipconfig /all
```

Look for the **Physical Address** of the relevant network interface.

### Cisco Router

Use:

```text
enable
show interface g0/0
```

or:

```text
show interface g0/1
```

The interface information displays its MAC address.

To check the running configuration:

```text
show running-config
```

Look under the relevant GigabitEthernet interface for the `mac-address` command.

## MAC Address Summary

| Device / Interface | MAC Ending |
|---|---:|
| PC1 | `1111` |
| R1 G0/0 | `AAAA` |
| R1 G0/1 | `BBBB` |
| R2 G0/0 | `CCCC` |
| R2 G0/1 | `DDDD` |
| R3 G0/0 | `EEEE` |
| R3 G0/1 | `FFFF` |
| PC3 | `3333` |
| PC4 | `4444` |

## Key Takeaway

### Same Network

```text
PC1 ── SW1 ── PC3
```

The frame uses:

```text
PC1 MAC → PC3 MAC
```

### Different Networks

```text
PC1 ── SW1 ── R1 ── R2 ── R3 ── SW2 ── PC4
```

At each router:

```text
Source MAC = outgoing router interface
Destination MAC = next-hop device


```

## Question 3-PC4 to PC1, it is the reverse path of PC1 → PC4.

The IP addresses are now:

Source IP: PC4 = 192.168.3.1
Destination IP: PC1 = 192.168.1.1

The MAC addresses change at every router hop.

PC4 → PC1
Segment	Source MAC	Destination MAC
PC4 → SW2	PC4 = 4444	R3 G0/1 = FFFF
SW2 → R3	PC4 = 4444	R3 G0/1 = FFFF
R3 → R2	R3 G0/0 = EEEE	R2 G0/1 = DDDD
R2 → R1	R2 G0/0 = CCCC	R1 G0/1 = BBBB
R1 → SW1	R1 G0/0 = AAAA	PC1 = 1111
SW1 → PC1	R1 G0/0 = AAAA	PC1 = 1111
Easy way to remember it

PC1 → PC4 was:

1111 → AAAA
BBBB → CCCC
DDDD → EEEE
FFFF → 4444

PC4 → PC1 reverses the direction:

4444 → FFFF
EEEE → DDDD
CCCC → BBBB
AAAA → 1111

The IP addresses stay end-to-end as:

192.168.3.1 → 192.168.1.1

The IP source and destination remain unchanged throughout the journey, while the Layer 2 MAC addresses are rewritten for each routed segment.


