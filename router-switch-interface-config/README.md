# CCNA Router and Switch Interface Configuration Lab
# Overview

This lab demonstrates the basic configuration of a Cisco router, two switches, and four PCs connected to a single LAN.

The LAN uses the network:

172.16.0.0/16

The lab focuses on:

Configuring hostnames
Configuring IP addresses
Configuring speed and duplex
Adding interface descriptions
Disabling unused interfaces
Saving device configurations
Network Devices
Device	Role	Address
R1	Router	172.16.255.254/16
SW1	Switch	Layer 2 switch
SW2	Switch	Layer 2 switch
PC1	End host	172.16.0.1/16
PC2	End host	172.16.0.2/16
PC3	End host	172.16.0.3/16
PC4	End host	172.16.0.4/16

Default gateway for all PCs: 172.16.255.254

R1 Configuration
enable
configure terminal


hostname R1


interface g0/0
ip address 172.16.255.254 255.255.0.0
speed 1000
duplex full
description ## to SW1 ##
no shutdown


interface range g0/1 - 2
description ## not in use ##


end


copy running-config startup-config
SW1 Configuration
enable
configure terminal


hostname SW1


interface g0/1
speed 1000
duplex full
description ## to R1 ##


interface g0/2
speed 1000
duplex full
description ## to SW2 ##


interface range f0/1 - 2
description ## to end hosts ##


interface range f0/3 - 24
description ## not in use ##
shutdown


end


write memory
SW2 Configuration
enable
configure terminal


hostname SW2


interface g0/1
speed 1000
duplex full
description ## to SW1 ##


interface range f0/1 - 2
description ## to end hosts ##


interface range g0/2, f0/3 - 24
description ## not in use ##
shutdown


end


write
PC Configuration

Configure the following addresses on the PCs:

PC1: 172.16.0.1 /16
PC2: 172.16.0.2 /16
PC3: 172.16.0.3 /16
PC4: 172.16.0.4 /16

Subnet mask:

255.255.0.0

Default gateway:

172.16.255.254
Verification Commands
Router
show ip interface brief
show running-config
show startup-config
Switches
show interfaces status
show running-config
show startup-config

These commands verify interface status, IP addressing, speed, duplex, descriptions, and saved configuration.

Important Concepts
Router Interfaces

Router interfaces are administratively down by default. An active interface therefore requires:

no shutdown
Switch Interfaces

Switch ports are normally enabled by default. Unused ports should be disabled:

shutdown
Speed and Duplex

The lab manually configures connected GigabitEthernet interfaces:

speed 1000
duplex full

Both ends of a connection should have compatible settings.

Running vs Startup Configuration
Running-config: Configuration currently active in RAM.
Startup-config: Configuration saved in NVRAM and loaded after a reboot.

Save the running configuration using:

copy running-config startup-config

or:

write memory

or:

write
Packet Tracer Note

Packet Tracer may display manually configured interfaces as a-1000 and a-full in some output, even though the speed and duplex were manually configured. This behavior can differ from a physical Cisco device.

Learning Objectives

After completing this lab, you should be able to:

Configure Cisco device hostnames
Configure IPv4 addresses on router interfaces
Configure Ethernet speed and duplex
Add meaningful interface descriptions
Identify and disable unused switch ports
Verify interface configurations
Save configurations to startup-config
Understand the difference between router and switch interface defaults
