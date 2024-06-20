# Installation

## Partitioning your device

## Notes:

> [!WARNING]  
> Do not run the same command twice unless specified.
> 
> DO NOT REBOOT YOUR PHONE! If you think you made a mistake, ask for help in the [Telegram chat]().
> 
> Do not run all commands at once, execute them in order!
>
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!


## 

#### Boot TWRP recovery through the PC with the command
```cmd
fastboot boot <twrp.img>
```
> If you already have TWRP installed, just hold the power and vol+ buttons at startup

#### Unmount all partitions
Go to TWRP settings and unmount all partitions

## Start the ADB shell
```cmd
adb shell
```
## Create Partitions

### Start parted
```sh
parted /dev/block/sda
```


### Delete the `userdata` partition
> You can make sure that 16 is the userdata partition number by running
>  `print all`
```sh
rm 16
```

### Create partitions
> If you get any warning message telling you to ignore or cancel, just type i and enter

#### For 128Gb models:

- Create the ESP partition (stores Windows bootloader data and EFI files)
```sh
mkpart esp fat32 10.8GB 11GB
```

- Create the main partition where Windows will be installed to
```sh
mkpart win ntfs 11GB 65GB
```

- Create Android's data partition
```sh
mkpart userdata ext4 65GB 123GB
```


### Make ESP partiton bootable so the EFI image can detect it
```sh
set 16 esp on
```

### Quit parted
```sh
quit
```

### Reboot to TWRP

### Start the shell again on your PC
```cmd
adb shell
```

### Format partitions
-  Format the ESP partiton as FAT32
```sh
mkfs.fat -F32 -s1 /dev/block/by-name/esp -n ESPSURYA
```

-  Format the Windows partition as NTFS
```sh
mkfs.ntfs -f /dev/block/by-name/win -L WINSURYA
```

- Format data
Go to Wipe menu and press Format Data, 
then type `yes`.

### Check if Android still starts
just restart the phone, and see if Android still works


## [Next step: Install Windows](https://github.com/SebastianZSXS/Poco-X3-NFC-WindowsARM/blob/main/guide/English/install.md)
