# BeagleBone Blue Setup

This instructions are for setting up a BeagleBone Blue for use with the
adafruit BNO055 IMU.  

## 1. Start from a fresh beaglebone image

Grab a minimal image for AM335x from here: https://forum.beagleboard.org/t/debian-11-x-bullseye-monthly-snapshots/31280

Direct download link: https://rcn-ee.com/rootfs/bb.org/testing/2023-03-10/bullseye-minimal-armhf/am335x-debian-11.6-minimal-armhf-2023-03-10-2gb.img.xz

Instructions are here: https://beagleboard.org/getting-started

You can run the OS off of the SD card, but I'd recommend flashing the eMMC with the new OS

## 2. Enable internet access

Add an entry to the `/etc/wpa_supplicant/wpa_supplicant-wlan0.conf` file

It should look something like:

```
network={
    ssid="your_network"
    psk="your_password"
}
```

If you need any more advanced network configuration here's a resource: https://wiki.archlinux.org/title/Wpa_supplicant#Configuration

Then reboot.  Once the BeagleBone restarts and connects to your network, 
a green LED should light up on the board.  

## 3. Enable the I2C port

i2c-1, which is the bus that the I2C port is connected to, is not enabled by default.
Run the `setup-i2c.sh` script and reboot to enable it

## 4. Setup python libraries

A few of the necessary python libraries that we need don't work out of the box, so I forked them.

~~[Adafruit_Blinka](https://github.com/adafruit/Adafruit_Blinka) didn't support the beaglebone
blue out of the box, so I added minimal board support that essentially just provides i2c
access.~~
My pull request got merged, you can now use the upstream version.

[Adafruit_BBIO](https://github.com/adafruit/adafruit-beaglebone-io-python) couldn't compile on 
newer beaglebone images, but the fix mentioned in issue #362 seems to work.

You can install all the necessary components using the `setup-python.sh` script.
