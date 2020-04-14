# ArduinoUsingRaspi
## Install Arduino on Raspberry
The easiest way to install the _Arduino IDE_ on a Raspberry Pi is to use apt:
```
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install arduino
```
## Program Arduino using GUI
Once the _Arduino IDE_ is installed on your Raspberry Pi, you can open a remote Desktop connection and then use the _Arduino IDE_ as with any other Computer. 
### XRDP
For _Windows_ users, the easiest way is to use **xrdp**. On newer Raspberian versions, the installed _RealVNC-Server_ has to be uninstalled first.
```
sudo apt-get purge realvnc-vnc-server
```
The _xrdp-server_ can be installed using apt: 
```
sudo apt-get install xrdp
```
With
```
sudo systemctl status xrdp
```
you can check the status of the _xrdp-server_ and see, whether it is running properly.
## Program Arduino using command line
First, the packages avr-gcc (package is usually called gcc-avr), avr-libc and avrdude need to be installed. On Debian-based systems (as raspian is) the command
```
sudo apt-get install gcc-avr avr-libc avrdude
```
can be used to install the required packages.
