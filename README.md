# Atheros

Atheros QCA6174 Wireless Firmware for Debian Buster (and probably Ubuntu). Kernel Versions 4.15 through 4.17 (Check out the appropriate Branches)

**SPECIAL THANKS**

Special thanks to @jeremyb31 ([his profile on ubuntuforums](https://ubuntuforums.org/member.php?u=1924242)) for having provided the [modified source and instructions](https://ubuntuforums.org/showthread.php?t=2384640&page=4) on ubuntuforums for getting the Atheros QCA6174 Wireless working on Ubuntu/Debian linux in kernel versions 4.15 and 4.16.

From what I understand the issue has to do with a few things, from a tweak to regulatory domain - to support for the EEPROM chipset used in devices such as the Samsung Galaxy Book 12.

The new source is simply a modified version of jeremyb31's - updated for 4.17 by replacing the `wil6210` directory with the updated one found in the [latest linux kernel source tree](https://www.kernel.org/pub/linux/kernel/v4.x/linux-4.17.tar.xz).

## Instructions

The following instructions will guide you through building and installing the Atheros QCA6174 Wireless Firmware in Debian and probably Ubuntu

### Dependencies

Ensure you have all the required tools

```bash
sudo apt-get update
sudo apt-get install git build-essential
```

### Get the Source

Clone the repository

```bash
git clone https://github.com/devrikx/atheros
```

Enter the newly checked out repository

```bash
cd atheros
```

Check out the branch for your kernel version. If you are using the latest kernel version available in Debian Buster, you can just use master. Otherwise, run the following command, replacing the xx with your minor kernel version (i.e. 15, 16, etc).

```bash
git checkout 4-XX-stable
```

The remaining process depends upon whether this is your first time building the source, or whether you're rebuilding due to a kernel patch version update (i.e. 4.17.0 to 4.17.0-3).

### Preparing to Update

If you are updating due to a kernel patch (or packaging) version update, you'll need to clean your working directory first [[1]](#note-to-ubuntu-users):

```bash
make -C /lib/modules/$(uname -r)/build M=$(pwd) clean
```

Now you are ready to continue as if it is your first run through this process

### Build

We start by copying some configuration to the working directory [[1]](#note-to-ubuntu-users):

```bash
cp /lib/modules/$(uname -r)/build/.config ./
cp /lib/modules/$(uname -r)/build/Module.symvers ./
```

Then we build the source

```bash
make -C /lib/modules/$(uname -r)/build M=$(pwd) modules
```

Great! You're ready to install.

### Install

To install the firmware, copy the compiled kernel driver to the appropriate system path [[1]](#note-to-ubuntu-users):

```bash
sudo cp ath.ko /lib/modules/$(uname -r)/kernel/drivers/net/wireless/ath
```

Finally, reboot your system:

```bash
sudo shutdown -r now
```

#### Note to Ubuntu Users

Please note that on Ubuntu, you probably need to prepend `/usr` to the paths used in the commands above.


## Conclusion

After you've rebooted your PC, you should have working wifi!

This works for me on my Samsung Galaxy Book 12, running Debian Buster with kernel version 4.17.0-3 (At the time this `README.md` was last updated.)

Feel free to let me know how it works for you - though for serious issues I'd recommend you seek help by posting to the forums where @jeremyb31 provides the [original source and solution](https://ubuntuforums.org/showthread.php?t=2384640&page=4)

## License

I claim no ownership nor authorship of anything within this repository other than this `README.md`. Any significant licenses may be found within the `preferred` directory.

The instructions and source used to get working atheros wifi on linux kernel version 4.17 is entirely based upon @jeremyb31's solution given on [ubuntuforumns](https://ubuntuforums.org/showthread.php?t=2384640&page=4), and provided on @jeremyb31's [github repository](https://github.com/jeremyb31/ath-4.15.git), respectively.

Thanks!