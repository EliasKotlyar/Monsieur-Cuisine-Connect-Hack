## Grober Plan:
1. Firmware dumpen

2. Firmware analysieren

3. Firmware hacken und wieder hochladen


## Durchgeführte Schritte:

1. Chipset ist ein MediaTek MT6580. Es läuft Android 6.0

2. Die Wartungsklappe enthält 2 USB-Ports. Einer linke ist mit 3.3V ausgestatet, der rechte zeigt 0V an.

3. Der rechte Port kann über einen "USB-A male to USB-A male" Kabel/Adapter mit einem Rechner verbunden werden.
![menu](/informations/usb.jpg)

Es bietet sich an einen gewinkelten Stecker mitzubestellen, damit man leichter verbinden kann. Hatte ich aber nicht da :/

Für den rechten Port benötigt man einen "links" gewinkelten Stecker, für den linken entsprechend einen "rechtsgewinkelten"

4. Das System meldet sich beim anschalten mit dem MediaTek "Debug-Port" an

5. Beim hochgefahrenen Zustand erscheint ein MTD-Device. Leider ist dieses nicht zugreifbar. Ich schätze mal das man
dafür erstmal einige Einstellungen treffen muss.

6. Nach kurzer Recherche kann man das System wohl mithilfe der folgenden Anleitung "hacken":
https://privatstrand.dirkschmidtke.de/2016/07/25/china-phones-scatter-datei-erzeugen-und-system-backup-durchfuehren/

Kurzfassung: Mithilfe eines Tools namens "SP-Flash" kann man die Firmware komplett dumpen.

7. Leider benötigt man dafür erstmal eine Scatter-Datei. Diese enthält Informationen, wie der Flash aufgeteilt ist.
Diese ist für jede Baureihe einzigartig und muss somit direkt vom Gerät besorgt werden. 

8. Nach einiger Recherche erhalte ich folgende Anleitung:

https://forum.hovatek.com/thread-21970.html

Diese verwendet ein Extra-Tool um die Firmware mithilfe von SP-Flash zu dumpen. Es wird dabei keine Scatter Datei benötigt. Diese 
wird im laufe des Prozesses erstellt.

SP-Flash findet man relativ einfach über Google.
WWR-Tool habe ich von https://4pda.ru/forum/index.php?showtopic=813767 genommen. Man sollte nicht die aktuelle Beta-Version verwenden,
da diese einen Werbeblock enthält.

10. Ich habe das SP-Flash sowie WwRTool zum laufen gekriegt. Folgendes Bild ergibt der Memory-Test:

```
External RAM:

	Type = DRAM

	Size = 0x40000000 (1024MB/8192Mb)

NAND Flash:

	ERROR: NAND Flash was not detected!

EMMC: 

	 EMMC_PART_BOOT1 	Size = 0x0000000000400000(4MB)
	 EMMC_PART_BOOT2 	Size = 0x0000000000400000(4MB)
	 EMMC_PART_RPMB 	Size = 0x0000000000400000(4MB)
	 EMMC_PART_GP1 	Size = 0x0000000000000000(0MB)
	 EMMC_PART_GP2 	Size = 0x0000000000000000(0MB)
	 EMMC_PART_GP3 	Size = 0x0000000000000000(0MB)
	 EMMC_PART_GP4 	Size = 0x0000000000000000(0MB)
	 EMMC_PART_USER 	Size = 0x00000003a3e00000(14910MB)

UFS: 

	ERROR: UFS was not detected!

============	 RAM Test 	============

```

D.h. Die wichtige Information ist hierbei, das der Flashspeicher etwa 16GB beträgt. Genaue Größe ist 0x3a3e00000

11. Ich habe die komplette Firmware gedumpt. Das dumpen an sich ca 2 Stunden(ca 2.2mb/s) - 111:15 Minutes . Hoffentlich muss man das nicht öfters machen.

12. Das einlesen der Firmware in WWR-Tool gestaltet sich als schwierig. In 2 von 3 Fällen stürzt das Programm ab. Dennoch war es möglich die firmware
damit einzulesen und in kleine Teile zu zerlegen. Anbei ein Bild des Layouts:
![layout](/informations/layout.png)

Daneben war es mir möglich ein Scatter-File zu dumpen. Dieses findet sich unter "l706_dfbh_v_695scatter.txt"

13. Ich habe das system.img runtergeladen und folgende 3 Zeilen in die build.prop hinzugefügt:
```
persist.service.adb.enable=1                                                    
persist.service.debuggable=1
persist.sys.usb.config=mtp,adb

```
Den Tipp habe ich von:
https://forum.xda-developers.com/showthread.php?t=2335799
Danach habe ich das ganze mit SP-Flash zurückgeflasht. Das zurückflashen dauert genau 5min, Datenrate ist 17MB/s.

14. Das Gerät reagiert nun auf ADB. Damit ist eine weitere Untersuchung des Geräts möglich.   

15. Ich habe folgende Keys entdeckt mit denen man das Gerät steuern kann.
```
Öffne Fenster Menü:
adb shell input keyevent 187
Home Taste(kehrt zu der Koch-App zurück):
adb shell input keyevent 3
Öffne Browser:
adb shell input keyevent 64
Zurück-Taste:
adb shell input keyevent 4
```

16.  Apps lassen sich mit folgendem Befehl auflisten sowie starten:
```
adb shell pm list packages
adb shell monkey --pct-syskeys 0 -p '<appname>' -v 500

```
Ein paar Apps:
```
Filemananger:
adb shell monkey --pct-syskeys 0 -p 'com.mediatek.filemanager' -v 500
Einstellungen Android:
adb shell monkey --pct-syskeys 0 -p 'com.android.settings' -v 500
```

17. Es gibt eine App namens "Endurancetest". Diese ist für den Debug-Modus verantwortlich. Ich habe mir die Funktionsweise des Tools angeschaut und habe folgende Erkenntnisse gezogen.

Ich gehe davon aus, das das System folgendermaßen angesteuert wird:
Es gibt ein Serielles Device unter /dev/ttyMT0
Dieses wird mit einigen Kommandos gesteuert..

18.  Ich habe versucht mit folgendem Tool eine CWM zu bauen:
https://forum.hovatek.com/thread-21839.html

Leider bootet das Gerät immer in die Stock-CWM


20. Ich kann den Trebuchet Launcher installieren auf dem Gerät. Dies macht man wie folgt:
Download von : https://www.apkmirror.com/apk/cyanogenmod/trebuchet-2/
```
adb install com.lineageport.trebuchet_8.1.0.16-8010016_minAPI21\(nodpi\)_apkmirror.com.apk
adb shell monkey --pct-syskeys 0 -p 'com.android.settings' -v 500
```
19. Ich habe probiert CWM zum laufen gekriegt. Erstens muss man auf der System-Partition eine Datei löschen,
da der Bootmanager sonst immer die Recovery mit einer Stock-Variante überschreibt. Der Sachverhalt ist hier beschrieben:
https://privatstrand.dirkschmidtke.de/2016/07/25/root-und-custom-recovery-auf-mediatek-smartphones/

20. Leider kriege ich keine der offizielen "Tools" zum bauen von CWM zum laufen. D.h. ich werde es wohl selbst kompilieren müssen.

21. Ich habe nun eine Custom Recovery compiliert. Diese ist noch etwas unausgereift(falscher Modus) aber langt normalerweise.

Anbei ein paar Anleitungen aus welchen ich das nötige Fachwissen entnommen habe:
https://gist.github.com/zawzaww/5096677c990cb6ec5bcd722162d61372

