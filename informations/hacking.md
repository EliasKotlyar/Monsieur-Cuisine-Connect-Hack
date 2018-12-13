## Grober Plan:
1. Firmware dumpen

2. Firmware analysieren

3. Firmware hacken und wieder hochladen


## Durchgeführte Schritte:

1. Chipset ist ein MediaTek MT6580. Es läuft Android 6.0

2. Die Wartungsklappe enthält 2 USB-Ports. Einer linke ist mit 3.3V ausgestatet, der rechte zeigt 0V an.

3. Der rechte Port kann über einen "USB-A male to USB-A male" Kabel/Adapter mit einem Rechner verbunden werden.
![menu](/informations/usb.jpg)

Es bietet sich an einen gewinkelten Stecker mitzubestellen, damit man leichter verbinden kann. 

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

Diese verwendet ein Extra-Tool um die Firmware mithilfe von SP-Flash zu dumpen. Es wird keine Scatter Datei benötigt.

9. Nachdem die Firmware vorliegt, müsste ich erstmal die Firmware etwas abändern, damit diese den ADB-Dienst startet: 

https://forum.xda-developers.com/showthread.php?t=2335799

Desweiteren würde ich auch gleich die Wireless-ADB aktivieren.