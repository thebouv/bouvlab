# PiHole

Of course I'm going to set up a pihole on my network, right?

And though I think I can eventually run that on [thehive](./thehive.md), I also wanted to buy and make use of some Raspberry Pi Zero W devices too since they're $10.

I followed the instructions [here](https://learn.adafruit.com/pi-hole-ad-blocker-with-pi-zero-w/install-pi-hole) and will detail some other changes I'm making below.

## Changes

**Disable Bluetooth:** I don't need it and it's more running on these thin devices. [Instructions here.](http://blog.mmone.de/2017/05/16/raspberry-pi-zero-w-disable-bluetooth/)

- Add `dtoverlay=disable-bt` to the `/boot/config.txt` file.

**Fix Locale:** I was getting a lot of locale errors, so followed [these instructions](https://www.jaredwolff.com/raspberry-pi-setting-your-locale/).

- `sudo perl -pi -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g' /etc/locale.gen`
- `sudo locale-gen en_US.UTF-8`
- `sudo update-locale en_US.UTF-8`

