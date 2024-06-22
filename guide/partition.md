# Partitioning your device

## Notes:

> [!WARNING]  
> Do not run the same command twice unless specified.
> 
> DO NOT REBOOT YOUR PHONE! If you think you made a mistake, ask for help in the [Telegram chat](https://t.me/+zVyKolc2Zec5N2Vl).
>
> If there is anything you do not understand or have difficulty with, please ask through [Telegram chat](https://t.me/+zVyKolc2Zec5N2Vl).
> 
> Do not run all commands at once, execute them in order! 
>
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!
> 

## Prerequisites

- Have the bootloader unlocked

- [orangefox recovery](https://github.com/cloudsweets/Port-Windows-11-Galaxy-A52s-5G/releases/tag/file)

- [Platform Tools](https://developer.android.com/tools/releases/platform-tools)

- [uefi](https://github.com/cloudsweets/Port-Windows-11-Galaxy-A52s-5G/releases/tag/file)

- [driver](https://github.com/cloudsweets/Kodiak-Drivers/archive/refs/heads/main.zip)

- [vbmeta.tar](https://github.com/cloudsweets/Port-Windows-11-Galaxy-A52s-5G/releases/tag/file)

- [Odin3](https://gitlab.com/Ryzen5950XT/odin_dl/-/raw/main/Odin3_v3.14.4.zip?inline=false)

- [Windows ISO](https://uupdump.net/selectlang.php?id=9be0c3f7-8590-4a1c-b793-aaa0021e412a)
  #Windows version must be 26090

#### You need to boot into orangefox recovery
Your device needs to boot into download mode
>
>1. With the device powered off, hold **Volume Up** and **Volume Down** and connect the USB cable to your PC.
>
>2. Click **AP**, select the **orangefox** **recovery** file, and click the **Start** button to install.
>
>3.  Then, it will automatically reboot into download mode again. There will be an error message, but ignore it. Click **AP** in Odin3, select **vbmeta.tar**, and click the **Start** button.
>
>4. As soon as the device turns off, press the **volume up** button and **power** button simultaneously to boot into **orangefox recovery**.

#### Unmount data partitions
- Click the menu button in orangefox recovery. Click the **Mount button**. Please **uncheck** the **data button**

## Start the ADB shell
```sh
adb shell
```
## Create Partitions

### Start parted
```sh
parted /dev/block/sda
```

### Delete the `userdata` partition
> You can make sure that 34 is the userdata partition number by running
>  `p`
```sh
rm 34
```

### Create partitions

#### For 128Gb models:

- Create the ESP partition (571MB)
```sh
mkpart esp fat32 13.2GB 13.8GB
```

- Create the main partition where Windows will be installed to (60GB)
```sh
mkpart win ntfs 13.8GB 73.8GB
```

- Create Android's data partition (53.6GB)
```sh
mkpart userdata ext4 73.8GB 127GB
```

### Make ESP partiton bootable so the EFI image can detect it
```sh
set 34 esp on
```

### Quit parted
```sh
quit
```

### Reboot to orangefox recovery

### Start the shell again on your PC
```cmd
adb shell
```

### Format partitions
-  Format the ESP partiton as FAT32
```sh
mkfs.fat -F32 -s1 /dev/block/bootdevice/by-name/esp -n ESPA52SXQ
```

-  Format the Windows partition as NTFS
```sh
mkfs.ntfs -f /dev/block/bootdevice/by-name/win -L WINA52SXQ
```

- Format data
Go to Wipe menu and press Format Data, 
then type `yes`.

### Check if Android still starts
just restart the phone, and see if Android still works

## [Next step: Install Windows](https://github.com/cloudsweets/Port-Windows-11-Galaxy-A52s-5G/blob/main/guide/install.md)
