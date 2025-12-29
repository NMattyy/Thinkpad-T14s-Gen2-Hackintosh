# How to install MacOS on a Thinkpad T14s Gen 2 AMD  (NO EFI)

> [!WARNING]
> I am not responsible for any damages you may cause installing MacOS on your Laptop.
>
> THIS IS ONLY A GUIDE. I WILL NOT PROVIDE ANY EFI.
>
> With this repository I would like to create a semi-guide to install MacOS on the Thinkpad T14s Gen 2 (Amd version) using OpenCore and showing what works and what does not.
> 
> You have to build your own EFI by following [Dortania guide](https://dortania.github.io/) and [ChefKiss Guide](https://chefkissinc.github.io/guides/hackintosh/) as you may have a slightly different configuration than mine.


## 💻 Hardware 
- CPU: AMD Ryzen 7 PRO 5850U With Radeon Graphics                         
- GPU: Integrated Vega 8 2GB (VRAM adjusted in BIOS)                      
- RAM: 16GB 4266 MHz
- Storage: 512GB SSD NVME (WDC PC SN730 512G)
- Audio Codec: Realtek ALC257
- Wireless Card: Intel AX210 WIFI 6E + Bluetooth 5.3
- LAN: Realtek GbE 8111 Family
- Screen: IPS 1920x1080 (500 nits variant)
- PS2 Keyboard and I2C Trackpad, Trackpoint and Touchscreen
- 2x USB-C 3.2 gen 2
- 2x USB 3.2 gen 1
- HDMI 2.0
- Dock extension connector
- SmartCard Reader

## BIOS Config

| BIOS     | Section           | Setting                          | Status    | Note         |
| -------- | ----------------- | -------------------------------- | --------- |--------------|
| Config   | Network           | Wake On Lan                      | `Disabled`|              |
|          | Network           | Wake On Lan From Dock            | `Disabled`|              |
|          | Display           | UMA Frame Buffer Size            | `512MB+`  | 2GB would be preferable              |
|          | Power             | CPU Power Management             | `Enabled` |              |
|          | Power             | Sleep State                      | `Linux`   |              |
| Security | Security Chip     | Security Chip                    | `Disabled`| Only if you have problems with sleep/wake functionality            |
|          | Memory Protection | Exectuion Prevention             | `Enabled` |              |
|          | Virtualization    | AMD V (TM) Technology            | `Enabled` |              |
|          | Secure Boot       | Secure Boot                      | `Disabled`| [Can be enabled after installation complete if you sign OpenCore](https://github.com/perez987/OpenCore-and-UEFI-Secure-Boot)             |     

## MacOS Support

This section only cover general compatibility of MacOS. I have not tested every version. For compatibility status see [Status](https://github.com/NMattyy/Thinkpad-T14s-G2-MacOS/?tab=readme-ov-file#-status).                                          
`SMBIOS: MacBookPro16,3 (MacBookPro16,2 for Tahoe 26.x)`                       
`Min version: Catalina 10.15.x` `Max Version: Tahoe 26.x`                      
`Recommended: Ventura 13.x`                             

| Version              | Note                 |
| -------------------- | -------------------- |
| Catalina 10.15       | Could have some issue with NootedRed | 
| Big Sur 11           |                       | 
| Monterey 12          |                       | 
| Ventura 13           |                       | 
| Sonoma 14            | Noticebly more unstable than Ventura because of NootedRed | 
| Sequoia 15           | Noticebly more unstable than Ventura because of NootedRed | 
| Tahoe 26             | Noticebly more unstable than Ventura because of NootedRed. [No official audio support](https://github.com/NMattyy/Thinkpad-T14s-G2-MacOS?tab=readme-ov-file#audio-support-on-Tahoe) | 

The CPU could, actually, support `High Sierra 10.13` and superior but, as there's no way to get proper Graphics acceleration, as NootedRed does not support anything below `Catalina 10.15`, I counted them as `Unsupported`

## Pre-setup

You must have a usb stick formatted in `FAT32` where you put OpenCore and the MacOS recovery.           
You must use your Target Laptop to set up your SSDT and USB and you should have a second PC for troubleshooting.
You should update your BIOS before trying to install MacOS.        
Download [OpenCore debug](https://github.com/acidanthera/opencorepkg/releases) (This will provide us more logs to help us troubleshooting in case of problems.)     
Download [ProperTree](https://github.com/corpnewt/ProperTree/archive/refs/heads/master.zip) (a .plist editor)       
Download [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS/archive/refs/heads/master.zip) (to generate your SMBIOS)      
In Windows Download [SSDTTime](https://github.com/corpnewt/SSDTTime/archive/refs/heads/master.zip) (to generate your SSDT)        
In Windows Download [USBToolBox](https://github.com/USBToolBox/tool/releases/tag/0.2) (to map your USB)

## Setup

You can either follow [Dortania](https://dortania.github.io/) and [ChefKiss](https://chefkissinc.github.io/guides/hackintosh/) guides (Highly recommended)
or you can follow these steps (Not recommended because this only applies to my precisely laptop configuration that could not be the same as yours, resulting in errors.)                                   
You should only rely on this guide for the Troubleshooting and Post-Install sections.

#### Initial EFI setup         
Download MacOS recovery using [Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/windows-install.html)      
You can install every MacOS version from Catalina to Sequoia (Recommended Ventura as it is the most stable so far)      
From the `OpenCore.zip` you've just downloaded, take the `EFI` folder from the `X64` folder and put it on your USB stick.        
Into the `EFI` you have 2 folder, open the `OC` one then you will have another 5 folders, from the `Drivers` one you have to delete everthing except for `OpenRuntime.efi` and you have to download [HFSPlus.efi](https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/HfsPlus.efi).      
Then, you have to go on the `Tools` folder. Here you have to delete everything except for `UEFIShell.efi` (You can also keep `CleanNVRAM.efi` to reset the NVRAM but note that it is known to brick some thinkpads making them unbootable so i would prefer not to). 
I DO NOT TAKE ANY RESPONSABILITY IF YOU BREAK YOUR LAPTOP.     

#### Kext folder setup        
You have to download the following kextd and put them into your `Kexts` folder :
| Kext        |  Note             |
| --------    | ----------------- |
| [Lilu](https://github.com/acidanthera/Lilu/releases)   |                   |
| [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases)  | From the zip folder that you get when you download this kext you have to also pick `SMCBattery`                |
| [SMCProcessorAMD](https://github.com/macos86/SMCProcessorAMD/releases)            | Optional. Gives SMC reading for the CPU |
| [SMCRadeonSensors](https://github.com/ChefKissInc/SMCRadeonSensors/releases)            | Optional. Gives SMC reading for the GPU |
| [NootedRed](https://nightly.link/ChefKissInc/NootedRed/workflows/main/master/Artifacts.zip)            | Could cause some problem on Sonoma+ so, during the installation process and the out of the box experience, disable It, change the background to a static one and change your account avatar to a normal photo, then you can re-enable It.    | 
| [AppleALC](https://github.com/acidanthera/AppleALC/releases)            | You can use `alcid=11` as codec in your boot-args       |
|  [RTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)           | Use version `2.4.2` as version `2.5.0` is not meant for AMD CPUs |
| [Itlwm or AirportItlwm](https://openintelwireless.github.io/itlwm/)    | If you're installing Ventura or lower you can use AirportItlwm as it enables native wifi but, if you're installing Sonoma+, itlwm + heliport is reccomended (Read the guide from the link to understand better)       |
| [OpenIntelBluetooth](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases) | On macOS 12+ you need to use IntelBluetoothFirmware and IntelBTPatcher from the zip that you just downloaded, and BlueToolFixup from [BrcmPatchRAM](https://github.com/acidanthera/BrcmPatchRAM/releases). |
|[VoodooPS2](https://github.com/acidanthera/VoodooPS2/releases) ||
| [VoodooI2C](https://github.com/VoodooI2C/VoodooI2C/releases) | This enables just trackpad but, If you want, you can also add VoodooI2CHID to also enables touchscreen. |
| [NVMeFix](https://github.com/acidanthera/NVMeFix/releases) | |
| [AppleMCEReporterDisabler](https://chefkissinc.github.io/Extras/Kexts/AppleMCEReporterDisabler.zip) | |
| [ForgedInvariant](https://github.com/ChefKissInc/ForgedInvariant/releases) | |
| [ECEnabler](https://github.com/1Revenger1/ECEnabler/releases) | |
| [BrightnessKeys](https://github.com/acidanthera/BrightnessKeys) | |
| [iBridged](https://github.com/Carnations-Botanica/iBridged/releases) | |

#### Map your USB     
In Windows, download USBToolBox, extract It and open `windows.exe`. Go in the settings and enable `Native Classes` then,
select `Discover Ports` and plug a USB 3 device and a USB 2 device in each port. Once you're done with mapping, go to
`Select Ports`, adjust anything that is not set correct and then press `K` to build the kext then, put it into your `kext` folder.

#### Config.plist setup
From the `OpenCorePKG` folder, open the `docs` folder then copy the `sample.plist` file and put it on your usb stick into the `OC` folder, then rename it into `config.plist`.
Now, follow [Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/AMD/zen.html) to setup your `config.plist`.

#### SSDT creation     
In Windows, download SSDTTime, extract It and open `SSDTTime.bat`.        
Dump your ACPI table pressing `P`.   
Then, you have to choose this options.   
`USBX` (Choose B).   
`RTCAWAC` (If It says that you do not need It, skip It.).  
`PluginType`   
`USB Reset`   
`FakeEC Laptop`  
`XOSI` (Choose A)  
`PNLF`   
`ALS0`     
Now, go into the `Results` folder and take every `.aml` file and put it into your `OC/ACPI` folder. Now, from the `SSDTTime` folder use the `PatchMerge.bat` file to merge your `config.plist` with the patch that SSDTTime created for you then, go to ProperTree again and perform an `OC Snapshot`.

## Troubleshooting

You will likely get stuck at `[EB|#LOG:EXITBS:START]` so you have to follow [this guide](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/kernel-issues.html#booter-issues). 
For my experience, you just have to set `EnableWriteUnprotector -> True`, `RebuildAppleMemoryMap -> False` and `SyncRuntimePermissions -> False`, then MacOS Recovery will boot (If you need to dualboot windows using opencore you have to set `SyncRuntimePermissions -> True`).

If you're using `Itlwm` instead of `AirportItlwm`, in Recovery, you won't initially have access to the Wifi panel, as you can't use Heliport, so you have to edit the `info.plist` that you find inside `itlwm.kext`to manually add your Wifi info into the configuration to allow `Itlwm` to connect to it without Heliport. If you can't get it to work and you can't use your ethernet, if you have an android device you can use as usb tethering, you can install [HoRNDIS.kext](https://drive.google.com/file/d/1oBKn5JwKisGOaADY5dE85-coVNqXrkFx/view).

## Post-Install

#### Bluetooth issues      
For Monterey and older, if you followed the guide, bluetooth should work Out Of The Box, but if you installed Ventura+ you have to do some trouble shooting.      
Go into your `NVRAM>7C436110-AB2A-4BBB-A880-FE41995C9F82` section on your `.plist` file and add this under the corrispondent `Add` and `Delete` sections:      

| Key        |  Type              |  Value            |
| --------    | ----------------- | ----------------- |
| bluetoothInternalControllerInfo | Data | 00000000 00000000 00000000 0000 |
| bluetoothExternalDongleFailed   | Data | 00 |

For Sonoma and older, if adding the NVRAM values does not work, you can try adding `-btlfxnvramcheck` in your boot-args.                                                                 
For Sequoia and newer, if adding the NVRAM values does not work, you can try adding `-btlfxallowanyaddr` in your boot-args.

Now your bluetooth should work properly.          

#### Crashing apps issue   
Install [AMDHelper](https://github.com/alvindimas05/AMDHelper) and enable the patches that you need for the apps that don't work/chromium based apps that cause graphical issues with NootedRed.

#### Poor battery life and heating
That's due to the PowerManagement being non-existent. Install [AMDHelper](https://github.com/alvindimas05/AMDHelper) and enable battery optimisation (Performance will be drastically reduced).

#### YogaSMC features
If you want to to have fan and sensors reading, fan control and other thinkpad's features that you have on windows like battery treshold or full function keys functionality, you can install [YogaSMC](https://github.com/zhen-zen/YogaSMC)

Requires some addittional SSDT that you have to compile by yourself using your `DSDT.aml` and the example SSDTs that are available on their site.

#### Audio support on Tahoe
As Apple removed AppleHDA since Tahoe Beta 2, there is no official audio support, but, with the help of some tools, we can add It back using [this guide](https://github.com/perez987/AppleHDA-back-on-macOS-26-Tahoe) instead of OCLP.

## 🔧 Status

> [!NOTE]
>
>I firstly installed ventura and then I upgraded to Sequoia and I did not test any other version but everything in this status should apply to every MacOS version installable on this Thinkpad. Let me know if It is not so.
> 
> - Working = Works out of the box or with some troubleshooting 
> - Partially Working = Working but with some occasional problems
> - Not Working = Does not work and probably never will
> - Not Tested = Not tested but would probably works
>
> This applies only to stable releases.


### ✔️ Working
- GPU acceleration and backlight control
- Audio + Jack + HDMI Audio
- Keyboard, Trackpoint and Trackpad's buttons
- Wifi (Trough AirportItlwm in Ventura and older, trough Itlwm + Heliport in Sonoma and newer)
- Bluetooth (See [Bluetooth issue](https://github.com/NMattyy/Thinkpad-T14s-G2-MacOS?tab=readme-ov-file#bluetooth-issues))
- HDMI
- SMC Sensor reading
- Clamshell mode
- Volume and Brightness function keys
- USB

### ⚠️ Partially Working
- Modern standby (I couldn't get MacOS to go into sleep mode while using `Modern Standby or Windows Standby` so I had to switch back into `S3 Standby or Linux Standby` in the bios. Other than that, standby works just fine).
- Trackpad and Touchscreen (They actually work pretty well but they work inconsistently as, occasionally, they just don't work and you have to restart the entire system to get them to work. Right now I don't really know what's causing that issue).

### ❌ Not Working
- Internal microphone (Could work if you switch from AppleALC to [VoodooHDA](https://github.com/CloverHackyColor/VoodooHDA). Note that the quality of the audio of the speakers will be lower if you switch to VoodooHDA).
- Internal camera
- PowerManagement (See [Poor battery life and heating](https://github.com/NMattyy/Thinkpad-T14s-G2-MacOS?tab=readme-ov-file#poor-battery-life-and-heating))
- Battery threshold (It could probably work. I think that, probably, my YogaSMC's SSDTs are just bad).

### ❓ Not Tested
- USB-C display port
- LAN with Dock extension connector
- SmartCard reader

## ℹ️ Credits
- [@acidanthera](https://github.com/acidanthera) for [OpenCore](https://github.com/acidanthera/OpenCorePkg) and many kexts
- [@corpnewt](https://github.com/corpnewt) for [ProperTree](https://github.com/corpnewt/ProperTree), [SSDTime](https://github.com/corpnewt/SSDTTime) and [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
- [@dortania](https://github.com/dortania) for their amazing [guide](https://dortania.github.io)
- [@alvindimas05](https://github.com/alvindimas05) for [AMDHelper](https://github.com/alvindimas05/AMDHelper)
- [@USBToolBox](https://github.com/USBToolBox) for [USBToolBox](https://github.com/USBToolBox/tool)
- [@ChefKissInc](https://github.com/ChefKissInc) for their also amazing [guide](https://chefkissinc.github.io/), [NootedRed](https://github.com/ChefKissInc/NootedRed), [SMCRadeonSensor](https://github.com/ChefKissInc/SMCRadeonSensors) and [ForgedInvariant](https://github.com/ChefKissInc/ForgedInvariant)
- [@1Revenger1](https://github.com/1Revenger1/) for [ECEnabler](https://github.com/1Revenger1/ECEnabler/)
- [@zxystd](https://github.com/zxystd) for [Intel Wireless Card kexts](https://github.com/OpenIntelWireless/)
- [@Mieze](https://github.com/Mieze) for [RTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X)
- [@zhen-zen](https://github.com/zhen-zen) for [YogaSMC](https://github.com/zhen-zen/YogaSMC)
- [@jwise](https://github.com/jwise) for [HoRNDIS](https://github.com/jwise/HoRNDIS)
- [@perez987](https://github.com/perez987) for their [Secure Boot guide](https://github.com/perez987/OpenCore-and-UEFI-Secure-Boot)
- [@CloverHackyColor](https://github.com/CloverHackyColor/) for [VoodooHDA](https://github.com/CloverHackyColor/VoodooHDA)
- [@macos86](https://github.com/macos86/) for [SMCProcessorAMD](https://github.com/macos86/SMCProcessorAMD)
- [@perez987](https://github.com/perez987/) for [AppleHDA back on Tahoe](https://github.com/perez987/AppleHDA-back-on-macOS-26-Tahoe)
- [@Carnations-Botanica](https://github.com/Carnations-Botanica) for [iBridged](https://github.com/Carnations-Botanica/iBridged/)
