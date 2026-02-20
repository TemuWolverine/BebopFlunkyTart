

# Bebop Flunky Tart
(synonyms for "Music Assistant Pie")

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Build Guide](#build-guide)
- [Parts list](#parts-list)
  - [Mechanical](#mechanical)
  - [Electrical](#electrical)
  - [Pi Specific](#pi-specific)
  - [Printing](#printing)
- [Model](#model)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
## [Build Guide](buildguide.md)

## Parts list
These are links to the *exact* products I used and have modeled in the case design. You can more than likely get it cheaper from AliExpress - try and match the diameters of things like the binding posts, DC panel jack and toggle switch or you will need to modify the models yourself.

### Mechanical  
* [M2.5 Hex standoffs](https://www.amazon.com.au/dp/B07PHBTTGV)
* 11x [M5 x 7.87mm heatset inserts](https://www.ebay.com.au/itm/176706086138)
* 11x M5 x 12mm machine screws

### Electrical  
* [4mm Binding posts ](https://www.amazon.com.au/dp/B0G87LDQ7T)
* [10cm Cat6 "up down" male-female adapter ](https://www.amazon.com.au/dp/B0D22DTX3T)
* [5.5mm x 2.5mm DC panel jack](https://www.amazon.com.au/dp/B0D9JGXBJB) 
* [40pin Female Pin Header riser](https://www.amazon.com.au/dp/B0F6V6PCVP)
* [Toggle Switch (On-Off-On)](https://www.amazon.com.au/dp/B0CNSRK9PJ)
* [KY-040 Rotary Encoder](https://www.amazon.com.au/dp/B0BRV91J1Z)
* 12-24v DC power supply
* Speaker wire (alternatively you can use the same type of wire used for power)
* Short wires for power, I used 15mm^2, it was a little thick

Make sure the DC panel jack you use is rated for whatever voltage you are operating at. These ones are 30v5a DC, more than enough. 

The power supply just needs to match the desired output from the Hifiberry Amp4 Pro with the speakers you'll be using. See the [datasheet for the amp](https://www.hifiberry.com/docs/data-sheets/datasheet-amp4-pro/).

* A 12v power supply gives a typical output of 14w per channel @ 4Ω - you'd need at least a 2.3a for the speakers, plus another 1.25a for a Pi5 for a total of 12v 3.6a minimum to achieve that (realistically 4a)

* A 24v power supply gives a typical output of 28w per channel at 8Ω. ((28w *2)+15w) / 24v = 24v 2.95a minimum.


### Pi Specific  
* Raspberry Pi 5
* [GeeekPi Armor Lite V5](https://www.amazon.com.au/dp/B0CNVFCWQR)
* [Freenove 5 Inch Touchscreen Monitor for Raspberry Pi](https://www.amazon.com.au/dp/B0B455LDKH)
* [Hifiberry Amp4 Pro](https://www.hifiberry.com/shop/boards/amp4pro/)

You do *not* have to use a Raspberry Pi 5, or a Amp4Pro. However, if you're using a Raspberry Pi 5, the Amp2/Amp4 may not provide sufficient power to the Pi 5. The Amp4 will power the Pi4, but only the Amp4Pro mentions the Pi5.

I had already bought the Pi5, so I matched the amp to it.


### Printing  
Any PLA or PETG, the amp doesn't get that hot


## Model
![alt text](images/Fusion360_NOl8xiB5L1.png)
![alt text](images/Fusion360_BuTtbfHx2u.png)