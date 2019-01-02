4564/5000
# Installation instructions:


# Hardware requirements
1. Monsieur Cuisine Connect
2. PC with Windows or Ubuntu
3. USB A Male to USB A Male Cable or Adapter & USB Extension Cable
![Menu](/informations/usb.jpg)

4. Small Torx screwdriver

## Software requirements:

### Linux:
adb

SP Flash Tools

Disable the modemmanager package (apt-get remove modemmanager) for SP Flash tools to work properly.

### Windows:
Install SP-Flash tools with the following instructions (https://spflashtool.com/):

https://www.chinamobilemag.de/tutorials/sp-flash-tool-universal-tutorial.html

Install ADB with the following instructions:

https://www.androidpit.de/adb-treiber-android-windows


# Installation:

1. Turn off the device

2. Unscrew the maintenance cover. This requires the torx screwdriver.

3. Connect the PC to the device using the USB cable. * Use only the right port labeled "Android USB" *. A connection with the left can destroy your device.

![USB PORTs](/informations/usbports.jpg)


4. Start SP-Flash tools. Use the scatter file l706_dfbh_v_695scatter.txt to initialize the tool.

5. Now make backups of any partitions. The following instructions will help:
https://www.android-hilfe.de/forum/anleitungen-fuer-mediatek-geraete.2400/anleitung-backup-readback-per-sp-flash-tool.746503.html

The most important is a backup of your "boot" partition. To do this, set the following:
`` `
Start Address:
0x0000000001d20000
lenght:
0x0000000001000000
`` `
Read this part of the flash memory as "boot.img". Furthermore, you could also backups the recovery partition.

*** Attention, do not skip this step !! The backup of the boot partition is needed in the following steps. ***

6. Using SP-FlashTools, flash the partition "boot" with the recovery file from "hacks/recovery.img"

7. Restart the MC, it should automatically load the TWRP recovery. If not, something went wrong when flashing.

8. Start the ADB shell. Then execute the following commands:
`` `
# Mount the system partition
mount / system /
# Allowed ADB
echo "persist.service.adb.enable = 1" >> /system/build.prop
echo "persist.service.debuggable = 1" >> /system/build.prop
echo "persist.sys.usb.config = mtp, adb" >> /system/build.prop
# Displays the Android buttons again:
echo "qemu.hw.mainkeys = 0" >> /system/build.prop
# Disable Recovery Check (https://privatstrand.dirkschmidtke.de/2016/07/25/root-and-custom-recovery-on-mediatek-smartphones/)
rm /system/recovery-from-boot.p
`` `

9. Push your original boot partition from step 5 onto the device via ADB.
Furthermore, you once again load the "hacks/recovery.img" file on your device:
`` `
adb push bootimg.bin / sdcard /
adb push recovery.bin / sdcard
`` `

13. Flashing with TWRP the two files accordingly.
Your original boot.img comes in the boot partition, the "hacks/recovery" file in the recovery partition

15. Restart the device. It should start the original MC firmware. Now you can also access the device during via ADB.

16. Now install the Trebuchet Launcher or alternatively your favorite launcher via ADB:
https://www.apkmirror.com/apk/cyanogenmod/trebuchet-2/trebuchet-2-8-1-0-16-release/trebuchet-8-1-0-16-android-apk-download/

`` `
adb install com.lineageport.trebuchet_8.1.0.16-8010016_minAPI21 \ (nodpi \) _ apkmirror.com.apk
`` `
Once the launcher is installed, you have to start it. To do this, execute the following command:
`` `
adb shell at start -a android.intent.action.MAIN -c android.intent.category.HOME com.lineageport.trebuchet

`` `
Then you can set by settings the Trebuchet Launcher as default launcher.
*** Attention: These controls are just wallpapers. Do not be confused, the Android buttons are as usual below ***

17. Download the Google Apps: https://opengapps.org/

`` `
Platform: ARM
Android Version: 6.0
Variant: pico
`` `

Other variants (micro, full) do not work - there produce only crashes / reboot loops.

You can boot into recovery by using
`` `
adb reboot recovery
`` `
and flash the ZIP file. In addition, you can also use the device with zb. SuperSU rooten (if needed)

** Attention, the device behaves a bit strange at the next start. Just tap the screen once in a while and it will reboot and everything will be fine **

18. Get new wallpapers from the Google Store and equip your device with them. I recommend the app "Walloid"
