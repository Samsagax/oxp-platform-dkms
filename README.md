# Platform driver for OneXPlayer boards

This driver provides functinoality to control the fan in the OneXPlayer mini
AMD variant. Intel boards are not yet supported until I can figure out EC
registers and values.

## Build
If you only want to build and test the module (you need headers for your
kernel):

```
$ git clone https://gitlab.com/Samsagax/oxp-platform-dkms.git
$ cd oxp-platform-dkms
$ make
```

Then insert the module and check `sensors` and `dmesg` if appropriate.

## Install

You'll need appropriate headers for your kernel and `dkms` package from your
distribution.

```
$ git clone https://gitlab.com/Samsagax/oxp-platform-dkms.git
$ cd oxp-platform-dkms
$ make
$ sudo make dkms
```

## Usage

Insert the module with `insmod`. Then look for a `hwmon` device with name
`oxpec`, i.e.:

`$ cat /sys/class/hwmon/hwmon?/name`

To enable fan control set the parameter `fan_control=1`.

### Reading fan RPM

`sensors` will show the fan RPM as read from the EC. You can also read the
file `fan1_input` to get the fan RPM.

### Controlling the fan

***Warning: controlling the fan without an accurate reading of the CPU, GPU,
and Battery temperature can cause irreversible damage to the device. Use at
your own risk!***

To enable manual control of the fan (assuming `hwmon5` is our driver):

`# echo 1 > /sys/class/hwmon/hwmon5/pwm1_enable`

Then input values in the range `[0-255]` to the pwm:

`# echo 100 > /sys/class/hwmon/hwmon5/pwm1`

