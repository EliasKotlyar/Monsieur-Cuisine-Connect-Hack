# Anleitung für die Installation:


# Hardware-Voraussetzungen
1. Monsieur Cuisine Connect
2. PC mit Windows oder Ubuntu
3. USB A-Male zu USB-A Male Kabel oder Adapter + Verlängerungskabel USB
![menu](/informations/usb.jpg)

4. Kleinen Torx-Schraubenzieher 

## Software Voraussetzungen:

### Linux:
adb

SP-Flash-Tools

Deaktiviere das Packet "modemmanager"(apt-get remove modemmanager), damit SP-Flash-Tools korrekt funktioniert.

### Windows:
Installiere SP-Flash-Tools mit folgender Anleitung(https://spflashtool.com/):

https://www.chinamobilemag.de/tutorials/sp-flash-tool-universal-tutorial.html

Installiere ADB mit folgender Anleitung:

https://www.androidpit.de/adb-treiber-android-windows


# Installation:

1. Schalte das Gerät ab

2. Schraube die Wartungsklappe auf. Dafür wird der Torx-Schraubenzieher benötigt.

3. Verbinde den PC mit dem Gerät mithilfe des USB-Kabels. *Benützt nur den rechten Port mit der Aufschrift "Android USB"*. Eine Verbindung mit dem Linken kann euer Gerät zerschießen.

![USB PORTs](/informations/usbports.jpg)


4. Start SP-Flash-Tools. Benütze die Scatter-Datei l706_dfbh_v_695scatter.txt um das Tool zu initialisieren.

5. Fertige nun Backups jeglicher Partionen an. Dazu hilft folgende Anleitung:
https://www.android-hilfe.de/forum/anleitungen-fuer-mediatek-geraete.2400/anleitung-backup-readback-per-sp-flash-tool.746503.html

Das wichtigste ist ein Backup deiner "boot"-Partition. Dazu stellt man folgendes ein:
```
Start-Address:
0x0000000001d20000
Lenght:
0x0000000001000000
```
Lese diesen Teil des Flash-Speichers als "boot.img" ein. Desweiteren könntest du auch die Recovery Partition backuppen.

***Achtung, diesen Schritt nicht überpringen!! Das Backup der Boot-Partition wird an einer anderen Stelle benötigt.*** 

6. Beschreibe mithilfe von SP-FlashTools die Partition "boot" mit der Recovery-Datei aus "hacks/recovery.img"

7. Starte den MC neu, es sollte in der TWRP-Recovery automatisch in der Recovery landen. Falls nicht - nochmals flashen.

8. Starte die ADB-Shell. Führe danach folgende Kommandos aus:
```
# Erlaubt ADB
echo "persist.service.adb.enable=1" >> /system/build.prop                                                 
echo "persist.service.debuggable=1" >> /system/build.prop
echo "persist.sys.usb.config=mtp,adb" >> /system/build.prop
# Enable Android-Buttons:
echo "qemu.hw.mainkeys=0" >> /system/build.prop
# Disable Recovery Check(https://privatstrand.dirkschmidtke.de/2016/07/25/root-und-custom-recovery-auf-mediatek-smartphones/)
rm /system/recovery-from-boot.p
```

9. Pushe per ADB  deine originale boot-partition aus Schritt 5 auf das Gerät.
Desweiteren lädst du wieder einmal die "hacks/recovery.img" Datei auf dein Gerät:
```
adb push bootimg.bin /sdcard/
adb push recovery.bin /sdcard
```

13. Flashe mit TWRP die beiden Dateien entsprechend.
Deine originale boot.img kommt in die boot-partition, die "hacks/recovery"-Datei in die recovery partition

15. Starte das Gerät neu. Es sollte die originale MC-Firmware starten.  Jetzt kannst du auch im laufenden Betrieb per ADB das Gerät zugreifen.

16. Installiere nun per ADB den Trebuchet Launcher oder alternativ deinen Lieblingslauncher:
https://www.apkmirror.com/apk/cyanogenmod/trebuchet-2/trebuchet-2-8-1-0-16-release/trebuchet-8-1-0-16-android-apk-download/

```
adb install com.lineageport.trebuchet_8.1.0.16-8010016_minAPI21\(nodpi\)_apkmirror.com.apk
```
Sobald der Launcher installiert ist, muss man diesen noch aktivieren. Dazu führt man folgenden Befehl aus:
```
adb shell am start -a android.intent.action.MAIN -c android.intent.category.HOME com.lineageport.trebuchet

```
Danach kann man per Einstellungen den Trebuchet Launcher als Default-Launcher festlegen.

17. Man lädt nun die Google Apps runter: https://opengapps.org/

```
Platform: ARM
Android Version: 6.0
Variant: pico
```

Andere Varianten(micro,full) funktionieren nicht - es gibt dann nur noch Abstürze etc.

Man bootet in die Recovery mithilfe von 
```
adb reboot recovery 
```
und flasht die ZIP-Datei. Zusätzlich kann man das Gerät auch mit zb. SuperSU rooten(falls benötigt)

**Achtung, das Gerät benimmt sich beim nächsten Start etwas komisch. EInfach paarmal auf den Bildschirm tippen, und es bootet neu**

18. Neue Wallpaper aus dem Google Store holen und sein Gerät damit ausrüsten. Ich empfehle Walloid
