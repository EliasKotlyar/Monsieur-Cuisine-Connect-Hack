# Anleitung für die Installation:


# Hardware-Voraussetzungen
1. Monsieur Cuisine Connect
2. PC mit Windows oder Ubuntu
3. USB A-Male zu USB-A Male Kabel
4. Kleinen Torx-Schraubenzieher 


# Installation:

1. Schalte das Gerät ab

2. Schraube die Wartungsklappe auf mithilfe des 10ner-Torx

3. Verbinde den PC mit dem Gerät mithilfe des USB-Kabels

4. Lade die Software SP-Flash Tool von https://spflashtool.com/ herunter

5. Bei Windows:

Installiere die Software mit folgender Anleitung:

https://www.chinamobilemag.de/tutorials/sp-flash-tool-universal-tutorial.html


Bei Ubuntu:

Deaktiviere das Packet "modemmanager"(apt-get remove modemmanager)

6. Benütze die Scatter-Datei l706_dfbh_v_695scatter.txt um das Tool zu initialisieren.

7. Fertige nun Backups jeglicher Partionen an. Dazu hilft folgende Anleitung:
https://www.android-hilfe.de/forum/anleitungen-fuer-mediatek-geraete.2400/anleitung-backup-readback-per-sp-flash-tool.746503.html

Das wichtigste ist ein Backup deiner "boot"-Partition. Dazu stellt man folgendes ein:
```
Start-Address:
0x0000000001d20000
Lenght:
0x0000000001000000
```
Lese diesen Teil des Flash-Speichers als "boot.img" ein

8. Beschreibe mithilfe von SP-FlashTools die Partition "boot" mit der Recovery-Datei aus "hacks/recovery.img"

9. Boote dein Gerät, es sollte in der TWRP-Recovery landen.

10. Installiere die ADB-Treiber auf deinen PC. Du solltest eine ADB-Shell sehen sobald du
auf das Gerät draufgehst.

11. Gehe per ADB dadrauf und führe folgende Kommandos aus:
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

12. Pushe per ADB die Recovery sowie deine originale boot-partition aus Schritt 7 auf das Gerät

adb push bootimg.bin /sdcard/
adb push recovery.bin /sdcard

13. Flashe mit TWRP die beiden Dateien entsprechend als Image. Damit wird das originale Bootimage wiederhergestellt sowie die Recovery in die zugehörige Recovery Partition geflashed.

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

17. Man lädt nun die Google Apps runter: https://opengapps.org/

```
Platform: ARM
Android Version: 6.0
Variant: pico
```

Andere Varianten funktionieren nicht - es gibt dann Abstürze etc.

Man bootet in die Recovery mithilfe von 
```
adb reboot recovery 
```
und flasht die ZIP-Datei. Zusätzlich kann man das Gerät auch mit zb. SuperSU rooten(falls benötigt)


