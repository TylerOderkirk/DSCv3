
# Introduction
Follow this tutorial to install the DSC3 software stack on a Raspberry Pi Zero with an 8GB SD card.

This tutorial assumes you're using Linux on your PC.

# Prepare an SD card
1. Download the Raspian Linux installer with your BitTorrent client of choice: https://downloads.raspberrypi.org/raspbian_lite/images/raspbian_lite-2017-07-05/2017-07-05-raspbian-jessie-lite.zip.torrent (mirrored at https://drive.google.com/file/d/0BzoAun5526HhOVBMdGlQOXlLamc/view?usp=sharing) 
2. Write the Raspian Linux installer to an SD card. (SHA1 checksum: 30a171e10eb0b93b0e552837929c504d1bacd755)

`$ unzip -p /home/tz/Downloads/2017-07-05-raspbian-jessie-lite.zip | sudo dd of=/dev/sdX bs=4M conv=fsync`

` 0+23940 records in`

` 0+23940 records out`

` 1725629563 bytes (1.7 GB) copied, 130.701 s, 13.2 MB/s`

## Flush the filesystem cache 
`$ sync`

## Eject the SD card from the PC

## Unplug and re-plug the SD card

## Enable SSH for remote access:
`sudo touch /media/[..]/boot/ssh`

## Eject the SD card from the PC

# Attach a USB Ethernet network adapter to the Raspberry Pi Zero

# Supply power to the Raspberry Pi Zero

# Obtain the the Raspberry Pi Zero's IP address from your router's administration interface

# Log in to the Raspberry Pi Zero using SSH
` ssh pi@x.x.x.x` (password "raspberry")

## Change the password

## Set your timezone
`sudo raspi-config`

### Choose "Localisation"

## Set up a read-only filesystem
### Follow the guide at https://hallard.me/raspberry-pi-read-only/
#### Skip the "Setup the Internet clock sync" portion - DSC3 doesn't require Internet access
#### Don't skip the DHCP pidfile fix
#### Your /etc/fstab entries will begin with "PARTUUID=[..]"
#### After rebooting you should see...
`$ mount | grep mmc`

`/dev/mmcblk0p2 on / type ext4 (ro,noatime,data=ordered)`

`/dev/mmcblk0p1 on /boot type vfat (ro,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=ascii,shortname=mixed,errors=remount-ro)`

## Download the DSC3 application code 
`wget https://github.com/bbq-research/DSCv3/archive/master.zip`
`unzip master.zip`
### Hack a current-revision stamp
`touch rev`

## Configure Linux to start the DSC3 Python application at boot-up
### Follow the guide at http://www.raspberrypi-spy.co.uk/2015/10/how-to-autorun-a-python-script-on-boot-using-systemd/

# Configure the real-time clock etc
### Follow the guide at https://learn.adafruit.com/adding-a-real-time-clock-to-raspberry-pi/set-rtc-time

`sudo raspi-config`

#### Choose "Interfacing options"
##### Enable I2C
### Prevent Linux from using the Raspberry Pi Zero's UART
#### Choose "Peripherals"
##### "No login shell over UART"
##### "Yes enable HW"

# Configure the real-time clock
`sudo apt-get install python-smbus i2c-tools`

`sudo nano /boot/config.txt`

` dtoverlay=i2c-rtc,ds3231`

### Reboot
`sudo shutdown -h now`

### Confirm the clock has been detected
`sudo i2cdetect -y 1`

#### Confirm "UU" is shown
### [TODO]

`sudo apt-get -y remove fake-hwclock`

`sudo update-rc.d -f fake-hwclock remove`

`sudo nano /lib/udev/hwclock-set`

#### Comment out the first bash script conditional statement

## Configure the OLED display

### Download and install dependencies:
`sudo apt-get install python-serial build-essential python-dev python-pip RPi.GPIO python-imaging libffi-dev libssl-dev cryptography`

### Download and install the OLED Python library:
`wget https://github.com/rm-hull/luma.oled/archive/c344841497bfddd0593995eaa94e05cf31adc2a6.zip` (Mar 3, 2016)

`sudo python setup.py install`

#### Note we don't use this library: https://learn.adafruit.com/adafruit-128x64-oled-bonnet-for-raspberry-pi/usage

