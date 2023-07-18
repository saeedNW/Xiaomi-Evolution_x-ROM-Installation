# Xiaomi-Evolution_x-ROM-Installation

#### In this guide we're going to install Evolution x custom ROM on a Xiaomi phone.

##### You can use this guide as the base guidance for other ROMs such as [pixel experience](https://get.pixelexperience.org/) or [crDroid](https://crdroid.net/) as or other phones well and make the needed changes based on that ROM or phone's installation guide.

#### At the end of this guide there is a tutorial to how to use [Magisk](https://magiskmanager.com/) to [root](https://en.wikipedia.org/wiki/Rooting_(Android)) your phone if you wanted to.

***
#### <span style="color:orange">Warning #1:</span> If something goes wrong while installing the ROM, there's a possibility of [bricking](https://en.wikipedia.org/wiki/Brick_(electronics)) your phone. <span style="color:red"> And it is not possible to fix a device if it is bricked.</span> So consider yourself warned.

#### <span style="color:orange">Warning #2:</span> Don't forget to backup your data, before doing even the simplest step in this guide

#### <span style="color:orange">Warning #3:</span> Make sure your phone has more than 50% charge before starting.

***
### Requirements:

+ A Windows machine 
	+ To be able to unlock Xiaomi phones' bootloader you need a Windows machine
+ A Unix base OS such as Linux or Mac
	+ Some of the packages and commands need a Unix base terminal to work properly. you can install and run them on windows power shell but I don't recommend it
	+ Warning: Do not use **Windows WSL** or **Virtual machines** under any circumstances. 
