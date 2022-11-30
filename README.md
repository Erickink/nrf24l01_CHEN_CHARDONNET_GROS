# nrf24l01 Driver
A radio driver for the nrf24l01

Linux project for ECE Engineering School

## Source
A lot of the file that we are using are from https://github.com/TobleMiner/kernelstuff/tree/master/modules/nrf24l01 by TobleMiner

## Installation
On a raspberrypi : 
- ```git clone https://github.com/Erickink/nrf24l01_CHEN_CHARDONNET_GROS.git``` clone our files
- ```sudo dtoverlay nrf24l01.dtbo```
- ```sudo su```

OR

On a raspberrypi :
- ```sudo apt-get update -y``` and ```sudo apt-get full-upgrade -y``` then reboot with ```sudo reboot```
- ```sudo apt-get install raspberrypi-kernel-headers``` to install kernel-headers
- ```sudo apt-get install git-all``` to install git
- ```git clone https://github.com/TobleMiner/kernelstuff.git``` clone TobleMiner kernelstuff git
- ```cd kernelstuff/modules/nrf24l01``` go to nrf24l01 directory
- There is some error in the "nrf24l01_core.c" file : ```nano nrf24l01_core.c```
- ^W (Control + W) in "nano" and search "use_single"
- modify ".use_single_rw = 1," to ".use_single_read = 1, .use_single_write = 1,"
- ```make``` to compile
- ```sudo cp nrf24l01.ko /lib/modules/5.10.103-v7l+``` so we don't need to do "insmod" everytime
- ```sudo depmod```
- ```dtc -@ -O dtb -o nrf24l01.dtbo nrf24l01.dts```
- ```sudo dtoverlay nrf24l01.dtbo```
- ```sudo su```

## How to use it
On the first Pi run as root:

```
echo DEADBEEF42 > /sys/class/nrf24/nrf24l01/tx_address
echo DEADBEEF42 > /sys/class/nrf24/nrf24l01/pipe0/address
echo DEADBEEF43 > /sys/class/nrf24/nrf24l01/pipe1/address

echo 1 > /sys/class/nrf24/nrf24l01/pipe0/dynamicpayload
echo 1 > /sys/class/nrf24/nrf24l01/pipe1/dynamicpayload

echo 1 > /sys/class/nrf24/nrf24l01/pipe0/autoack
echo 1 > /sys/class/nrf24/nrf24l01/pipe1/autoack

echo 0 > /sys/class/nrf24/nrf24l01/pipe0/payloadwidth
echo 0 > /sys/class/nrf24/nrf24l01/pipe1/payloadwidth

echo 2000 > /sys/class/nrf24/nrf24l01/rf/datarate
echo 73 > /sys/class/nrf24/nrf24l01/rf/channel

echo 500 > /sys/class/nrf24/nrf24l01/retransmit/delay
echo 15 > /sys/class/nrf24/nrf24l01/retransmit/count
```

On the second PI - once again as root - run:

```
echo DEADBEEF43 > /sys/class/nrf24/nrf24l01/tx_address
echo DEADBEEF43 > /sys/class/nrf24/nrf24l01/pipe0/address
echo DEADBEEF42 > /sys/class/nrf24/nrf24l01/pipe1/address

echo 1 > /sys/class/nrf24/nrf24l01/pipe0/dynamicpayload
echo 1 > /sys/class/nrf24/nrf24l01/pipe1/dynamicpayload

echo 1 > /sys/class/nrf24/nrf24l01/pipe0/autoack
echo 1 > /sys/class/nrf24/nrf24l01/pipe1/autoack

echo 0 > /sys/class/nrf24/nrf24l01/pipe0/payloadwidth
echo 0 > /sys/class/nrf24/nrf24l01/pipe1/payloadwidth

echo 2000 > /sys/class/nrf24/nrf24l01/rf/datarate
echo 73 > /sys/class/nrf24/nrf24l01/rf/channel

echo 500 > /sys/class/nrf24/nrf24l01/retransmit/delay
echo 15 > /sys/class/nrf24/nrf24l01/retransmit/count
```
Now you should be able to transfer data between the two Pis.

Test it by running ```cat /dev/nrf24l01``` on the first Pi

and ```echo 'Mess with the best, die like the rest' > /dev/nrf24l01``` on the second Pi
