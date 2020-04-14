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
**Important**: Make shure that in `sudo raspi-config` "_Interfacing Options_ -> _VNC_" is enabled! Otherwise your client won't be able to connect to your Raspberry Pi.

To open a connection to your Raspberry Pi via VNC, you have to install the [RealVNC-Client](https://www.realvnc.com/de/connect/download/viewer/) on your computer or use another comatible VNC-Client. 

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
and **one** of the settings
```
# resolution: 1024x768 / 60 Hz
hdmi_mode=16
# resolution: 1280x768 / 60 Hz
hdmi_mode=23
# resolution: 1366x768 / 60 Hz
hdmi_mode=81
# resolution: 1920x1080 / 60 Hz (1080p)
hdmi_mode=82
```
at the end of the file and save it. More on the settings can be found [here](https://elinux.org/RPiconfig#Video_mode_options).

**Note**: You have to `sudo reboot` the Raspberry Pi to apply your saved the changes to the `/boot/config.txt` file.
## Program Arduino using command line
First, the packages avr-gcc (package is usually called gcc-avr), avr-libc and avrdude need to be installed. On Debian-based systems (as raspian is) the command
```
sudo apt-get install gcc-avr avr-libc avrdude
```
can be used to install the required packages. There is a detailed description of all the commands and options in the [Arduino Project](https://github.com/arduino/Arduino/blob/master/build/shared/manpage.adoc) on GitHub. Here, only the necessary commands for compiling and uploading your _arduino sketch_ are provided. The description is also taken from the [Arduino Project](https://github.com/arduino/Arduino/blob/master/build/shared/manpage.adoc).

Before you can flash your Arduino, you have to find out its serial port on the Raspberry Pi. `ls /dev/tty*` lists all serial ports of the Raspberry Pi. Usually, the Arduino takes the port `/dev/ttyACM0`, `/dev/ttyUSB0` or similar. If you are not shure, you can either try out the potential ports or compare the list with and without the Arduino connected.
### Actions
- `--verify` Build the sketch.
- `--upload` Build and upload the sketch.
- `--get-pref [preference]` Prints the value of the given preference to the standard output stream. When the value does not exist, nothing is printed and the exit status is set (see EXIT STATUS below). If no preference is given as parameter, it prints all preferences.
- `--install-boards package name:platform architecture[:version]` Fetches available board support (platform) list and install the specified one, along with its related tools. If version is omitted, the latest is installed. If a platform with the same version is already installed, nothing is installed and program exits with exit code 1. If a platform with a different version is already installed, it’s replaced.
- `--install-library library name[:version]` Fetches available libraries list and install the specified one. If version is omitted, the latest is installed. If a library with the same version is already installed, nothing is installed and program exits with exit code 1. If a library with a different version is already installed, it’s replaced. Multiple libraries can be specified, separated by a comma.
- `--version` Print the version information and exit.
### Options
- `--board package:arch:board[:parameters]` Select the board to compile for.
	- __package__ is the identifier of the vendor (the first level folders inside the 'hardware' directory). Default arduino boards use 'arduino'.
	- __architecture__ is the architecture of the board (second level folders inside the 'hardware' directory). Default arduino boards use either *arduino:avr* for all AVR-based boards (like Uno, Mega or Leonardo) or `arduino:sam` for 32bit SAM-based boards (like Arduino Due).
	- __board__ is the actual board to use, as defined in 'boards.txt' contained in the architecture folder selected. For example, `arduino:avr:uno` for the Arduino Uno, `arduino:avr:diecimila` for the Arduino Duemilanove or  Diecimila, or `arduino:avr:mega` for the Arduino Mega.
	- __parameters__ is a comma-separated list of boards specific parameters that are normally shown under submenus of the "Tools" menu. For example `arduino:avr:nano:cpu=atmega168` to Select the mega168 variant of the Arduino Nano board. 
	
	If this option is not passed, the value from the current preferences is used (e.g., the last board selected in the IDE).

- `--port portname`Select the serial port to perform upload of the sketch. On linux and MacOS X, this should be the path to a device file (e.g.,	`/dev/ttyACM0`). On Windows, this should be the name of the serial port (e.g., `COM3`). 

	If this option is not passed, the value from the current preferences is used (e.g., the last port selected in the IDE).
	
- `--verbose-build` Enable verbose mode during build. If this option is not given, verbose mode during build is *disabled* regardless of the current preferences.
- `--preserve-temp-files` Keep temporary files (preprocessed sketch, object files...) after termination. If omitted, temporary files are deleted. 

	This option is only valid together with `--verify` or `--upload`.
- `--useprogrammer` Upload using a programmer. Set if you're using an external programmer, or using the Arduino as ISP.
- `--verbose-upload` Enable verbose mode during upload. If this option is not given, verbose mode during upload is *disabled* regardless of the current preferences. This option is only valid together with `--verify` or `--upload`.
- `-v`, `--verbose` Enable verbose mode during build and upload. This option has the same effect of using both `--verbose-build` and `--verbose-upload`. 

	This option is only valid together with `--verify` or `--upload`.
- `--preferences-file filename` Read and store preferences from the specified `filename` instead	of the default one.
- `--pref name=value` Sets the preference __name__ to the given __value__. 

	Note that the preferences you set with this option are not validated: Invalid names will be set but never used, invalid values might lead to an error later on.
- `--save-prefs` Save any (changed) preferences to *preferences.txt*. In particular `--board`, `--port`, `--pref`, `--verbose`, `--verbose-build` and `--verbose-upload` may alter the current preferences.

### PREFERENCES
Arduino keeps a list of preferences, as simple name and value pairs. Below, a few of them are documented but a lot more are available.

- `sketchbook.path` The path where sketches are (usually) stored. This path can also contain some special subdirectories (see FILES below).
- `update.check` When set to true, the IDE checks for a new version on startup.
- `editor.external` When set to true, use an external editor (the IDE does not allow editing and reloads each file before verifying).
- `build.path` The path to use for building. This is where things like the preprocessed .cpp file, compiled .o files and the final .hex	file go. 
	
	If set, this directory should already exist before running the arduino command.

	If this preference is not set (which is normally the case), a new temporary build folder is created on every run and deleted again when the application is closed.

### EXIT STATUS
- `0` Success
- `1` Build failed or upload failed
- `2` Sketch not found
- `3` Invalid (argument for) commandline option
- `4` Preference passed to `--get-pref` does not exist

### FILES
The file `preferences.txt` stores the preferences used for the IDE, building and uploading sketches.
- `%LOCALAPPDATA%/Arduino15/preferences.txt` (Windows)
- `~/Library/Arduino15/preferences.txt` (Max OS X)
- `~/.arduino15/preferences.txt` (Linux)

The directory `.../Arduino` is referred to as the "Sketchbook" and contains the user's sketches. The path can be changed through the `sketchbook.path` preference.
- `My Documents/Arduino/` (Windows)
- `~/Documents/Arduino/` (Mac OS X)
- `~/Arduino/` (Linux)
	
Apart from sketches, three special directories can be inside the sketchbook:

- **libraries**: Libraries can be put inside this directory, one library per subdirectory.
- **hardware**: Support for third-party hardware can be added through this directory.
- **tools**: External code-processing tools (that can be run through the Tools menu of the IDE) can be added here.
### EXAMPLES

Start the Arduino IDE, with two files open:
```
arduino /path/to/sketch/sketch.ino /path/to/sketch/extra.ino
```
Compile and upload a sketch using the last selected board and serial port
```
arduino --upload /path/to/sketch/sketch.ino
```
Compile and upload a sketch to an Arduino Nano, with an Atmega168 CPU,
connected on port '/dev/ttyACM0':
```
arduino --board arduino:avr:nano:cpu=atmega168 --port /dev/ttyACM0 --upload /path/to/sketch/sketch.ino
```
Compile a sketch, put the build results in the 'build' directory an
re-use any previous build results in that directory.
```
arduino --pref build.path=/path/to/sketch/build --verify /path/to/sketch/sketch.ino
```
Change the selected board and build path and do nothing else.
```
arduino --pref build.path=/path/to/sketch/build --board arduino:avr:uno --save-prefs
```
Install latest SAM board support
```
arduino --install-boards "arduino:sam"
```
Install AVR board support, 1.6.2
```
arduino --install-boards "arduino:avr:1.6.2"
```
Install Bridge library version 1.0.0
```
arduino --install-library "Bridge:1.0.0"
```
Install Bridge and Servo libraries
```
arduino --install-library "Bridge:1.0.0,Servo:1.2.0"
```
### X11 error
There is an error occuring when starting `sudo arduino --upload path/sketch.ino --port /dev/ttyACM0`. [Here](https://github.com/arduino/Arduino/issues/1981) more details on it.
## Use make for programming the Arduino from command line
A tutorial can be found [here](http://www.raspberryvi.org/stories/arduino-cli.html)
