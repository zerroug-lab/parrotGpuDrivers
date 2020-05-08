# parrotGpuDrivers
Part 1: AMD Drivers

To install the AMD graphics driver open a terminal and type out the following:

sudo apt install xserver-xorg-video-amdgpu

To install AMD firmware type out:

sudo apt install firmware-amd-graphics

For further reference see AMD GPU drivers
Nvidia and/or Nouveau

This tutorial is for those that have two GPUs (laptops primarily), which are an Intel integrated GPU (low-power consumption) and Nvidia GPU (high-power consumption).

The first thing you want to do is to know whether you prefer to use the Nouveau open sourced driver or the Nvidia proprietary driver. Conduct your own research to figure out which will best suit your needs.

For those that choose the Nouveau driver follow the steps in part 2. For Nvidia start at part 3.
Part 2: Nouveau

Since the Nouveau driver is already integrated to the kernel, there is only one thing to do: Install bumblebee and primus:

sudo apt update && sudo apt install bumblebee primus

To start a program using the nvidia gpu do this:

optirun _yourprogram_

And you are done with the install!
To test if the install is successful run this:

optirun glxgears

You might need to reboot to make it work.
Part 3: Nvidia-driver

For those that choose the proprietary Nvidia driver, two extra steps are needed.

The first thing to do is blacklist the Nouveau driver, in order to do that, you have to create this file:

sudo nano /etc/modprobe.d/blacklist-nouveau.conf

And write this in the file:

blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off 
alias lbm-nouveau off

When you are done hit Ctrl+X and save the file.

Reboot.

Now you can install the Nvidia driver:

sudo apt update && sudo apt install nvidia-driver

You can install Bumblebee and Primus:

sudo apt install bumblebee-nvidia primus

To run a program using the Nvidia gpu, use optirun:

optirun _yourprogram_

To test your config, you can run this:

optirun glxgears

Also in order to have no screen tearing when using Intel card, you may have to add custom configuration file by running next command:

sudo nano /usr/share/X11/xorg.conf.d/20-intel.conf

And insert next content by copying it with Ctrl+C and inserting to nano with Ctrl+Shift+V:

Section "Device"
    Identifier "Intel Graphics"
    Driver "intel"
    Option "TearFree" "true"
EndSection

Also append next nvidia configuration file:

sudo nano /etc/bumblebee/xorg.conf.nvidia

And at the end of the file insert next content:

Section "Screen"
    Identifier "Default Screen"
    Device "DiscreteNvidia"
EndSection

And the last step is installing OpenCL driver to make your hashcat and any other GUI programm work:

sudo apt install -y ocl-icd-libopencl1 nvidia-cuda-toolkit

A reboot is needed to make Bumblebee work as well as firejail adjustments.
