# Install Windows

### Activate **Mass storage mode** using this command

```cmd
adb shell msc
```

## Change Samsung UFS from offline to online

> [!WARNING]  
>
> **Please pay attention to this part. If you do it wrong, it can make your Samsung UFS clean.**
>
> **Please note that in the case of A52s, the firehose cannot be found and recovery is almost impossible.**
>

### Please run the **Disk Management** program in Windows.

![img](https://raw.githubusercontent.com/cloudsweets/Port-Windows-11-Galaxy-A52s-5G/main/image/disk.png)

**Once mass storage mode is properly enabled, you will see a disk that is offline and has many partitions.**

 **Click on the disk, left-click, and click the online button.**


## Restore GPT

Please start adb shell using this command

```cmd
adb shell
```

You need gdisk to recover the GPT. Enter this command to use gdisk.

```cmd
gdisk /dev/block/sda
```

## Now you need to restore GPT using gdisk.

```
Command (? for help):
```
**If this phrase appears, enter (r)**

```
Recovery/transformation command (? for help):
```

**If you entered it correctly, this message will appear. Please enter (c) at this time.**

```
Warning! This will probably do weird things if you've converted an MBR to
GPT form and haven't yet saved the GPT! Proceed? (Y/N):
```

**Then, if this phrase appears, enter (y)**

```
Recovery/transformation command (? for help):
```

**If you've made it this far, you'll see this message again. Please enter (w) this time**

```
Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
PARTITIONS!!

Do you want to proceed? (Y/N):
```

**Then type (y) when this phrase appears.**

![img](https://raw.githubusercontent.com/cloudsweets/Port-Windows-11-Galaxy-A52s-5G/main/image/disk2.png)

**If you have made it this far, please run the Disk Management program. If all goes well, it will come online and many partitions will be displayed.**

## Assign letters to disks

#### Please enter this command in cmd

```cmd
diskpart
```

### Assign `X` to Windows volume

#### Select the Windows volume of the phone
> Use `list volume` to find it, it's the ones named "WINA52SXQ" and "ESPA52SXQ"

```diskpart
select volume <number>
```

#### Assign the letter X
```diskpart
assign letter=x
```

### Assign `Y` to esp volume

#### Select the esp volume of the phone
> Use `list volume` to find it, it's usually the last one

```diskpart
select volume <number>
```

#### Assign the letter Y

```diskpart
assign letter=y
```

### Exit diskpart:
```diskpart
exit
```

## Install

> Replace `<path/to/install.wim>` with the actual install.wim path,

> `install.wim` is located in sources folder inside your ISO
> You can get it either by mounting or extracting it

```cmd
dism /apply-image /ImageFile:<path/to/install.wim> /index:1 /ApplyDir:X:\
```

# Install Drivers

> Replace `<Kodiakdriversfolder>` with the location of the drivers folder

```cmd
driverupdater.exe -d <Kodiakdriversfolder>\definitions\Desktop\ARM64\Internal\kodiak.txt -r <Kodiakdriversfolder> -p X:
```

# Create Windows bootloader files for the EFI

```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

# Allow unsigned drivers

> If you don't do this you'll get a BSOD

```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" testsigning on

bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" nointegritychecks on

bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" recoveryenabled no

bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" bootstatuspolicy IgnoreAllFailures
```

>[!TIP]
>
>For UFS on Samsung devices, once Windows enters recovery mode, the partition label will be corrupted and the device will not work. This part is optional, but I recommend doing it.
>
> [Removing Windows recovery and disk checking](https://github.com/Project-Silicium/WoA-Guides/blob/main/Mu-Qcom/Vendors/Samsung/remove-win-recovery-disk-checking.md)
>

# Boot into Windows

### Move the `<Mu-a52sxq.img>` file to the device

```cmd
adb push <Mu-a52sxq.img> /sdcard
```

##### if you have a microSD card use this

```cmd
adb push <Mu-a52sxq.img> /sdcard1
```

### Make a backup of your existing boot image
> You need to do it just once

> Put it to the microSD card if possible

### Flash the uefi image from orangefox recovery
Navigate to the `Mu-a52sxq.img` file and flash it into boot

> [!IMPORTANT]
>
> Flash UEFI, unplug the USB, and boot. Then, when Windows OOBE appears, connect the USB. If you don't do this, the device will stop when Windows boots.
>

# Boot back into Android
> Use your backup boot image from orangefox recovery

# Finished!
> [!CAUTION]
>
> Do not force the device to shut down after Windows boots. Doing so may cause Windows to enter recovery mode the next time you boot Windows, which could damage your device. If you force quit by mistake, do not boot Windows, but boot into Orange Fox Recovery and reinstall Windows.
>
