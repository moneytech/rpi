# gles2_bcm

Example how to do GLES2 rendering with Broadcom proprietary OpenGL driver.

This driver is available in Raspbian and 32-bit based Arch Linux ARM (armv6 & armv7) and
Alpine Linux distributions.


# Preparations

First install necessary development files in sysroot.

For Raspbian do:

    ../sysroot.py --distro raspbian --sysroot "${SYSROOT}" libraspberrypi-dev

For Ubuntu do:

    ../sysroot.py --distro ubuntu --sysroot "${SYSROOT}" libraspberrypi-dev

For Arch Linux ARM:

    ../sysroot.py --distro alarm --target "${TARGET}" --sysroot "${SYSROOT}" raspberrypi-firmware

For Alpine Linux:

    ../sysroot.py --distro alpine --target "${TARGET}" --sysroot "${SYSROOT}" raspberrypi-dev

Make sure your Raspberry Pi has not loaded open-source vc4 OpenGL driver. On Raspbian or
Arch Linux ARM check `/boot/config.txt` file, on Ubuntu check `/boot/firmware/config.txt`, it
must not have `dtoverlay=vc4-fkms-v3d` or `dtoverlay=vc4-kms-v3d` setting enabled.

On Ubuntu you'll need to install proprietary OpenGL user-space libraries from extra repository:

    sudo add-apt-repository ppa:ubuntu-raspi2/ppa
    sudo apt-get update
    sudo apt install libraspberrypi0

On Alpine Linux install proprietary OpenGL user-space libraries:

    sudo apk add raspberrypi-libs

On Alpine Linux you'll need to add `/opt/vc/lib` folder to dynamic linker path when running
binary. Do that with `LD_LIBRARY_PATH=/opt/vc/lib` environment variable before running binary,
or add it permanently to library path by creating `/etc/ld-musl-armhf.path` file with following
contents:

    /opt/vc/lib:/lib:/usr/local/lib:/usr/lib


# Building & running

Execute `make` to build.

You can execute `make run` to build & upload & run executable on Raspberry Pi. By default it will
try to connect to pi@raspberrypi.local host. You can override it by setting `PI` variable:
`make run PI=alarm@alarmpi.local`

You should see colorful triangle spinning. By default vsync is enabled, but you can disable it by changing
`ENABLE_VSYNC` variable in source code.

Error handling is done using `assert`, feel free to change it when porting to your code.
