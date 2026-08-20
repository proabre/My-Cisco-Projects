# Cisco CCNA Static Routing Lab

## Overview

This lab provides hands-on practice configuring **static routes on Cisco routers** using Cisco IOS.

The objective is to configure the network so that **PC1 and PC2 can communicate with each other using ICMP ping**.

The topology contains:

```text
PC1 ── SW1 ── R1 ── R2 ── R3 ── SW2 ── PC2
```

The lab also reinforces basic Cisco IOS configuration, interface addressing, routing tables, and route verification.

---

## Learning Objectives

By completing this lab, you will practice:

- Configuring Cisco router hostnames
- Configuring IPv4 addresses on router interfaces
- Enabling interfaces with `no shutdown`
- Configuring PC IP addresses and default gateways
- Understanding connected and local routes
- Configuring static routes
- Using next-hop IP addresses
- Using an exit interface for static routing
- Reading the Cisco routing table
- Verifying end-to-end connectivity with `ping`
- Understanding why static routes must provide **two-way reachability**

---

## Network Addressing

| Device | Interface | IP Address | Subnet Mask | Connection |
|---|---|---|---|---|
| PC1 | FastEthernet0 | `192.168.1.1` | `255.255.255.0` | R1 |
| R1 | G0/1 | `192.168.1.254` | `255.255.255.0` | PC1 |
| R1 | G0/0 | `192.168.12.1` | `255.255.255.0` | R2 |
| R2 | G0/0 | `192.168.12.2` | `255.255.255.0` | R1 |
| R2 | G0/1 | `192.168.13.2` | `255.255.255.0` | R3 |
| R3 | G0/0 | `192.168.13.3` | `255.255.255.0` | R2 |
| R3 | G0/1 | `192.168.3.254` | `255.255.255.0` | PC2 |
| PC2 | FastEthernet0 | `192.168.3.1` | `255.255.255.0` | R3 |

### Default Gateways

```text
PC1 → 192.168.1.254
PC2 → 192.168.3.254
```

---

## Static Routing Requirements

The routers are directly connected to some networks but not all of them.

### R1

R1 needs a route to:

```text
192.168.3.0/24
```

Next hop:

```text
192.168.12.2
```

### R2

R2 needs two routes:

```text
192.168.1.0/24
192.168.3.0/24
```

### R3

R3 needs a route to:

```text
192.168.1.0/24
```

Next hop:

```text
192.168.13.2
```

Therefore, **four static routes** are required in total.

---

# Configuration

## PC1

Configure:

```text
IP Address:      192.168.1.1
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.1.254
```

---

## R1 Basic Configuration

```cisco
enable
configure terminal

hostname R1

interface g0/1
ip address 192.168.1.254 255.255.255.0
description ## to SW1 ##
no shutdown
exit

interface g0/0
ip address 192.168.12.1 255.255.255.0
description ## to R2 ##
no shutdown
exit
```

Verify:

```cisco
do show ip interface brief
```

### R1 Static Route

```cisco
ip route 192.168.3.0 255.255.255.0 192.168.12.2
```

Verify:

```cisco
do show ip route
```

---

## R2 Basic Configuration

```cisco
enable
configure terminal

hostname R2

interface g0/0
ip address 192.168.12.2 255.255.255.0
description ## to R1 ##
no shutdown
exit

interface g0/1
ip address 192.168.13.2 255.255.255.0
description ## to R3 ##
no shutdown
exit
```

Verify:

```cisco
do show ip interface brief
```

### R2 Static Routes

Route to PC1's network using the exit interface:

```cisco
ip route 192.168.1.0 255.255.255.0 g0/0
```

Route to PC2's network using the next-hop address:

```cisco
ip route 192.168.3.0 255.255.255.0 192.168.13.3
```

Verify:

```cisco
do show ip route
```

---

## R3 Basic Configuration

```cisco
enable
configure terminal

hostname R3

interface g0/0
ip address 192.168.13.3 255.255.255.0
description ## to R2 ##
no shutdown
exit

interface g0/1
ip address 192.168.3.254 255.255.255.0
description ## to SW2 ##
no shutdown
exit
```

Verify:

```cisco
do show ip interface brief
```

### R3 Static Route

```cisco
ip route 192.168.1.0 255.255.255.0 192.168.13.2
```

Verify:

```cisco
do show ip route
```

---

## PC2

Configure:

```text
IP Address:      192.168.3.1
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.3.254
```

---

# Verification

### Check Interface Status

On each router:

```cisco
show ip interface brief
```

Interfaces should generally show:

```text
Status    Protocol
up        up
```

### Check Routing Tables

```cisco
show ip route
```

Static routes appear with:

```text
S
```

For example:

```text
S    192.168.3.0/24 [1/0] via 192.168.12.2
```

### Test Connectivity

From PC1:

```text
ping 192.168.3.1
```

From PC2:

```text
ping 192.168.1.1
```

Successful replies confirm that routing is working in both directions.

---

# Important Concepts

### Connected vs. Local Routes

When an interface is configured with an IP address, Cisco normally creates two relevant routes.

**Connected route:**

```text
C    192.168.1.0/24
```

This represents the entire directly connected network.

**Local route:**

```text
L    192.168.1.254/32
```

This represents the router's own interface address.

---

### Next-Hop Static Route

Example:

```cisco
ip route 192.168.3.0 255.255.255.0 192.168.12.2
```

Meaning:

> To reach `192.168.3.0/24`, forward the packet to the next-hop router at `192.168.12.2`.

---

### Exit-Interface Static Route

Example:

```cisco
ip route 192.168.1.0 255.255.255.0 g0/0
```

Instead of specifying the next-hop IP address, the route specifies the interface through which the packet should leave.

---

## Troubleshooting

If PC1 cannot ping PC2, check the following:

### 1. Check interface status

```cisco
show ip interface brief
```

Look for interfaces that are:

```text
administratively down
```

or:

```text
up/down
```

An administratively down interface usually needs:

```cisco
no shutdown
```

### 2. Check IP addresses

```cisco
show ip interface brief
```

Make sure the configured addresses match the topology.

### 3. Check the routing table

```cisco
show ip route
```

R1 should have a route to:

```text
192.168.3.0/24
```

R2 should have routes to:

```text
192.168.1.0/24
192.168.3.0/24
```

R3 should have a route to:

```text
192.168.1.0/24
```

### 4. Check PC default gateways

PC1:

```text
192.168.1.254
```

PC2:

```text
192.168.3.254
```

### 5. Test hop by hop

From R1:

```cisco
ping 192.168.12.2
```

From R2:

```cisco
ping 192.168.13.3
```

From R3:

```cisco
ping 192.168.3.1
```

This helps identify where connectivity breaks.

---

## Key Takeaways

- Routers only know about networks that are **directly connected** or learned through routing.
- Static routes manually tell a router how to reach a remote network.
- `ip route` is used to configure a static route.
- `show ip route` displays the routing table.
- `C` means **Connected**.
- `L` means **Local**.
- `S` means **Static**.
- End-to-end communication requires **return routes**, not just a route in one direction.
- The first ping can sometimes fail while **ARP** resolves the next-hop MAC address.

---

## Commands to Remember

```cisco
enable
configure terminal
hostname R1
interface g0/0
ip address <IP> <MASK>
no shutdown
description <DESCRIPTION>
show ip interface brief
show ip route
ip route <NETWORK> <MASK> <NEXT-HOP>
ip route <NETWORK> <MASK> <EXIT-INTERFACE>
ping <IP>
```

## Lab Goal

The final test should succeed:
**Successful bidirectional ping = static routing configured correctly.**
