Currently available access options
==================================

Regular webinterface
--------------------

The default interface, running on `192.168.2.1` seems mostly functional. Default password-only login using "admin" is functional, you can browse around, edit config values, etc. Uploading a new firmware however results in an authorisation error, and trying to complete a full setup will not properly complete.

Serial interface
----------------

Soldering the GND/TX/RT pins on the board enables a serial interface (using an USB-serial FTDI-chip). Communication is at 115200 baud, pressing `Enter` early in the boot process will give you the *primary bootloader CLI*, and completing the full boot process will result in the *standard IGD CLI*, using `administrator` (password `admin`) as login.

Telnet interface
----------------

When fully booted, you have a telnet interface running on the default port. Same login and same *standard IGD CLI* as through the *serial interface*.

Primary bootloader CLI
----------------------

If sufficiently fast in pressing `Enter` while booting, you get a low-level CLI in the primary bootloader stage.

Here you have a set of commands that allow you to set core environment variables (TFTP boot ip, Flash memory protection, etc.) and flash memory read/write commands. Here you could possibly override the final firmware that gets loaded by the second bootloader.

Standard IGD CLI
----------------

Completing the bootprocess will give you a IGD-prompt (not clear what that is). Same login as above. The `help` command shows a lot of possible commands, but the usefulness is not yet clear. 

Executing the "hidden command" `sx763os` in this environment will drop you into the Busybox CLI.

Busybox CLI
-----------

Gives you a regular linux shell, filesystem, etc. Available commands are limited to the set included by Busybox (includes `netcat`). You can poke around in the internals, access config and log files, `dd`, etc.