+ [Original ROM of your phone](https://xiaomifirmwareupdater.com/)
	+ if your on a custom ROM, it's best to do a clean install.
+ [Evolution x Custom ROM](https://evolution-x.org/download)
+ Latest version of [magisk ](https://github.com/topjohnwu/Magisk)
	+ if you plan to root your device
+ ADB and Fastboot utilities
+ [payload-dumper-go](https://github.com/ssut/payload-dumper-go) utility

***
### Unlock the bootloader (Windows machine Required) 

##### <span style="color:orange">Data Loss Warning: </span> Unlocking your phone's bootloader will lead to a <span style="color:red">Factory Reset</span> and <span style="color:red">Data Loss</span>. 

In order to unlock the bootloader of your phone, first you need to have a windows machine and then you can follow this steps.

1. Create a Mi account on [Xiaomi’s website](https://global.account.xiaomi.com/pass/register). 
	1. Note that every account is allowed to unlock one specific device every 30 days
2. Add your phone number to your account
3. Insert the SIM of the phone number you used to your device
4. Enable developer options in `Settings` > `About Phone` by repeatedly tapping `MIUI Version`.
5. Link the device to your Mi account in `Settings` > `Additional settings` > `Developer options` > `Mi Unlock status`.
	1. In some cases you need to use your SIM card data for the device to be linked to your account 
6. Download the [Mi Unlock app](https://en.miui.com/unlock/download_en.html) (Windows is required to run the app).
	1. Note that the version that will be downloaded is an outdated version of the app. and you need to download the new version within the app
7. After downloading the latest version of the app, you need to install needed drivers for your phone
	1. Connect your phone to your system with a cable
	2. run one of the driver installers based on your system structure (64/32 Bit)
	3. wait for the installation to finished
8. Run the Mi Unlock app and follow the instructions provided by the app.
	1. There is a chance that it tell you that you have to wait up to 30 days for the first time that you're trying to unlock your phone's bootloader. If it does so, please wait the quoted amount of time before continuing to the next step.
9. After device and Mi account are successfully verified, the bootloader should be unlocked.
	1. Unlocking the bootloader will cause a factory reset on your phone. Setup your phone without any passwords and don't login to any accounts
***
### ADB and Fastboot utilities installation

**MacOS:**
Note: [brew](https://brew.sh/) package manager should be installed.
```shell
brew install android-platform-tools
```

**Debian / Ubuntu:**
```shell
sudo apt install android-tools-adb android-tools-fastboot -y
```

**CentOS / Fedora / openSUSE:**
```shell
sudo dnf install android-tools
```

**Arch Linux and derivatives:**
```shell
sudo pacman -Sy android-tools
```

***
### payload-dumper-go utility installation

**MacOS:**
Note: [brew](https://brew.sh/) package manager should be installed.
```shell
brew install payload-dumper-go
```

**Arch Linux and derivatives:**
Note: an [AUR](https://aur.archlinux.org/) package manager should be installed
```shell
paru -S payload-dumper-go-bin
```

**Universal Method:**
you can use this method for every OS:
1. download the latest version of the `payload-dumper-go` from its [GitHub repository](https://github.com/ssut/payload-dumper-go) based on your OS and CPU type
	1. MacOS
		1. Intel Processors : XX_darwin_amd64.tar.gz
		2. Apple Silicon : XX_darwin_arm64.tar.gz
	2. Linux
		1. 32 bit Processors : XX_linux_386.tar.gz
		2. 64 bit Processors : XX_linux_amd64.tar.gz
		3. 64-bit ARM Processors: XX_linux_arm64.tar.gz
	3. Windows
		1. 32 bit Processors : XX_windows_386.tar.gz
		2. 64 bit Processors : XX_windows_amd64.tar.gz
2. extract the downloaded file
3. open a terminal in extracted directory
4. change the access permission of the `payload-dumper-go` file
```shell
chmod +x payload-dumper-go
```

***
### Enable your phone's USB debugging

1. Enable developer options in `Settings` > `About Phone` by repeatedly tapping `build number`.
2. Enable USB debugging `Settings` > `System` or `Additional options` > `Developer options` > `USB debugging`
3. connect your phone to the system. if it asked for access permission, allow it.

***
### Flash the original Rom (if you're on a custom ROM)

#### <span style="color:orange">Warning:</span> Remember to sign out of your google accounts and remove screen lock.

In order to flash the original ROM you first need to download your phone's original ROM (Go to [Requirements](#requirements) section). The downloaded file will be a archived file which you need to extract it and open a terminal window in the extracted directory. and run the `ls` command. The output should be like this
```text
flash_all.bat  flash_all_except_data_storage.bat  flash_all_lock.bat  flash_gen_crc_list.py  images      misc.txt
flash_all.sh   flash_all_except_data_storage.sh   flash_all_lock.sh   flash_gen_md5_list.py  md5sum.xml  
```

Run this command to check if your phone is connected correctly
```shell
adb devices
```

output example:
```shell
# List of devices attached
# <phone_id>    device
```

Now you need to reboot your phone in to the bootloader
```shell
adb reboot bootloader
```

and after your phone has been loaded into the bootloader, you can run this command to check your phone connection
```shell
fastboot devices
```

output example:
```shell
# <phone_id>     fastboot
```

Now there are two options:
1. Flash the original Rom and re-lock the bootloader.
2. Flash the original Rom without re-locking the bootloader.

**Warning:** from now on we're going to flash the phone's ROM, so make sure to not move the phone or the connection cable or etc. <span style="color:orange">Otherwise the phone might brick</span>.

In order to flash the original Rom and re-lock the bootloader, run this command.
```shell
sh flash_all_lock.sh
```

And if you plan to flash the original Rom without re-locking the bootloader, run this
```shell
sh flash_all.sh
```

After the process is finished, the phone should be rebooted into MIUI operating system.

If you plan to install a custom ROM on your phone then setup it without any passwords and don't login to any accounts. But if you wan't to keep the MIUI then feel free to setup it completely 

***
### Check phone's partition type

Before continuing you need to check your phone's partition type. Generally there are two types for android phones partitions
1. A-Only System Partition
2. A/B System Partition

In order to check your phone's partition type connect your phone to the system and run this command to check if it's connected correctly
```shell
adb devices
```

output example:
```shell
# List of devices attached
# <phone_id>    device
```

Then, open adb shell whit this command
```shell
adb shell
```

Now run this command to check your phone partition type
```shell
getprop ro.build.ab_update
```

This will return true if the device supports dual partitions and false if it's not.

***
### Flashing the  Evolution_x ROM

[Enable your phone's USB debugging](#enable-your-phones-usb-debugging)

#### <span style="color:orange">Warning:</span> Remember to sign out of your google accounts and remove screen lock.

In order to flash the Evolution_x ROM you fist need to download the ROM for your phone (Go to [Requirements](#requirements) section). The downloaded file will be a archived file which you need to extract.

After extracting the file contents, you need to extract payload data from the `payload.bin` file. In order to do this there are two options

Option #1: If you have installed the payload-dumper-go on your system you can open a terminal in the directory where you have extracted the ROM and run this command
```shell
payload-dumper-go payload.bin
```

Option #2: If you're using the universal method of the `payload-dumper-go` installation, then open a terminal in the the `payload-dumper-go`  directory and run this command
```shell
./payload-dumper-go path/to/payload.bin
```

After running this command there will be a new folder with a name such as `extracted_20230717_175100` or `META-INF` in the same directory as the `payload.bin` file. Open it in terminal.

Before going any farther you need to connect your phone to the system. Run this command to check if your phone is connected correctly
```shell
adb devices
```

output example:
```shell
# List of devices attached
# <phone_id>    device
```

Now you need to reboot your phone in to the bootloader
```
adb reboot bootloader
```

and after your phone has been loaded into the bootloader, you can run this command to check your phone is connected
```shell
fastboot devices
```

output example:
```shell
# <phone_id>     fastboot
```

If the connection is fine, you can continue with the next step. 

Now based on your phone's partition type you need to locate `boot.img` and `vendor_boot.img` files. These are the recovery files which you need to flash to your phone. Run the following commands based on your phone's partition type in order to flash them to your phone.

If your phone does support dual partitions, you need to flash both `boot.img` and `vendor_boot.img` to your phone. In order to do that run the following commands
```shell
fastboot flash vendor_boot vendor_boot.img
```

```shell
fastboot flash boot boot.img
```

If your phone doesn't support dual partitions, then you only need to flash `boot.img` to your phone. In order to do that run the following command
```shell
fastboot flash recovery boot.img
```

Now boot to the phone's recovery menu
```
fastboot reboot recovery
```

Now your phone is ready to be flashed with the Evolution_x ROM. in recovery menu go to `Apply update` > `Apply From ADB`

**Warning:** from now on we're going to flash the phone's ROM, so make sure to not move the phone or the connection cable or etc. <span style="color:orange">Otherwise the phone might brick</span>.

Run this command to check if your phone is connected correctly
```shell
adb devices
```

output example:
```shell
# List of devices attached
# <phone_id>    device
```

if the connection is fine then open a terminal in the current directory of the Evolution_x zip file that you have downloaded  at the beginning of this section and run this command to flash it to your phone. make sure to replace `evolution_x_filename.zip` with the actual file name
```shell
adb sideload evolution_x_filename.zip
```

**Note:** The ROM installation progress percentage indicator will most likely be stuck somewhere between 37-47%. Don't worry there is no problem, let the system continue to work until the process is finished. After the process is complete, a message stating that the process was successful with a status code of 0 should be printed on the phone screen in the reports section. If so, you can move on.

Now, you need to format your phone to factory settings, in order to do that in the recovery menu go to `Factory Reset` > `Format Data` or `Factory Reset` > choose `yes`

If the last step result was successful, then congratulation, you're now a proud owner of a Xiaomi phone with the Evolution_x ROM as the phone's Operating System. 

the last thing you need to do is to: in the recovery menu go to `Reboot System Now`.

If you plan to root your phone then you can go to [Rooting_phone_with_magisk](#Rooting-phone-with-magisk) section. otherwise your done.

***
### Rooting phone with magisk

In the last step we have flashed our phone with the Evolution_x ROM, and now we're going to root our device. In order to do that, you need to setup your device first and [Enable your phone's USB debugging](#enable-your-phones-usb-debugging) option.

Connect your phone to your system again and check if your phone is connected correctly
```shell
adb devices
```

output example:
```shell
# List of devices attached
# <phone_id>    device
```

Reboot your phone in to the recovery menu
```
adb reboot recovery
```

In the recovery menu go to `Apply update` > `Apply From ADB`

Check if your phone is connected correctly
```shell
adb devices
```

output example:
```shell
# List of devices attached
# <phone_id>    device
```

Now, open a terminal in the current location of the `Magisk.apk` file and run this command to start installing magisk application and root your device. Replace `magist_file_name.apk` with the actual name.
```
adb sideload magist_file_name.apk
```

After the process has ended in recovery menu go to `Reboot System Now`. and wait for the phone to boot up.

After the phone rebooted open `magisk` from apps list and follow this steps.

1. Follow the startup requests if there was any such as completing the installation and etc. 
2. Go to `Settings` > `Hide the Magisk app`.
3. Choose a name for the hidden app and click `OK`.
4. Go to `Settings` > enable `Zygisk` and `Enforce DenyList`.
5. Go to `Settings` > `Configure DenyList` .
6. Click on the three dots at the top right and select `Show system apps.`
7. Check the following apps.
	1. Google play services (necessary)
	2. Carrier Services (necessary)
	3. Google Play Games (necessary)
	4. Google Play Store (necessary)
	5. Accounting apps such as banking apps (necessary)
	6. Any other app that you don't wan't to let them know that your phone is rooted (optional)

It's just that. Done. Finished.
Enjoy.
