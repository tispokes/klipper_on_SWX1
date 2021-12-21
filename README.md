# How to Install Klipper on a Artillery Sidewinder X1 fast - with MainsailOS and Home Assistant
I recently struggled by findig a good and fast installation manual for Klipper with this combination.
Plus i wanted it in my HASS (HAssOS) i recently setted up.

## Before you start
1. You will need somethink like a RasPi (Raspberry Pi), i use a 3B+ with 8 GB microSD Card.
2. Your SWX1 touch display is no longer useful.
3. Decide which platform to use:
- Mainsail (nice UI) [this manual]
- Octoprint (common but overpacked - not described here)
- Fluidd (didn't looked at - not descripbed here)

### Whats "Mainsail" and "MainsailOS"?
In short: Mainsail is the Frontend (UI) and MainsailOS is a complete OS for RasPi (Operating System, Klipper, Moonraker, Mainsail)
For more Details: https://docs.mainsail.xyz/

### What is "Moonraker"?
A REST Api.

### Just to be doublecheck, what is Klipper?
A FDM 3D printer firmware like Marlin, just with a lot beneftits

## Let's go
Steps 1 to 4 are according to the MainsailOS installation guide.
With step 5 we start with SWX1 specials and varying a bit from the manual.
This is important, because from Klipper v0.9.x to v0.10.0 are some important changes for the `printers.cfg`

### 1. MainsailOS download and unpacking
Head to https://github.com/mainsail-crew/MainsailOS/releases and download the lastest Relase. Use the .zip-file or feel free to compile yourselfs.
I used v0.5.0 wich comes with:
- Klipper: v0.9.1-743-g95047784
- Moonraker: v0.7.1-41-g2450534
- Mainsail: v2.0.0
At this moment there was already Klipper v0.10.0 and Mainsail v2.1.0, buz **no problem**, with MainsailOS it's super easy to **update all components.** 
Unpack the .zip-file and you get something like "mainsailos-raspios-lite-latest.img".

### 2. Installing MainsailOS to microSD card
Install the image to the microSD card. All data will be deleted.
Use e.g. balenaEtcher or Raspberry Pi Imager
see: https://docs.mainsail.xyz/setup/mainsail-os

### 3. WiFi & SSH access
When used balenaEtcher, don't forget to manually set WiFi and check if SSH-file exist -> https://docs.mainsail.xyz/setup/mainsailos/balena-etcher

### 4. First start-up
- Turn RasPi, printer off
- Insert SD into RasPi
- Connect WebCam (if u use one) with RasPi
- Connect SWX1 via USB Port on the right side to the RasPi
- PowerUp Raspi
- Wait until you can find & reach it (http://mainsail/ or http://mainsail.local/ or check the IP manually at the router's interface)

Now you see the plain Mainsail Frontend.
The error is no problem and will be fixed soon.

### 5. Update all components
Go to "Machine" ![image](https://user-images.githubusercontent.com/45465820/146962027-f61c049a-6784-4603-ba97-3c48352ed2d8.png) and update:
- "klipper"
- "moonraker"
- "mainsail"
- "System"

This may take a while. In the meanwhile you may install a terminal programm you want to, e.g.:
- PuTTY or
- Tera Term or
- WSL (Windows Subsystem for Linux)

I use PuTTY. When the updates are finished, it looks like this ![image](https://user-images.githubusercontent.com/45465820/146962744-1b9141d7-55b4-49ad-9b4e-5df5a0722047.png)

### 6. Create "printer.cfg"
Click on ![image](https://user-images.githubusercontent.com/45465820/146962862-95c8bc63-1bab-41c0-8224-cf4ba2e26d2b.png)
and add the new file - if not present - `printer.cfg` ![image](https://user-images.githubusercontent.com/45465820/146962934-5c854d4f-7fbb-4aba-aa70-bf4628982057.png)
and click create.

### 7. Fill the "printer.cfg"
Click on `printer.cfg` and copy these 3 lines. This is important, to include the mainsail.cfg.
~~~
[include mainsail.cfg]

#SWX1

~~~
Then we need the Sidewinder X1 specific configs.
There is a Repo with some configs. I forked it added the changes for Klipper v0.10.0
Open https://github.com/tispokes/Clanks-Klipper-Configs in a new tab.

Choose the config that suits your SWX1. There a serveral options:
- SideWinder X1 BLTouch E3D Hemera.cfg -> BLTouch & Hemera Extruder, max 325 °C
- SideWinder X1 BLTouch E3D Thermistor.cfg -> BLTouch & E3D Thermistor
- SideWinder X1 BLTouch.cfg -> BLTouch
- SideWinder X1 Stock MBL.cfg -> Manual Bed Leveling
- SideWinder X1 Stock.cfg -> Stock
- SideWinder X1 Z-Endstop.cfg -> Z-Endstop as Bed Lever

Click on the file you need and then click on RAW ![image](https://user-images.githubusercontent.com/45465820/146964838-40cc93f6-537c-4246-801a-c691f394ba9d.png)
Select & copy all the text (Ctrl-A; Ctrl-C) and paste it into you `printer.cfg`after `#SWX1`.

If you are interested in the difference between Klipper v0.9.x and v0.10.0 -> see: https://github.com/tispokes/Clanks-Klipper-Configs/commit/334f693c54555fcc70fdc5e86cf304bbb6e66e51

Your config looks like this now. ![image](https://user-images.githubusercontent.com/45465820/146966168-f0de1976-a985-4463-8e53-96ecc9c49f5e.png)

**Now click "Save & Restart"**

### 8. Terminal Session
Open your favorite terminal programm e.g. PuTTY and connect to the printer. just fill in `mainsailos` and "open". ![image](https://user-images.githubusercontent.com/45465820/146966700-199c1101-a494-464e-b2af-3d408cae22b5.png)

#### 8.1 Login
Default username is `pi` and password `raspberry`.
_If it't your first time with a terminal, you can't see passwords you type in._
![image](https://user-images.githubusercontent.com/45465820/146966976-77adc97b-85de-4fd0-9951-615b7d9c4ec7.png)
No worry, unless you are not routing it from the outside it's just reachable inside your home network. But always better to change the password as mentioned from the prompt .
Follow these instructions, if you want to.
![image](https://user-images.githubusercontent.com/45465820/146967220-c85aa418-3299-4bb5-81c5-2c3557bb5226.png)

#### 8.2 Make the Firmware
Copy and paste (rightclick, *no ctrl-v in PuTTY*) this line:
`cd klipper; make menuconfig; make clean; make`
and press return.
This mask will appear ![image](https://user-images.githubusercontent.com/45465820/146967805-e1fa72f6-9793-4436-a242-1ce95d1ad61c.png)
 and you have to check (spacebar) the Ènable extra low-level configuration options`. The rest should be set correctly and check that you have the same settings as me.
 ![image](https://user-images.githubusercontent.com/45465820/146968002-dc3c70bd-ecd6-4da3-9fcc-ccaa49c880c4.png)
 Quit (Esc) and Save (y). Now the firmware is being compiled.
 ![image](https://user-images.githubusercontent.com/45465820/146968248-40cd6b7b-65a0-4c36-900b-1018bb2adb8b.png)
Nice, done!

#### 8.3 Find your printers USB port.
`ls /dev/serial/by-id/\*` ![image](https://user-images.githubusercontent.com/45465820/146968758-9a7085f0-3ca3-44ed-be72-00e71e292a22.png)

**When there is only one line,** like in the picture execute this command `PRINTER=$(ls /dev/serial/by-id/\*)`
**When you have multiple lines:** Mark the line, in my example `/dev/serial/by-id/usb-1a86_USB_Serial-if00-port0`. When using PuTTY, it'S instant in your clipboard. Write `PRINTER=$(`, paste your clipboard and add a closing bracket `)`. Return.

#### [OPTIONAL]8.4 Save existing firmware from your printer
`avrdude -patmega2560 -cwiring -P $PRINTER -F -Uflash:r:dump.hex:i`

#### 8.5 Write firmware
Simply type (better copy ;-) ) `make flash FLASH_DEVICE=$PRINTER`
![image](https://user-images.githubusercontent.com/45465820/146972858-e9cb0c68-11ae-41a1-8e9b-e7d02d64133c.png)

**Done!**

Press "_Restart_" in your Mainsail Webinterface and wait some seconds.

### 9 Leveling
Depends on what Bedlevel you use.


#### 10. Connect to Home Assiatant
