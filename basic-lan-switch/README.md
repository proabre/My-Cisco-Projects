# Basic LAN Connectivity — Cisco Packet Tracer

## Overview

This project demonstrates a basic Local Area Network (LAN) created using **Cisco Packet Tracer**.

The network consists of two laptops connected to a single network switch. Both devices are configured with IP addresses within the same network, allowing them to communicate with each other.

The connectivity between the two laptops was verified using the **ping** command.

## Network Topology

```text
Laptop 1
192.168.1.1
     │
     │
     ▼
  Switch
     ▲
     │
     │
Laptop 2
192.168.1.2
```

## Devices Used

* 2 × Laptops
* 1 × Cisco Switch
* Ethernet connections

## IP Addressing

| Device   | IP Address     | Subnet Mask     |
| -------- | -------------- | --------------- |
| Laptop 1 | `192.168.1.10` | `255.255.255.0` |
| Laptop 2 | `192.168.1.20` | `255.255.255.0` |

Both laptops are on the same subnet:

```text
192.168.1.0/24
```

## Configuration

### Laptop 1

```text
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
```

### Laptop 2

```text
IP Address: 192.168.1.20
Subnet Mask: 255.255.255.0
```

No default gateway is required for communication between these two devices because they are on the same local network.

## Connectivity Test

Connectivity was tested from Laptop 1 using and:

```bash
ping 192.168.1.20
```
Connectivity was tested from Laptop 2 using:

```bash
ping 192.168.1.10
```

A successful ping confirms that Laptop 1 can communicate with Laptop 2 through the switch.

Example:

```text
Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
```

## What I Learned

This project helped demonstrate the fundamentals of:

* Local Area Networks (LANs)
* Ethernet connectivity
* IP addressing
* Subnet masks
* Network switches
* Same-subnet communication
* Using `ping` to test network connectivity

## Project Files

```text
.
├── topology.pkt
├── README.md

```

## Tools

* **Cisco Packet Tracer**
* Basic networking and TCP/IP concepts

## Conclusion

This project demonstrates a simple switched LAN in which two laptops successfully communicate with each other using IP addressing and Ethernet connectivity. The successful ping test verifies basic network connectivity between the devices.

