# How To Install an OS On iBook G3 (Clamshell) - No CD, No Firewire

This is a little documentation of a process I went through recently in order to restore a tangerine iBook G3 (with a broken CD drive). 

My main goal was to reinstall the OS, because the installed OS was installed with German language, and I wanted to use an English OS. And secondary goal - upgrade the HDD, because a 26 year old HDD is prone to fail very soon.

Eventually I installed OS 9 and OSX 10.3 on an SD card and swapped the HDD using an SD-IDE adapter.

## Prerequisites

There are two ways to reinstall the OS without the CD drive and Firewire - 1) via USB or 2) by installing the OS on another machine and swapping the disk.

I eventually used the second method, because I upgraded the HDD from 3GB to a 128GB SD card.

### USB Install Prerequisites

- OS 9 ISO (I used [this one from archive.org](https://archive.org/details/os-9-install-cd), but [Universal Install from MacOS9Lives](http://macos9lives.com/downloads) is also recommended)
- A working Mac or Linux machine with USB drive (I used Mac Mini Server 2011)
- USB drive memory (1 GB minimum)

### HDD/SD Swap Prerequisites

- OS 9 ISO (I used [this one from archive.org](https://archive.org/details/os-9-install-cd), but [Universal Install from MacOS9Lives](http://macos9lives.com/downloads) and OSX 10.3 ISOs (I used [the ones from archive.org](https://archive.org/details/mac-os-x-10.3))
- SD-IDE 44pin adapter (there are many no name adapters on Amazon, every one of them is basically the same)
- SD card (I used a 128 GB Kingston)
- A working Mac/Linux with qemu installed (I'm using a Mac Mini Server 2011 and compiled quemu 5.2.0 from [sources](https://download.qemu.org/), but you can install it using homebrew)
- A set of torx and philips screwdrivers and a lot of patience (those iBooks are pain in the ass to disassembly)

## Reset Open Firmware Protection

In this guide, you will need to boot into Open Firmware by holding `cmd + option + o + f` during boot (before the chime). 

If, for some reason, you boot into Open Firmware (or cannot boot from USB), you need to reset Open Firmware protection (I don't know what it's actually called to be honest).

To do so, remove the RAM from the iBook and do a PRAM reset by holding `cmd + option + p + r` during boot (before the chime). Keep holding them until you hear two chimes. Then power off the iBook.

After resetting the PRAM this way, you can insert the RAM. 

**WARNING**: A standard PRAM reset **WILL NOT** remove the protection (the iBook will refuse to boot into Open Firmware after HDD swap or boot from USB). You need to change the RAM amount to perform a full reset.

## USB Install

This is fairly simple. We just need to clone the ISO onto the USB memory using `dd` and then force the iBook to boot from USB. 

### Step 1: Flashing the USB

If you're using Mac, use `diskutil list` to check the list of devices and find your USB drive. **Make sure you have the correct number**, because if you make a mistake, you will erase data on your computer.

Actually, you can run `diskutil list` before and after plugging the USB memory to compare which disk number is the USB device. 

Optionally, you can compare the results of `ls /dev` (works also on Linux).

In my case, the USB memory is `/dev/disk4`.

Unmount the memory by using the following command (replace *X* with your disk number):

```sh
diskutil unmountDisk /dev/diskX
```

If you're using linux, you may use `sudo umounnt /dev/XYZ`, replace `XYZ` with the device name.

Now you're ready to clone the ISO. Run this command in the directory containing your OS 9 ISO (replace `macos9.iso` with the name of your ISO file and `X` with the USB device number).

```sh
sudo dd if=macos9.iso of=/dev/rdiskX bs=1m
```

Notice the **r** before the `disk` (`/dev/rdiskX`). This is the raw device, the flashing will be faster.

### Step 2: Boot From USB

After flashing is done, insert the USB into the iBook.

Turn it on and immediately hold `cmd + option + o + f` until you see Open Firmware prompt.

Unfortunately it's the only way to boot from USB on those devices. Some models can also boot from usb when you use the boot selection screen (hold `option` during startup), but my tangerine iBook (1999) cannot.

I found out that the command from [this macrumors forum post](https://forums.macrumors.com/threads/the-open-firmware-wiki.2225024/) works the best.

```
boot usb0/disk@1:\\:tbxi
```

On newer devices you can just boot by using `boot ud:,\\:tbxi`, but in my case I needed to use `boot usb0/disk@1:\\:tbxi`.

Usually you should be able by using this command, but if your Mac has more than one USB port, you may need to change `usb0` to a higher number. 

And depending on which image you used to flash the USB, you may need to increase the number after `@` (because the `tbxi` file may be on a different partition). 

### Step 3: Install The OS

Now just install the OS normally. Double click the disk/CD icon on the desktop and use the installer located in this folder. 

If you wish to erase data from the HDD, you may use `Drive Setup` (from `Utilities`) to initialize your HDD (which erases the data).

## Direct SD Install

TODO