<img align="right" src="https://github.com/n00b69/woa-everything/blob/main/devices.png" width="350" alt="Windows 11 running on a cellphone">

# Running Windows on your cellphone

## Partitioning your device

### Prerequisites
- A brain (most important of all)

- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [Parted](https://github.com/n00b69/woa-everything/releases/download/Files/parted)

- TWRP or any other recovery

### Notes
> [!WARNING]  
> 
> DO NOT REBOOT YOUR PHONE! If you think you made a mistake, ask for help in the [Telegram chat](https://t.me/joinchat/MNjTmBqHIokjweeN0SpoyA).
> 
> Do not run all commands at once, execute them in order!
>
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!

### Opening CMD as an admin
> Download **platform-tools** and extract the folder somewhere, then open CMD as an **administrator**.
>
> It is recommended to keep this window open and use it throughout the entire guide.
> 
> Replace `path\to\platform-tools` with the actual path to the platform-tools folder, for example **C:\platform-tools**.
```cmd
cd path\to\platform-tools
```

#### Boot TWRP recovery
> Open a CMD window inside the platform-tools folder, then (while your phone is in fastboot mode) run
```cmd
fastboot boot path\to\twrp.img
```

#### Backing up important files
> This will back up **fsc**, **fsg**, **modemst1** and **modemst2** to the current path your CMD is opened in (for example **C:\platform-tools**). Confirm these files are actually there before proceeding.
>
> If you've got anything else you want to back up, do this now. Your Android data will be erased in the next steps.
```cmd
adb shell "dd if=/dev/block/by-name/fsc of=/tmp/fsc" || true && adb pull /tmp/fsc || true && adb shell "dd if=/dev/block/by-name/fsg of=/tmp/fsg" || true && adb pull /tmp/fsg || true && adb shell "dd if=/dev/block/by-name/modemst1 of=/tmp/modemst1" || true && adb pull /tmp/modemst1 || true && adb shell "dd if=/dev/block/by-name/modemst2 of=/tmp/modemst2" || true && adb pull /tmp/modemst2
```

### Partitioning guide
> Your device may have different storage sizes. This guide uses the values of the 128GB Pocophone F1 as an example. When relevant, the guide will mention if other values can or should be used.

#### Unmount data
- Go to "Mount" in TWRP and unmount data, if it is mounted

#### Preparing for partitioning
> Download the parted file and move it in the platform-tools folder, then run
```cmd
adb push parted /cache/ && adb shell "chmod 755 /cache/parted" && adb shell /cache/parted /dev/block/sda
```

#### Printing the current partition table
> Parted will print the list of partitions, userdata should be the last partition in the list.
```cmd
print
```

#### Removing userdata
> Replace **$** with the number of the **userdata** partition
```cmd
rm $
```

#### Recreating userdata
> Replace **1611MB** with the former start value of **userdata** which we just deleted
>
> Replace **32GB** with the end value you want **userdata** to have
```cmd
mkpart userdata ext4 1611MB 32GB
```

#### Creating ESP partition
> Replace **32GB** with the end value of **userdata**
>
> Replace **32.3GB** with the value you used before, adding **0.3GB** to it
```cmd
mkpart esp fat32 32GB 32.3GB
```

#### Creating Windows partition
> Replace **32.3GB** with the end value of **esp**
>
> Replace **123GB** with the end value of your disk, use `p free` to find it
```cmd
mkpart win ntfs 32.3GB 123GB
```

#### Making ESP bootable
> Use `print` to see all partitions. Replace "$" with your ESP partition number
```cmd
set $ esp on
```

#### Exit parted
```cmd
quit
```

#### Formatting data
> Format all data in TWRP, or Android will not boot.
- ( Go to Wipe > Format data > type yes )

#### Check if Android still starts
- Just restart the phone, and see if Android still works


## [Next step: Installing Windows](/guide/2-install.md)





















