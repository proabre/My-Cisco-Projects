README — Cisco IOS CLI Basic Configuration Lab
Overview

This lab introduces the Cisco IOS Command-Line Interface (CLI) using Cisco Packet Tracer.

The objective is to practice basic configuration tasks on a Cisco router and switch, including:

Configuring hostnames
Accessing privileged EXEC mode
Accessing global configuration mode
Configuring an enable password
Encrypting passwords
Configuring an enable secret
Viewing the running configuration
Saving the configuration
Viewing the startup configuration
Using command shortcuts and do

Practice recommendation: Complete the same configuration steps on both R1 and SW1 to build familiarity with Cisco IOS commands.

Lab Topology

The lab contains:

Router: R1
Switch: SW1
Several PCs

The configurations demonstrated in the lab are primarily performed on R1, but the same concepts should be practiced on SW1.

Cisco IOS CLI Modes

Cisco IOS uses different command modes.

User EXEC Mode

Prompt:

Router>

This is the initial mode after accessing the device.

Privileged EXEC Mode

Prompt:

Router#

Enter this mode with:

enable

Shortcut:

en
Global Configuration Mode

Prompt:

Router(config)#

Enter this mode with:

configure terminal

Shortcut:

conf t
Mode Navigation
Router> enable
Router# configure terminal
Router(config)#

To return to the previous mode:

exit
Step 1 — Configure Hostnames

The default hostname is usually:

Router

Configure R1:

enable
configure terminal
hostname R1

The prompt should change to:

R1(config)#

Configure SW1 similarly:

enable
configure terminal
hostname SW1
Step 2 — Configure the Enable Password

The enable password command protects access to privileged EXEC mode.

From global configuration mode:

enable password CCNA

The configured password is:

CCNA
Step 3 — Test the Enable Password

Return to user EXEC mode:

exit
exit

The prompt should be:

R1>

Enter privileged EXEC mode:

enable

The device will request the password.

Enter:

CCNA

If the password is correct, you will enter:

R1#

Incorrect passwords will result in an authentication failure.

Step 4 — View the Running Configuration

The running configuration contains the configuration currently active on the device.

From privileged EXEC mode:

show running-config

Shortcut:

show run

Example:

R1# show running-config

You should initially see the enable password in clear text:

enable password CCNA
Important

The running configuration is stored in RAM.

If you make changes and restart the device without saving them, those changes can be lost.

Step 5 — Enable Password Encryption

From global configuration mode:

configure terminal
service password-encryption

This enables encryption for passwords that would otherwise appear in clear text in the configuration.

Step 6 — Verify Password Encryption

You can return to privileged EXEC mode:

exit

Then:

show running-config

Alternatively, use do from global configuration mode:

do show running-config

The password should now appear similar to:

enable password 7 <encrypted-string>

The 7 indicates Cisco Type 7 password obfuscation.

Important

Type 7 is not considered strong encryption. It mainly prevents someone from immediately reading the password in the configuration.

Step 7 — Configure an Enable Secret

A more secure method is to configure an enable secret.

From global configuration mode:

enable secret Cisco

The password is:

Cisco

The enable secret takes precedence over the enable password.

Step 8 — Test the Enable Secret

Return to user EXEC mode:

exit
exit

Then:

enable

Try the old password:

CCNA

It will no longer work when an enable secret is configured.

Now enter:

Cisco

You should successfully enter privileged EXEC mode.

Key Rule

If both are configured:

enable password CCNA
enable secret Cisco

The enable secret is used for privileged EXEC authentication.

Step 9 — View the Passwords

Use:

show running-config

You should see entries similar to:

enable password 7 <encrypted-string>
enable secret 5 <encrypted-string>

The numbers identify the password types shown in this lab:

Type	Example	Description
Type 7	enable password 7 ...	Cisco password obfuscation
Type 5	enable secret 5 ...	MD5-based secret format

Modern Cisco IOS versions may support newer password-hashing mechanisms. The exact format depends on the IOS version and configuration.

Step 10 — Save the Configuration

The running configuration must be copied to the startup configuration if you want it to survive a reboot.

There are three common commands:

Method 1
write
Method 2
write memory

Shortcut:

wr
Method 3
copy running-config startup-config

The third method explicitly copies:

running-config → startup-config
Verify the Startup Configuration

Use:

show startup-config

The saved configuration should contain the settings that were copied from the running configuration.

Important Commands
Command	Purpose
enable	Enter privileged EXEC mode
configure terminal	Enter global configuration mode
hostname R1	Change device hostname
enable password CCNA	Configure an enable password
service password-encryption	Obfuscate applicable plaintext passwords
enable secret Cisco	Configure an enable secret
show running-config	Display active configuration
show startup-config	Display saved configuration
do show running-config	Run an EXEC command from configuration mode
write	Save configuration
write memory	Save configuration
copy running-config startup-config	Save running configuration
Useful CLI Shortcuts

Cisco IOS allows abbreviated commands when the abbreviation is unambiguous.

For example:

en

is enough for:

enable

And:

conf t

is enough for:

configure terminal

Similarly:

sh run

can be used for:

show running-config
Ambiguous Commands

A command must be abbreviated enough to uniquely identify it.

For example:

e

is ambiguous because both:

enable
exit

begin with e.

You can use:

e?

to see possible commands beginning with e.

Using ? and Tab Completion

Cisco IOS provides built-in command help.

Example:

R1# e?

The device displays possible commands.

You can also use Tab to automatically complete an unambiguous command.

For example:

R1# en<Tab>

becomes:

R1# enable
Running Configuration vs Startup Configuration

This is an important CCNA concept.

Configuration	Stored In	Purpose
Running configuration	RAM	Currently active configuration
Startup configuration	NVRAM	Configuration loaded after reboot

The basic workflow is:

Configure device
      ↓
Running-config
      ↓
copy running-config startup-config
      ↓
Startup-config

If you configure a device but do not save the configuration, a reboot can cause the unsaved changes to disappear.

Complete Configuration Example

A basic R1 configuration from this lab would look like:

enable
configure terminal
hostname R1
enable password CCNA
service password-encryption
enable secret Cisco
exit
exit
show running-config
copy running-config startup-config
show startup-config
Key CCNA Takeaways
User EXEC mode uses >.
Privileged EXEC mode uses #.
enable enters privileged EXEC mode.
configure terminal enters global configuration mode.
hostname changes the device hostname.
enable password provides a basic privileged EXEC password.
service password-encryption obfuscates applicable plaintext passwords.
Enable secret takes precedence over enable password.
show running-config displays the active configuration.
show startup-config displays the saved configuration.
do allows certain EXEC commands to be executed from configuration mode.
The running configuration is stored in RAM.
The startup configuration is stored in NVRAM.
Use copy running-config startup-config to save changes.
Cisco IOS accepts command abbreviations when they are unambiguous.
? provides context-sensitive CLI help.
Type 7 password obfuscation is not strong cryptographic protection.
The lab's Type 5 enable secret example uses an MD5-based format; newer IOS versions may provide stronger secret/hash options.
