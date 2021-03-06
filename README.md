# OBD-II Display für Rasperry PI/Linux und Tiny-CAN

[![OBD-Display](https://github.com/MHS-Elektronik/OBD-Display/blob/master/doku/obd_display.jpg)](https://github.com/MHS-Elektronik/OBD-Display/blob/master/doku/obd_Display.jpg)

# Funktionsumfang

* Abfrage von Messdaten (SID 0x01)
  * Konfigurierbare Anzeige der PID Werte von 0x00 bis 0x4E
* Abfrage Fahrzeuginformationen (SID 0x09)
  * Aufschlüsselung der Fahrgestellnummer (VIN - Vehicle Identification Number) in Hersteller, Land, Modelljahr, Seriennummer, ...)
* Fehlercodes lesen (DTC Diagnostic Trouble Codes) (SID 0x03)
  * Klartext anzeige der Fehlercodes mittels Datenbank
* CAN Rohdaten anzeigen (CAN-Trace)
* Automatische Erkennung der CAN Hardware und Baudrate

# Screenshots 

[![OBD-Display](https://github.com/MHS-Elektronik/OBD-Display/blob/master/doku/obd_start.png)](https://github.com/MHS-Elektronik/OBD-Display/blob/master/doku/obd_start.png)

[![OBD-Display](https://github.com/MHS-Elektronik/OBD-Display/blob/master/doku/obd_pid_select.jpg)](https://github.com/MHS-Elektronik/OBD-Display/blob/master/doku/obd_pid_select.jpg)

[![OBD-Display](https://github.com/MHS-Elektronik/OBD-Display/blob/master/doku/obd_list_view.jpg)](https://github.com/MHS-Elektronik/OBD-Display/blob/master/doku/obd_list_view.jpg)

[![OBD-Display](https://github.com/MHS-Elektronik/OBD-Display/blob/master/doku/obd_error_codes.jpg)](https://github.com/MHS-Elektronik/OBD-Display/blob/master/doku/obd_error_codes.jpg)


# Hardware
* Linux PC z.B. Rasberry PI
* Tiny-CAN z.B. Tiny-CAN I-XL, Bezugsquelle: http://www.mhs-elektronik.de
* OBD-CAN Kabel

[![OBD-Display](https://github.com/MHS-Elektronik/OBD-Display/blob/master/doku/obd2_cable.jpg)](https://github.com/MHS-Elektronik/OBD-Display/blob/master/doku/obd2_cable.jpg)

# Installation

Zuerst muss das Tiny-CAN Softwarepaket installiert werden, für den Betrieb der Software wird nur die "libmhstcan.so" benötigt. "OBD-Display" sucht diese Datei im Verzeichnis "/opt/tiny_can/can_api". 

Die Datei "tiny_can_raspberry_XXX.tar.gz" von "http://www.mhs-elektronik.de" downloaden und im Verzeichnis "/opt" entpacken. XXX durch die aktuellste Version ersetzen. Nach dem entpacken kann das Archiv gelöscht werden. Zuvor müssen noch die Zugriffsrechte für das "/opt" Verzeichnis gesetzt werden.

    $ sudo chmod -R 775 /opt
    $ cd /opt
    $ mv /home/pi/tiny_can_raspberry_XXX.tar.gz .
    $ tar -xzvf  tiny_can_raspberry_XXX.tar.gz
    $ rm tiny_can_rasberry_XXX.tar.gz

Die Tiny-CAN API kompilieren: 
    $ cd /opt/tiny_can/can_api/src/mhstcan/linux
    $ make
    $ mv libmhstcan.so ../../..
Die Lib ist dem Paket bereits enthalten, in der Regel ist das kompilieren nicht notwendig.

"Git" installieren:
    $ sudo apt-get install git

"ObdDisplay" von "Git-Server" holen:
    $ cd /opt
    $ git clone http://github.com/MHS-Elektronik/OBD-Display.git

Development Pakete installieren:
    $ sudo apt-get install gtk2.0-dev

"ObdDisplay" kompilieren:
    $ cd /opt/OBD-Display/linux
    $ make

ObdDisplay starten:
    $ cd /opt/OBD-Display/linux/bin
    $ ./ObdDisplay

# Quellen/Links
Zwei der Kerndateien, „obd_db.c“ und „obd_decode.c“ in diesem Projekt basieren auf der OBD-II API von Ethan Vaughan, https://github.com/ejvaughan/obdii

Beim ISO-TP Treiber habe ich ein wenig aus dem ISP-TP Linux Kernel Treiber von Oliver Hartkopp gespickt, https://github.com/hartkopp/can-isotp

Die meisten Informationen habe ich diesen Dokument von emotive entnommen:
http://www.emotive.de/documents/WebcastsProtected/Transport-Diagnoseprotokolle.pdf
Wirklich sehr lesenswert, kann ich nur empfehlen!

Andere Quellen die ich benutzt habe:
https://github.com/iotlabsltd/pyvin/tree/master/pyvin
ftp://ftp.nhtsa.dot.gov/manufacture
https://de.wikipedia.org/wiki/Fahrzeug-Identifizierungsnummer
https://en.wikipedia.org/wiki/On-board_diagnostics
https://de.wikipedia.org/wiki/ISO_15765-2

 

# Bildquellen
## Hintergrundgrafik, VW Käfer:
Quelle: https://commons.wikimedia.org/wiki/File:Der_Samtrote_Sonderkäfer.jpg
Urheber: Marco Strohmeier
Lizenz: Public Domain

## OBD-Stecker:
Quelle: https://commons.wikimedia.org/wiki/File:OBD2-Buchse-Stecker-Belegung.jpg
Urheber: https://de.wikipedia.org/wiki/Benutzer:Losch
Lizenz: CC BY-SA 4.0  https://creativecommons.org/licenses/by-sa/4.0

Die Bilddateien wurden für meine Zwecke angepasst.


# Tipps und Tricks
## Das Programm Automatisch starten
Sollte keine Maus oder Tastatur an dem PI angeschlossen sein ist es sinnvoll das Programm automatisch zu starten. Kopieren Sie dazu die Datei "J1939Display.desktop" aus dem Verzeichnis "tools" ins Verzeichnis "/etc/xdg/autostart".

    $ sudo cp /opt/J1939Display/tools/J1939Display.desktop /etc/xdg/autostart

## Bildschirmschoner abschalten
Die Datei "/etc/lightdm/lightdm.conf" im Editor als "admin" öffnen.

    $ sudo leafpad /etc/lightdm/lightdm.conf
In der Sektion "SeatDefaults" die Zeile "xserver-command" wie folgt abändern oder wenn nicht vorhanden ergänzen.

    [SeatDefaults]
    ....
    xserver-command=X -s 0 -dpms
    ....

## Bildschirm-Anzeige drehen
Die Datei "/boot/config.txt" im Editor als "admin" öffnen.

    $ sudo leafpad /boot/config.txt
In die Datei folgende Zeile eintragen
    lcd_rotate=2
Nach dem Neustart sollte das Bild um 180° gedreht sein und Du kannst das Display umdrehen, womit sich der microUSB Stecker oben befindet.

## Maus-Zeiger verschwinden lassen
Für unsere spezielle Anwendung ist der Mauszeiger störend. Um den Cursor ganz einfach zu entfernen, können wir ein Paket installieren, welches ihn ausblendet:
    $ sudo apt-get install unclutter
Nach einem Neustart ist der Cursor nicht mehr sichtbar.













