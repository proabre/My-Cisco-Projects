# Cisco Packet Tracer --- Network Cabling Lab

## Overview

This lab practices selecting the correct cable type for connecting
network devices in Cisco Packet Tracer.

For this exercise, assume:

-   **Auto MDI-X is disabled or not supported.**
-   Packet Tracer does not distinguish between single-mode and multimode
    fiber, so choose the appropriate fiber type based on the distance.
-   Copper UTP supports distances of up to **100 meters** in this
    simplified lab.

------------------------------------------------------------------------

## Cable Selection Rules

  Connection                 Cable
  -------------------------- -------------------------
  PC ↔ Switch                Copper straight-through
  Server ↔ Switch            Copper straight-through
  Router ↔ Switch            Copper straight-through
  PC ↔ PC                    Copper crossover
  Switch ↔ Switch            Copper crossover
  Router ↔ Router            Copper crossover
  Long-distance connection   Fiber optic

### Why?

With Auto MDI-X disabled:

-   PCs and routers transmit on pins **1 and 2** and receive on pins **3
    and 6**.
-   Switches transmit on pins **3 and 6** and receive on pins **1 and
    2**.

Therefore:

-   **Different device types** generally use **straight-through**
    cables.
-   **Same device types** generally use **crossover** cables.

------------------------------------------------------------------------

## Distance and Fiber

### Copper UTP

Maximum distance used in this lab:

**100 meters**

Example:

``` text
Router ───────── 50 m ───────── Router
```

A copper crossover cable is appropriate because the routers are the same
device type and the distance is within 100 meters.

### Multimode Fiber

Multimode fiber is appropriate for distances of **hundreds of meters**.

Example:

``` text
Router ───────── 250 m ───────── Router
```

250 meters is too far for the assumed 100-meter UTP limit, but it is
within the range of multimode fiber.

### Single-mode Fiber

Single-mode fiber is used for **kilometer-scale distances**.

Example:

``` text
Router ───────────── 3 km ───────────── Router
```

3 km is too far for UTP and beyond the range assumed for multimode
fiber, so single-mode fiber is appropriate.

------------------------------------------------------------------------

## Lab Connections

### Copper Straight-Through

``` text
PC1  → SW3
PC2  → SW4
PC3  → SW7
SRV1 → SW8

SW1 → R2
SW2 → R2
SW5 → R4
SW6 → R4
```

### Copper Crossover

``` text
SW3 → SW1
SW1 → SW2
SW4 → SW2
SW7 → SW5
SW5 → SW6
SW8 → SW6

R2 → R1   (50 m)
```

### Fiber

``` text
R1 → R3   (3 km)    → Single-mode fiber
R3 → R4   (250 m)   → Multimode fiber
```

------------------------------------------------------------------------

## Quick Decision Process

When choosing a cable, follow these steps:

### 1. Identify the devices

Ask:

``` text
What device is connected to what?
```

### 2. Determine whether the devices are the same type

``` text
Different device types → Straight-through
Same device types      → Crossover
```

### 3. Check the distance

``` text
≤ 100 m       → Copper UTP
Hundreds of m → Multimode fiber
Several km    → Single-mode fiber
```

### 4. Choose the cable

Example:

``` text
Router ↔ Router
Distance = 50 m

Same device type
        ↓
Crossover
        ↓
50 m is within 100 m
        ↓
Copper crossover
```

------------------------------------------------------------------------

## Important Concepts

### Straight-Through

A straight-through cable connects transmit pins to the appropriate
receive pins when connecting different device types.

``` text
PC ───── Straight-through ───── Switch
```

### Crossover

A crossover cable swaps the transmit and receive pairs.

``` text
Switch ───── Crossover ───── Switch
```

### Multimode vs Single-mode

**Multimode fiber**

-   Shorter distances
-   Commonly used for hundreds of meters
-   Suitable for the 250-meter connection in this lab

**Single-mode fiber**

-   Much longer distances
-   Suitable for kilometer-scale links
-   Suitable for the 3-kilometer connection in this lab

------------------------------------------------------------------------

## Important Real-World Note

Modern Ethernet equipment commonly supports **Auto MDI-X**, which
automatically detects and corrects the transmit/receive pair
configuration.

Therefore, the strict straight-through-versus-crossover rules are mainly
important for understanding this Packet Tracer exercise and traditional
Ethernet behavior.

------------------------------------------------------------------------

## Key Takeaway

Remember these three rules:

> **Different device types → straight-through.**

> **Same device types → crossover.**

> **Long distance → choose the appropriate fiber type.**

For this lab:

``` text
≤ 100 m       → Copper
250 m         → Multimode fiber
3 km          → Single-mode fiber
```
