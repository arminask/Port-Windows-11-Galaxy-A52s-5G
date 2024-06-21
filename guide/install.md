# Running Windows on the POCO X3 Nfc

## Installation

### Activate `Mass storage mode` using this command

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

### Please run the `Disk Management` program in Windows.

![img]()

Once mass storage mode is properly enabled, you will see a disk that is offline and has many partitions.

## Assign letters to disks


#### Start the Windows disk manager

> Once the X3 Nfc is detected as a disk

```cmd
diskpart
```


### Assign `X` to Windows volume

#### Select the Windows volume of the phone
> Use `list volume` to find it, it's the ones named "WINSURYA" and "ESPSURYA"

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

> Replace `<suryadriversfolder>` with the location of the drivers folder

```cmd
driverupdater.exe -d <suryariversfolder>\definitions\Desktop\ARM64\Internal\surya.txt -r <suryadriversfolder> -p X:
```

  

# Create Windows bootloader files for the EFI

```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

  
  

# Allow unsigned drivers

> If you don't do this you'll get a BSOD

```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set {default} testsigning on
```

# Boot into Windows

### Move the `<uefi.img>` file to the device

```cmd
adb push <uefi.img> /sdcard
```

##### if you have a microSD card use this

```cmd
adb push <uefi.img> /external_sd
```


### Make a backup of your existing boot image
> You need to do it just once

> Put it to the microSD card if possible


### Flash the uefi image from TWRP
Navigate to the `uefi.img` file and flash it into boot

# Boot back into Android
> Use your backup boot image from TWRP

# Finished!
