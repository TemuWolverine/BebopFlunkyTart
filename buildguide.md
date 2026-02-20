# Building "The Bebop Flunky Tart"
<!-- run 'doctoc .' from bash>
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Hardware](#hardware)
  - [Parts list](#parts-list)
  - [Printing](#printing)
    - [Filament](#filament)
    - [Printing Post Processing](#printing-post-processing)
  - [TODO: Inserts](#todo-inserts)
  - [TODO: Back plate](#todo-back-plate)
  - [Raspberry Pi](#raspberry-pi)
  - [TODO: Screen](#todo-screen)
  - [Heatsink Fan](#heatsink-fan)
  - [Amp 4 Pro](#amp-4-pro)
  - [TODO: Installing the Pi+Screen+Amp4Pro into the shell](#todo-installing-the-piscreenamp4pro-into-the-shell)
  - [Rotary Encoder/GPIO](#rotary-encodergpio)
- [Software](#software)
  - [Option1: Raspbian](#option1-raspbian)
    - [Touch Gestures (like scrolling)](#touch-gestures-like-scrolling)
  - [TODO: Option2: DRM (Direct Rendering Manager)](#todo-option2-drm-direct-rendering-manager)
  - [TODO: Player: WateryTart](#todo-player-waterytart)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
# Hardware
## [Parts list](readme.md)

## Printing
![alt text](images/bambuslicer.png)  

Proconfigured 3MF files are available, but if you'd like to control it all yourself, here are some recommendations

* Infill: Gyroid
* Brim: 5mm
* Print the shell with the front panel facing down. You won't need any supports on the exterior of the shell (it's only a 25 degree angle), but you'll likely need some supports on the inside for where the back panel mounts.
* Print the backpanel outside face down. If you're printing with the labels using an AMS or similar, it'll only need 4 filament swaps for the text, so it's relatively efficient with purge waste

### Filament
PLA is just fine, PETG would work too. I used Bambu's matte green PLA, and Jayo Black and White matte PLAs

### Printing Post Processing
Apart from removing the supports, there isn't a lot of post processing needed. If you have sharp edges from the brim, I recommend using a deburring tool. This can also be used to open up any holes if they printed too small

![alt text](images/deburring.png)

## TODO: Inserts
I find 320c on my soldering iron works great for installing the inserts. The inserts should be installed flush or slightly below the surface, not slightly above.

## TODO: Back plate 

## Raspberry Pi
Install the microSD card now - it'll be a pain in the butt to get to later.


## TODO: Screen


## Heatsink Fan
Follow the instructions that come with the heatsink/fan. If you pick the GeeekPi Armor Lite V5, it's pretty straight forward - install some thermal pads in various thicknesses over the chips, then screw down the heatsink, plug in the fan to the fan header.

## Amp 4 Pro
To give the Amp 4 Pro board some clearance over the fan (and to allow general air flow), first install a 40pin header riser onto the Raspberry Pi.
With that in, you'll need to use larger standoffs than what come in the Amp4Pro package. 

You should now connect all the wires to the board - 2x power, 4x speaker wires.

Plug in the ethernet cable

## TODO: Installing the Pi+Screen+Amp4Pro into the shell


## Rotary Encoder/GPIO
The Hifiberry boards use a bunch of pins, so only specific ones are still available despite the full header available on the Amp4Pro. [From the documentation](https://www.hifiberry.com/docs/hardware/gpio-usage-of-hifiberry-boards/)

> **GPIO2-3 (pins 3 and 5)** are used by our products for configuration. If you are experienced with I2C, you might add other slave devices. If you a a novice, we don’t recommend this at all.  
> 
> **GPIOs 18-21 (pins 12, 35, 38 and 40)** are used for the sound interface. You can’t use these for any other purpose.  
>
> **GPIO4** is used to control the MUTE function of the power stage. Pulling it to low will mute the output.

For the single device (rotary encoder), we need 2 data pins plus GND and 3v3.

![Pin out](images/image-1.png)

Note that GPIO numbers don't line up with the physical pin numbers.

Plug into + (or perhaps VIN) into pin 1 (3v3 power), GND into pin 9 (gnd). From the above Hifiberry documents, we can see GPIO17 and GPIO27 are free, so plug into CLK into pin 11 and DT into pin 13.

If you want to use the push button function of the KY-040, plug SW into GPIO22/Pin 22.


# Software
## Option1: Raspbian
Open up **/boot/firmware/config.txt** (`sudo nano /boot/firmware/config.txt`) and add
```
dtoverlay=hifiberry-amp4pro
```
This sets up the amp4pro to work with the built in drivers.

Find the `dtoverlay=vc4-kms`.. line and change it to 

```
dtoverlay=vc4-kms-v3d,noaudio
```
This will disable the built in audio (HDMI), for best results we want a single audio device.

Save the file, then reboot (`sudo reboot`).

You can test it by running the command `aplay -l`. You should get a result like the following

```bash
paul@raspbiantest:~ $ aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: sndrpihifiberry [snd_rpi_hifiberry_dacplus], device 0: HiFiBerry AMP4 Pro HiFi pcm512x-hifi-0 [HiFiBerry AMP4 Pro HiFi pcm512x-hifi-0]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```
### Touch Gestures (like scrolling)
The stock Raspbian Desktop Environment does not include any handling of touch based scrolling, unless you tap the scroll bar.

I went with KDE Plasma, [using this guide](https://lucstechblog.blogspot.com/2025/10/raspberry-os-bookworm-with-kde-desktop.html).
TL;DR version:

```bash
sudo apt update && sudo apt full-upgrade -y
sudo apt install kde-full -y
sudo reboot
```

```bash
sudo apt install kde-plasma-desktop sddm -y
sudo dpkg-reconfigure sddm
sudo reboot
```

If you want to completely remove the Raspbian DE (Pixel) - this is optional
```bash
sudo apt purge lxde* lightdm* openbox* -y
sudo apt purge raspberrypi-ui-mods -y
sudo apt autoremove --purge -y
sudo apt clean
sudo reboot.
```

KDE Plasma supports touch out of the box, but you may want to install a virtual keyboard.
```bash
sudo apt-get install maliit-keyboard
```

Then you'll need to open up the system settings dialog in KDE, navigate to Keyboards, then select the `maliit` under Virtual Keyboards

## TODO: Option2: DRM (Direct Rendering Manager)
If you'd prefer DRM mode - a more kiosk-type mode, there is a bit more configuration.
Using Raspbian-Lite, run through the same `dtoverlay` config process as in Option 1.  

Instead of `dtoverlay=vc4-kms-v3d,noaudio`, use 

```
dtoverlay=vc4-kms-dsi-7inch,noaudio
```
The Freenove 5" touch screen emulates the older Raspberry Pi 7" touch screen.

Then, you'll need to install a few other things to get it working.

```bash
sudo apt update
sudo apt upgrade
sudo reboot
sudo apt-get install libgbm1 libgl1-mesa-dri mesa-utils libinput10
sudo apt-get install kmscube
sudo kmscube
```

If you see a test cube (on the screen), well done, that's working then.
WateryTart (below) does not support DRM mode due to the way there is no on screen keyboard "built in" nor does it handle *physical keyboard* input - it has to be implemented by the application itself!

## TODO: Player: WateryTart

One of the volume modes in WateryTart allows it to control system volume, rather than just application volume. For that, you'll need to install pulseaudio-utils.  

```bash
sudo apt-get install pulseaudio-utils
```
