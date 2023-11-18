# Ryzentosh 7 5700U
My documentation on putting MacOS on a Lenovo IdeaPad Flex 5-14ALC05 with Ryzen 7 5700U. There is some jumping around between different guides that I used to get MacOS working on my laptop. First we will start with Dortania's OpenCore guide.

# [Dortania Opencore Guide](https://dortania.github.io/OpenCore-Install-Guide/prerequisites.html)
Following the dortania opencore guide I downloaded Monterey using the [Windows](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/windows-install.html) method then continued on to [adding the base opencore files](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/opencore-efi.html)
to finish setting up my USB. After that, I moved on to gathering the files and dowloaded the [required firmware drivers.](https://dortania.github.io/OpenCore-Install-Guide/ktext.html#must-haves](https://dortania.github.io/OpenCore-Install-Guide/ktext.html#universal)) This next step is where I jumped over to the NootedRed Guide.
# [NootedRed Guide](https://chefkissinc.github.io/guide)
In the NootedRed Guide I started with gathering the needed [kexts](https://chefkissinc.github.io/guide/gathering-files/kexts) for my machine. Most are explained in the guide and you should pick the ones required for your specific machine. In my case I used: 

* [Lilu](https://github.com/acidanthera/Lilu)

* [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases)

* SMCBattery Manager (Bundled with VirtualSMC download)

* SMCLightSensor (Bundled with VirtualSMC download)

* [SMCAMDProcessor](https://github.com/trulyspinach/SMCAMDProcessor/releases)

* AMDRyzenCPUPowerManagement (Bundled with SMCAMDProcessor download)

* [NootedRed](https://github.com/ChefKissInc/NootedRed/actions) (You have to be signed into your github account and then click on any of the workflows and at the very bottom click on "Assets" to download the kext)

* [RealtekRTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases) (for USB Ethernet though its probably not needed)

* [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM/releases)

* [AirportItlwm](https://github.com/OpenIntelWireless/itlwm/releases)

* [IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)

* [USBToolBox](https://github.com/USBToolBox/kext/releases/tag/1.1.1) (Follow the [Windows](https://github.com/USBToolBox/tool#readme) intructions to map USB ports)

* [VoodooPS2](https://github.com/acidanthera/VoodooPS2/releases)

* [VoodooI2C](https://chefkissinc.github.io/Extras/Kexts/VoodooI2C.zip) (**You have to use this secific version of the kext as it has support for AMD controllers**)

* VoodooI2CHID (Bundled with VoodooI2C download)

* [NVMEFix](https://github.com/acidanthera/NVMeFix/releases)

* [AppleMCEReporterDisabler](https://chefkissinc.github.io/Extras/Kexts/AppleMCEReporterDisabler.zip)

* [RestrictEvents](https://github.com/acidanthera/RestrictEvents/releases)

* [ECEnabler](https://github.com/1Revenger1/ECEnabler/releases)

* [BrightnessKeys](https://github.com/acidanthera/BrightnessKeys/releases)

* [AMDTscSync](https://github.com/naveenkrdy/AmdTscSync/releases)
  


[**The ACPI files**](https://chefkissinc.github.io/guide/gathering-files/acpi) section details the steps to use SSDTTime to dump the DSDT file and then create SSDT files based off the dumped DSDT. I read all the way through that page as there are different files needed for desktops and laptops. Towards the bottom of the page it details how to patch the PLIST using the SSDTTime PLIST patcher script, but instead of that I just copy and pasted the patches from the patches_OC.plist to my config.plist in the ACPI->Patches section. You can use the script if you want, I just didnt feel like it.

# Return to Dortania
After gathering the ACPI files using the NootedRed guide, I went back to the opencore guide in the [AMD](https://dortania.github.io/OpenCore-Install-Guide/AMD/zen.html#ryzen-and-threadripper-17h-and-19h) section to cofigure the config.plist. If you want you can start [here](https://dortania.github.io/OpenCore-Install-Guide/AMD/zen.html#booter) instead because at this point you should already have the ACPI patches in place.

Follow the plist setup page all the way through and after finishing (including disabling/enabling settings in the bios) boot into MacOS using the flashdrive you setup and if you dont have graphics acceleration move on to the next section.

**Working SMBIOS: MacBookPro16,3, iMac20,1 or iMacPro1,1**

# [Smokeless_UMAF](https://github.com/DavidS95/Smokeless_UMAF#readme)
Smokeless_UMAF is a tool that lets you go deeper into the bios. In my case, the bios was locked down pretty tight and I did not have the option to allocate more VRAM to the iGPU. To do this I downloaded the [tool](https://github.com/DavidS95/Smokeless_UMAF/archive/refs/heads/main.zip), extracted the main folder, opened it, then exctracted "UniversalAMDFormBrowser" to a flashdrive. The next steps are as follows:

1. Boot into USB
2. Device Manager > AMD CBS > NBIO Common Options > GFX Configuration
3. Set iGPU Configuration to "UMA_Specified"
4. Set UMA Framebuffer Size to desired amount **Has to be 512M or More**
5. Press F10
6. Press Y
7. Press power button to power off machine
8. Boot into MacOS and check for Graphics Acceleration

If this still doesn't work, check the NootedRed kext folder in your efi and make sure it was extracted correctly and check the plist to make sure that it is also present there. If all is good in those places, try a different SMBIOS value.

# Credits
**I do not own nor did I develop any of the files or programs mentioned in this documentation.**
Thank you to all those that made this possible.


