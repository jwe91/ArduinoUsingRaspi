# ArduinoUsingRaspi
## Install Arduino on Raspberry
The easiest way to install the _Arduino IDE_ on a Raspberry Pi is to use apt:
```
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install arduino
```
## Program Arduino using GUI
Once the _Arduino IDE_ is installed on your Raspberry Pi, you can open a remote Desktop connection and then use the _Arduino IDE_ as with any other Computer. 

**Note**: Before you can start a remote desktop connection to your Raspberry Pi, you have to enable (if not already enabled) boot to desktop. To do so, type
```
sudo raspi-config
```
and select **Enable boot to desktop/scratch**. Also, in the _raspi-config_, it is recommended to go to "_Advanced Options_ -> _Memory Split_" and set the value to **64 (MByte)**. 
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
### RealVNC
RealVNC works for Linux, Mac and Windows. It can be installed installed on the Raspberry Pi using apt:
```
sudo apt-get install realvnc-vnc-server
```
To open a connection to your Raspberry Pi via VNC, you have to install the _RealVNC-Client_ on your computer or use another comatible VNC-Client. 

Often the resolution choosen by the Raspberry Pi (matching the connected HDI-Display or a standard value, if no display connected) is not suitable for working purposes. Therefore, you may choose a resolution, that fits to your display in use.
#### Set resolution through raspi-config
Type
```
sudo raspi-config
```
and select a compatible resolution under „_Advanced Options / Resolution_“.
#### Set a fix value for the resolution
There are two modi (CEA and DVT) for monitors. You can display the supported resolutions for CEA and DVT with
```
/opt/vc/bin/tvservice -m CEA
/opt/vc/bin/tvservice -m DVT
```
and the resolution of the current monitor with:
```
/opt/vc/bin/tvservice -s
```
For our use, the **DVT** mode is relevant. To set fix resolution open
```
sudo nano /boot/config.txt
```
and insert the lines
```
# enable HDMI without monitor connected (optional)
hdmi_force_hotplug=1
# Output audio through HDMI (optional)
hdmi_drive=2
# enable DMT-mode (required)
hdmi_group=2
```
at the end of the file.
## Program Arduino using command line
First, the packages avr-gcc (package is usually called gcc-avr), avr-libc and avrdude need to be installed. On Debian-based systems (as raspian is) the command
```
sudo apt-get install gcc-avr avr-libc avrdude
```
can be used to install the required packages.
