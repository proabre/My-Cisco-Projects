# Cisco Router IP Address Configuration Lab
## Lab Objectives

In this lab, you will configure a Cisco router and three PCs in Packet Tracer. You will practice basic Cisco IOS commands, interface configuration, IP addressing, configuration verification, and connectivity testing.

## Tasks
## 1. Configure R1's hostname

Enter privileged EXEC mode:

Router> enable
Router#

Enter global configuration mode:

R1# configure terminal
R1(config)#

Configure the hostname:

R1(config)# hostname R1
R1(config)#

The router prompt should now display R1.

## 2. View R1's interfaces

Use:

R1# show ip interface brief

This command displays:

Interface names
IP addresses
Interface status
Protocol status

Initially, the interfaces may show:

administratively down

This is because Cisco router interfaces are disabled by default.

If you are in configuration mode, you can use:

R1(config)# do show ip interface brief
3. Configure R1's interfaces
GigabitEthernet 0/0
R1(config)# interface g0/0
R1(config-if)# ip address 15.255.255.254 255.0.0.0
R1(config-if)# description ## to SW1 ##
R1(config-if)# no shutdown
GigabitEthernet 0/1
R1(config-if)# interface g0/1
R1(config-if)# ip address 182.98.255.254 255.255.0.0
R1(config-if)# description ## to SW2 ##
R1(config-if)# no shutdown
GigabitEthernet 0/2
R1(config-if)# interface g0/2
R1(config-if)# ip address 201.191.20.254 255.255.255.0
R1(config-if)# description ## to SW3 ##
R1(config-if)# no shutdown
Interface addressing
Interface	IP Address	Subnet Mask	Network
G0/0	15.255.255.254	255.0.0.0	15.0.0.0/8
G0/1	182.98.255.254	255.255.0.0	182.98.0.0/16
G0/2	201.191.20.254	255.255.255.0	201.191.20.0/24

## 4. Verify R1's interfaces

Return to privileged EXEC mode:

R1(config-if)# end

Then:

R1# show ip interface brief

The interfaces should show:

Interface              IP-Address         Status       Protocol
GigabitEthernet0/0     15.255.255.254    up           up
GigabitEthernet0/1     182.98.255.254    up           up
GigabitEthernet0/2     201.191.20.254    up           up

The important result is:

up/up

up/up means the interface and its protocol are operational.

## 5. View and save the configuration

View the running configuration:

R1# show running-config

Shortcut:

R1# show run

Check that the interfaces contain the correct:

IP addresses
Subnet masks
Descriptions
no shutdown

Save the configuration:

R1# copy running-config startup-config

You can also use:

R1# write

The difference is:

running-config
↓
save
↓
startup-config

The running configuration is currently active. The startup configuration is loaded when the router reboots.

## 6. Configure PC1, PC2, and PC3

In Packet Tracer, select each PC:

PC → Config → FastEthernet0
PC1
IP Address:       15.0.0.1
Subnet Mask:      255.0.0.0
Default Gateway:  15.255.255.254
PC2
IP Address:       182.98.0.1
Subnet Mask:      255.255.0.0
Default Gateway:  182.98.255.254
PC3
IP Address:       201.191.20.1
Subnet Mask:      255.255.255.0
Default Gateway:  201.191.20.254


## 7. Test connectivity

On PC1, open:

PC1 → Desktop → Command Prompt

Ping PC2:

ping 182.98.0.1

Then ping PC3:

ping 201.191.20.1

Successful replies indicate that PC1 can communicate with PC2 and PC3 through R1.

## Complete R1 Configuration
enable
configure terminal


hostname R1


interface g0/0
ip address 15.255.255.254 255.0.0.0
description ## to SW1 ##
no shutdown


interface g0/1
ip address 182.98.255.254 255.255.0.0
description ## to SW2 ##
no shutdown


interface g0/2
ip address 201.191.20.254 255.255.255.0
description ## to SW3 ##
no shutdown

end

show ip interface brief
show running-config
copy running-config startup-config
Verification Checklist
