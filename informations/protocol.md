## Senden Format:
Byte 1-3: Fester Wert
0x55
0x0F
0xA1

Byte 4:
Gibt die Operation an. Diese kann folgende Werte haben:
0x00 -> Tue garnichts
0x01 -> Starte Motor und Heizung
0x02 -> Heizung an
0x03 -> Garen an
0x04 -> ??? 
0x05 -> ???
0x0F -> Schlafen gehen
0xC6 -> Motor einmal drehen

Byte 5:
Tara-Waage
A9 wenn Tara gewünscht ist
Ansonsten 0

Byte 6:
GeschwindigkeitsLevel
Wert kann von 0 bis 10 sein (decimal)

Byte 7:
TemperaturLevel
Wert kann von 0 bis 19 sein (decimal)

Byte 8,9,10,11:
Stehen alle auf 0x00

Byte 12:
Motor Drehrichtung
0 => dreht sich rechts
1 => dreht sich links

Byte 13:
Waage kalibrierung
0 -> garnix
0xE9 -> Kalibrierung starten
0XE6 -> Kalibrierung automatisch(?)
0xE7 -> Kalibrierung manuell(?)

Byte 14:
Prüfsumme. Wird wohl über die Gesamte Summe der Bytes so berechnet:
for value in byte:
    c += value
    c = c & 0xFF

Byte 15:
0xAA fest


55 0F A1 00 00010000000000000006AA


## Empfangsformat:
Erstes byte sollte immer 0x55 sein
Zweites Byte sollte immer 0x1B
3tes Byte sollte immer 0xB1 sein
Letztes Byte sollte immer 0xAA sein
Checksumme über die Gesamte länge des Buffers(-2) wird im vorletzten Byte gespeichert. Genauso wie beim Senden.



## Folgende Operationen sind bekannt:
"55", "0F", "A1", "00", "00", "00", "00", "00", "00", "00", "00", "00", "00" -> Waage Messung beenden
"55", "0F", "A1", "0F", "00", "00", "00", "00", "00", "00", "00", "00", "00" -> Schlafen gehen
"55", "0F", "A1", "00", "00", "00", "00", "00", "00", "00", "00", "00", "E6" -> Waage Messung beitreten
"55", "0F", "A1", "00", "00", "00", "00", "00", "00", "00", "00", "00", "E7" -> Waage Messung 2
"55", "0F", "A1", "0A", "00", "00", "00", "00", "00", "00", "00", "01", "00" -> Motor zurückdrehen
"55", "0F", "A1", "0A", "00", "P1", "00", "00", "00", "00", "00", "00", "00" -> Geschwindigkeit setzen
"55", "0F", "A1", "0A", "00", "00", "P1", "00", "00", "00", "00", "00", "00" -> Temperatur setzen
"55", "0F", "A1", "00", "00", "00", "00", "00", "00", "00", "00", "00", "E9" -> Waage kalibrieren starten
"55", "0F", "A1", "02", "00", "00", "P1", "00", "00", "00", "00", "00", "00" -> Heizung an
"55", "0F", "A1", "01", "00", "P1", "P2", "00", "00", "00", "00", "00", "00" -> Starten des Motors und der Heizung
"55", "0F", "A1", "C6", "00", "00", "00", "00", "00", "00", "00", "00", "00" -> Motor einmal drehen
"55", "0F", "A1", "00", "00", "00", "00", "00", "00", "00", "00", "00", "00" -> Waage Messung start
"55", "0F", "A1", "03", "00", "00", "P1", "00", "00", "00", "00", "00", "00" -> Heizung an
"55", "0F", "A1", "00", "00", "00", "00", "00", "00", "00", "00", "00", "00" -> Stop
"55", "0F", "A1", "0A", "A9", "00", "00", "00", "00", "00", "00", "00", "00" -> Tara Waage



