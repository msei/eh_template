# Energy Havester Project Template

This is a template repository for the [Energy Harvester LoRaWAN Node](https://github.com/ablexOnGithub/energyharvester) by @ablexomat.
It serves as a starting point for the board, its AVRmega 328pb and the RFM95 Module with some of the necessary boilerplate.

**Warning** I take no responsibility for this code and possible damages from it. Please verify for yourself if it does what you expect. Also I have not had access to the hardware yet though this might change soon. If you find bugs, do not keep them to yourself. Please raise an issue and send a pull request if you find some way to improve things.

## Prerequisits
This template is built on [platformIO](https://platformio.org/). So I suggest you install it on an [IDE that supports it](https://platformio.org/platformio-ide) to get code completion and other nice features.
Then use git or the version control feature of vsCode to clone the template repository from github.
```sh
$ git clone https://github.com/msei/eh_template
```
or in vsCode use the hotkey
Ctrl+Shift+p --> Git: clone --> https://github.com/msei/eh_template

## Configuration
Open the cloned directory in your IDE. In vsCode it should be detected as a platformIO project.
To get an overview of what is already there, take a look at the platformio.ini. This is the central configuration file for your project. The hardware used, build tools, libraries will be declared.
```ini
[env:ATmega328PB]
  platform = atmelavr
  board = ATmega328PB
  framework = arduino

  ; change MCU frequency
  ;board_build.f_cpu = 16000000L
  board_build.f_cpu = 8000000L
```
As you can see, some things are already taken care of. The MCU, board, platform are set up for the hardware of the energy harvester board and the platform and framework are set respectively. The MCU's clockrate is set to 8MHz.
```ini
  lib_deps =
    ;${common_env_data.lib_deps_builtin}
    ;${common_env_data.lib_deps_external}
    MCCI LoRaWAN LMIC library@>=3.0.99
    ;LMIC-Arduino
    ;OneWire@>=2.3.5
    ;Adafruit Unified Sensor@>=1.0.3
    ;Adafruit BME280 Library@>=1.0.10
    ;ArduinoJson@>=6.13.0
    ;CayenneLPP@>=1.0.3
```
The library dependencies are managed by platformIO's dependency manager. Some of the necessary or common libraries are mentioned, even so most are items commented out.

```ini
  build_flags =
    -D ARDUINO_LMIC_PROJECT_CONFIG_H_SUPPRESS
    -D CFG_eu868=1
    -D CFG_sx1276_radio=1
    -D DISABLE_BEACONS
    -D DISABLE_PING
    -D LMIC_ENABLE_DeviceTimeReq=0

  lib_ldf_mode = chain+
```
The LoRaWAN library (mcci-arduino-lmic) is configured to use the 868MHz spectrum used in europe. 
As the ATmega328PB MCU is rather low on flash and RAM compared to some of its 32bit counterparts, hence some tweaking of the LoRaWAN library hast been done in the build flags. Support for Class B features is disabled and thus not linked into the project during build. More about that can be found [here](https://github.com/mcci-catena/arduino-lmic).

In the directory *src* there is a main.cpp file prefilled with some expample code from the LoRaWAN library. To enable your board to communicate with [TheThingsNetwork](https://www.thethingsnetwork.org/), log into the [TTN Console](https://console.thethingsnetwork.org/) (sign up if necessary), create an application and register a new device. Once done, copy the respective settings (device and application eui in lsb, app key in msb order) to their respective constant declarations in main.cpp. 

## Building  and uploading the project
Select *Command Palette* from the *View* menu and select platformio: build or simply or hit the *Ctrl+Shift+p* hotkey. The dependencies will be pulled from their respective sources and the project will be built. If you have made changes to the platformio.ini file, run the PlatformIO: clean task befor building the project.
To upload the binary to the energy harvester, connect the board via your ftdi cable. Then hit PlatformIO: Upload from the Command Palette or *Ctrl+Alt+u*. The binary will be uploaded into the energy harvester board and the board will be reset.

## Todo
* ~~Put in the correct pinmap for the rfm95~~
* Read VCC on A0 and adapt sleep cycle
* Enable/disable functions for Vcc on the Grove sockets

