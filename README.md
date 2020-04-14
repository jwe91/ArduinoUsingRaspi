# ArduinoUsingRaspi
## Install Arduino on Raspberry
The easiest way to install the _Arduino IDE_ on a Raspberry Pi is to use apt:
```
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install arduino
```
## Program Arduino using command line
First, the packages avr-gcc (package is usually called gcc-avr), avr-libc and avrdude need to be installed. On Debian-based systems (as raspian is) the command
```
sudo apt-get install gcc-avr avr-libc avrdude
```
can be used to install the required packages.
