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

### HDD-SD Swap Prerequisites

- OS 9 ISO (I used [this one from archive.org](https://archive.org/details/os-9-install-cd), but [Universal Install from MacOS9Lives](http://macos9lives.com/downloads) and OSX 10.3 ISOs (I used [the ones from archive.org](https://archive.org/details/mac-os-x-10.3))
- SD-IDE 44pin adapter (there are many no name adapters on Amazon, every one of them is basically the same)
- SD card (I used a 128 GB Kingston)
- A working Mac/Linux with quemu installed (I'm using a Mac Mini Server 2011 and compiled quemu 5.2.0 from [sources](https://download.qemu.org/), but you can install it using homebrew)
- A set of torx and philips screwdrivers and a lot of patience (those iBooks are pain in the ass to disassembly)

## Reset Open Firmware Protection

In this guide, you will need to boot into Open Firmware by holding cmd + option + o + f during boot (before the chime). 

If, for some reason, you boot into Open Firmware (or cannot boot from USB), you need to reset Open Firmware protection (I don't know what it's actually called to be honest).

To do so, remove the RAM from the iBook and do a PRAM reset by holding cmd + option + p + r during boot (before the chime). Keep holding them until you hear two chimes. Then power off the iBook.

After resetting the PRAM this way, you can insert the RAM. 

**WARNING**: A standard PRAM reset **WILL NOT** remove the protection (the iBook will refuse to boot into Open Firmware after HDD swap or boot from USB). You need to change the RAM amount to perform a full reset.

## USB Install

TODO