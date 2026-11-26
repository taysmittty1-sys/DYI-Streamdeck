# System-Spezifikation für das DIY-Streamdeck

**Stand** 26. November 2026 · **Status** Entwurf für den ersten Prototyp

Der Prototyp besteht aus einem **Raspberry Pi Pico 2**, **fünf Tastern**, **zwei Drehknöpfen** und einem **1,3-Zoll-Farb-LCD**. Der Pico meldet Tastendrücke und Drehbewegungen per USB an eine Python-Anwendung unter Windows 11. Die Anwendung führt die Aktionen aus und sendet den Audiozustand zurück an das Display. Die Tastenbelegung lässt sich am PC ändern.

## Abgleich mit den Decision Records

| Decision Record | Umsetzung und Voraussetzung | Abnahme |
| --- | --- | --- |
| [1 – Programme starten](Decision-records/record1.md) | Eine Taste startet die hinterlegte, lokal installierte `.exe` über `subprocess.Popen`. | Ein Druck startet das gewählte Programm einmal. Ein ungültiger Pfad führt zu einer verständlichen Meldung. |
| [2 – Mikrofon stummschalten](Decision-records/record2.md) | Eine Taste schaltet den in der App ausgewählten Windows-Audioeingang um. Die Meeting-App muss denselben Eingang verwenden. Im gemeinsam genutzten Audiomodus betrifft die Umschaltung alle Anwendungen, die diesen Eingang nutzen. [Windows Core Audio](https://learn.microsoft.com/en-us/windows/win32/coreaudio/endpointvolume-api) | Im Testmeeting kommt nach dem ersten Druck kein Mikrofonsignal mehr an. Nach dem zweiten Druck ist es wieder hörbar. |
| [3 – Stream-Szene wechseln](Decision-records/record3.md) | Zwei Tasten senden die in OBS Studio eingerichteten Hotkeys für „Spiel“ und „Pause“. OBS muss laufen und die Hotkeys bei aktivem Spielfenster annehmen. | Die Pausentaste zeigt die Pausenanzeige im OBS-Programmbild. Die Spieltaste schaltet zurück. |
| [4 – Musik steuern](Decision-records/record4.md) | Eine Taste sendet die Windows-Medientaste für Wiedergabe und Pause. Der verwendete Musikplayer muss diese Medientaste unterstützen. | Bei aktivem Arbeitsfenster pausiert die laufende Musik und wird beim nächsten Druck fortgesetzt. Das Arbeitsfenster bleibt aktiv. |
| [5 – Tasten belegen](Decision-records/Record5.md) | Der Nutzer ändert die Belegung im Einstellungsfenster. Die App speichert sie als JSON-Datei im Windows-Benutzerprofil. | Eine neue Belegung funktioniert direkt nach dem Speichern und bleibt nach einem Neustart erhalten. |

Die fünf Taster decken die Aktionen aus den Decision Records ab. Die Tastenbelegung wird im Einstellungsfenster geändert. LCD und Drehknöpfe ergänzen diese Funktionen. Ihre Belegung ist ein Vorschlag für den Prototyp.

| Bedienelement | Vorgeschlagene Funktion |
| --- | --- |
| Drehknopf 1 | Lautstärke des aktuellen Windows-Standardausgabegeräts regeln, etwa für Kopfhörer oder Lautsprecher. |
| Drehknopf 2 | Eingangspegel des ausgewählten Mikrofons regeln. Die Mikrofontaste schaltet denselben Eingang stumm und wieder frei. |
| LCD | Verbindung zur PC-Anwendung, Ausgabelautstärke, Mikrofonpegel und Stummstatus anzeigen. |

Eine Rastung im Uhrzeigersinn erhöht den jeweiligen Wert um fünf Prozentpunkte auf der Windows-Lautstärkeskala. Gegen den Uhrzeigersinn sinkt der Wert entsprechend. Der Bereich liegt zwischen 0 und 100. Die Stummschaltung wird über die Mikrofontaste bedient.

## Hardware und Material

| Menge | Teil | Anforderung |
| ---: | --- | --- |
| 1 | Raspberry Pi Pico 2 **mit vorgelöteten Stiftleisten** | RP2350-Mikrocontroller mit Micro-USB-B für Daten und Strom. Die Stiftleisten lassen sich direkt ins Steckbrett stecken. [Raspberry Pi Pico-Dokumentation](https://www.raspberrypi.com/documentation/microcontrollers/pico-series.html) · [Pico-2-Datenblatt](https://pip-assets.raspberrypi.com/categories/1005-raspberry-pi-pico-2/documents/RP-008299-DS-3-pico-2-datasheet.pdf) |
| 5 | Unbeleuchtete, steckbrettgeeignete Taster | Momenttaster mit Schließerkontakt (`NO`). Elektrisch sind sie im Ruhezustand offen und schließen beim Drücken. |
| 2 | Mechanische Drehgeber mit Rastung und passenden Knopfkappen | Inkrementelle Drehgeber mit zwei Signalen (`A`, `B`) und gemeinsamem Kontakt (`C`), etwa aus der Bourns-PEC11R-Serie. Für die vorgeschlagene Auswertung je ein Modell mit 24 Rastungen und 24 Impulsen pro Umdrehung wählen. Knopfkappen müssen zur Wellenform und zum Durchmesser passen. [Bourns-Datenblatt](https://www.bourns.com/data/global/pdfs/PEC11R.pdf) |
| 1 | 1,3-Zoll-TFT-LCD mit SPI | Farb-LCD mit **ST7789**, 240 × 240 Pixeln und 3,3-V-tauglicher Versorgung und Logik. Referenzmodul ist das Adafruit-Breakout Nr. 4313. [Displaymodul](https://www.adafruit.cyom/product/4313) · [Anschlussbelegung des Moduls](https://learn.adafruit.com/adafruit-1-3-and-1-54-240-x-240-wide-angle-tft-lcd-displays/pinouts) |
| 4 | 10-kΩ-Widerstände | Je ein Pull-up für die A- und B-Signale der beiden passiven Drehgeber. |
| 1 | Steckbrett | Zum Beispiel ein Steckbrett mit 830 Kontakten für Pico, Taster und Überbrückungen. |
| 1 Satz | Steckbrücken und Stiftleisten | Rund 30 Steckbrücken für Eingänge, Display, 3,3 V und Masse. Je nach Modul Stiftleisten anlöten oder passende Adapter für die Drehgeber verwenden. |
| 1 | USB-Datenkabel | Micro-USB-B am Pico und je nach PC USB-A oder USB-C am anderen Ende. Die Verbindung versorgt das Streamdeck mit Strom und überträgt Daten in beide Richtungen. |
| 1 | Windows-11-PC, x64 | Mit freiem USB-Port. Für den OBS-Test muss der PC auch die gewählte Spielaufnahme verarbeiten können. [OBS-Systemanforderungen](https://obsproject.com/kb/system-requirements) |
| 1 | Mikrofon oder Headset am PC | Für den Test von Record 2 eignet sich auch ein bereits vorhandenes Mikrofon. |

Für den Tischaufbau werden Pico, Taster und Module am Steckbrett verdrahtet. Für lose Stiftleisten oder blanke Drehgeber kommen Lötkolben und Lötzinn hinzu. Ein späteres Gehäuse braucht Ausschnitte für LCD, Taster, Drehgeberachsen und USB-Kabel.

### Pinbelegung und Verdrahtung

| Tasten-ID | Pico-GPIO | Physischer Pin | Beispielbelegung |
| ---: | --- | ---: | --- |
| 1 | GP2 | 4 | Programm starten |
| 2 | GP3 | 5 | Mikrofon umschalten |
| 3 | GP4 | 6 | OBS-Szene „Spiel“ |
| 4 | GP5 | 7 | OBS-Szene „Pause“ |
| 5 | GP6 | 9 | Wiedergabe oder Pause |

| Bauteil und Anschluss | Pico-Signal | Physischer Pin |
| --- | --- | ---: |
| Drehgeber 1 · A | GP16 | 21 |
| Drehgeber 1 · B | GP17 | 22 |
| Drehgeber 2 · A | GP18 | 24 |
| Drehgeber 2 · B | GP19 | 25 |
| LCD · SCK (SPI-Takt) | GP10 | 14 |
| LCD · SI (MOSI) | GP11 | 15 |
| LCD · D/C (Daten oder Befehl) | GP12 | 16 |
| LCD · TCS (Chip Select) | GP13 | 17 |
| LCD · RST (Reset) | GP14 | 19 |
| LCD · Vin | 3V3(OUT) | 36 |
| LCD · GND, gemeinsame Kontakte beider Drehgeber und Taster | GND | 8 oder 23 |

Einen Kontakt pro Taster mit dem angegebenen GPIO verbinden, den anderen an die GND-Leiste. Bei vierbeinigen Tastern je einen Anschluss von den zwei getrennten Kontaktpaaren verwenden. Die Firmware aktiviert für diese Eingänge `Pin.PULL_UP`. Die Drehgeberkontakte `C` kommen an dieselbe GND-Leiste. Je einen 10-kΩ-Widerstand von `A` und `B` nach `3V3(OUT)` anschließen und die vier Drehgeber-GPIOs als Eingänge konfigurieren. [Pico-2-Pinout](https://pip-assets.raspberrypi.com/categories/1005-raspberry-pi-pico-2/documents/RP-008301-DS-1-Pico-2-Pinout.pdf) · [MicroPython-Dokumentation zu `machine.Pin`](https://github.com/micropython/micropython/blob/master/docs/library/machine.Pin.rst)

Das LCD wird an `SPI(1)` mit 3,3 V betrieben. Für den ersten Test sind 10 MHz und SPI-Modus 0 vorgesehen. Falls die SPI-Initialisierung einen MISO-Pin setzt, dafür GP8 am Pico konfigurieren und physisch frei lassen. `SO` und `CCS` sind für den SD-Kartenleser des Moduls und bleiben frei. Die Hintergrundbeleuchtung des Referenzmoduls ist bei offenem `LITE`-Pin eingeschaltet. [LCD-Anschlussbelegung](https://learn.adafruit.com/adafruit-1-3-and-1-54-240-x-240-wide-angle-tft-lcd-displays/pinouts)

## Programmiersprachen und Entwicklungswerkzeuge

| Bereich | Sprache oder Werkzeug | Einsatz |
| --- | --- | --- |
| Firmware | **MicroPython** für den Pico 2 | Taster entprellen, Drehgeber auswerten, Eingaben über USB melden und Displaydaten empfangen. Das Programm wird als `main.py` auf dem Pico gespeichert. [MicroPython-Firmware für Pico 2](https://micropython.org/download/RPI_PICO2/) |
| Firmware-Entwicklung | **Thonny** | MicroPython-Code bearbeiten, auf dem Board speichern und die serielle Ausgabe prüfen. [Thonny und MicroPython](https://github.com/thonny/thonny/wiki/MicroPython) |
| LCD | `st7789py` und MicroPython-`framebuf` | Den ST7789 per `machine.SPI` ansteuern und Text oder Balken für die Audiowerte zeichnen. `st7789py.py` wird direkt auf den Pico kopiert. [MicroPython-Treiber für ST7789](https://github.com/russhughes/st7789py_mpy) |
| Windows-Anwendung | **Python 3.14 (64 Bit)**, **VS Code**, **PowerShell** | Anwendung entwickeln und aus einer virtuellen Umgebung (`venv`) starten. In VS Code den Interpreter aus `.venv` auswählen. [Python unter Windows](https://docs.python.org/3.14/using/windows.html) · [VS Code für Python](https://code.visualstudio.com/docs/languages/python) |
| Oberfläche und Konfiguration | `tkinter`, `json`, `pathlib` | Einstellungsfenster, Dateiauswahl und lokale JSON-Konfiguration. Diese Module gehören zur Python-Standardbibliothek. [Python-Dokumentation zu tkinter](https://docs.python.org/3.14/library/tkinter.html) |
| Serielle Verbindung | `pyserial` | COM-Port öffnen, Tasten- und Drehmeldungen lesen und den Audiostatus senden. [pySerial-Dokumentation](https://pyserial.readthedocs.io/en/latest/shortintro.html) |
| Programmstart | `subprocess` | Die gespeicherte `.exe` direkt per `Popen` starten. [Python-Dokumentation zu subprocess](https://docs.python.org/3.14/library/subprocess.html) |
| Audio | `pycaw` und Windows Core Audio | Aktive Mikrofone finden und den Stummzustand sowie die Lautstärke am jeweiligen Endgerät abfragen und setzen. `pip` installiert die Abhängigkeiten `comtypes` und `psutil` mit. [Pycaw](https://github.com/AndreMiras/pycaw) · [Paketabhängigkeiten](https://github.com/AndreMiras/pycaw/blob/main/pyproject.toml) |
| OBS und Musik | `ctypes` und die Windows-Funktion `SendInput` | Tastenkombinationen sowie die Medientaste `VK_MEDIA_PLAY_PAUSE` als Drücken und Loslassen senden. [Microsoft-Dokumentation zu SendInput](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-sendinput) · [Windows-Tastencodes](https://learn.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes) |
| Funktionstests | **OBS Studio**, eine Meeting-App und ein kompatibler Musikplayer | Zwei OBS-Szenen einrichten, einen Mikrofontest durchführen und die Mediensteuerung prüfen. [OBS Studio](https://obsproject.com/download) |

## Firmware, USB-Protokoll und Windows-Anwendung

### Firmware und USB

1. Die Firmware liest die fünf Taster regelmäßig, zum Beispiel alle 5 ms. Ein Zustandswechsel gilt nach 25 ms stabilem Pegel als bestätigt. Beim Drücken wird genau ein Ereignis erzeugt. Beim Einschalten gehaltene Tasten werden nach dem Loslassen für neue Ereignisse freigegeben.
2. Die Firmware erfasst bei beiden Drehgebern die steigenden und fallenden Flanken der A- und B-Signale. Sie wertet die Reihenfolge der gültigen Quadraturübergänge aus und zählt beim vorgesehenen Drehgeber eine vollständige Folge als eine Rastung. So lässt sich die Drehrichtung erkennen und Kontaktprellen von einer vollständigen Drehbewegung unterscheiden. Die Firmware erfasst Flanken auch während einer Displayaktualisierung, zum Beispiel über GPIO-Interrupts. [Bourns-Drehgeberdatenblatt](https://www.bourns.com/data/global/pdfs/PEC11R.pdf) · [MicroPython-GPIO-Interrupts](https://github.com/micropython/micropython/blob/master/docs/library/machine.Pin.rst)
3. Windows stellt die USB-CDC-Verbindung als COM-Port bereit. Die PC-Anwendung öffnet den ausgewählten Port mit `115200` Baud, `8N1` und einem kurzen Lesetimeout. Der Pico kann eingehende Zeichen mit `select.poll()` abfragen, damit die Anzeige und die Eingabeverarbeitung während der USB-Kommunikation weiterlaufen. Thonny gibt den Port vor dem Start der PC-Anwendung frei. [MicroPython am USB-Port](https://github.com/micropython/micropython/blob/master/docs/rp2/quickref.rst) · [MicroPython `select`](https://github.com/micropython/micropython/blob/master/docs/library/select.rst) · [Windows-USB-Serielltreiber](https://learn.microsoft.com/en-us/windows-hardware/drivers/usbcon/usb-driver-installation-based-on-compatible-ids)

Beide Seiten verarbeiten vollständige ASCII-Zeilen und akzeptieren `LF` oder `CRLF` als Zeilenende. Ungültige Zeilen und MicroPython-Startmeldungen werden ignoriert.

| Richtung | Nachricht | Werte und Beispiel |
| --- | --- | --- |
| Pico → PC | `BUTTON <ID>` | Tasten-ID `1` bis `5`, zum Beispiel `BUTTON 3`. |
| Pico → PC | `ENCODER <ID> <SCHRITT>` | Drehknopf `1` oder `2`. Wert `+1` für eine Rastung im Uhrzeigersinn und `-1` für die Gegenrichtung, zum Beispiel `ENCODER 1 +1`. |
| PC → Pico | `STATUS <AUSGABE> <MIKRO> <STUMM>` | Lautstärke und Mikrofonpegel als ganze Zahlen von `0` bis `100`. Stummstatus `0` oder `1`. Fehlende Werte werden als `-` gesendet. `STATUS 45 70 1` bedeutet 45 % Ausgabe, 70 % Mikrofonpegel und stumm. |

### Anzeige und Audiosynchronisierung

- Nach dem Start zeigt das LCD „PC getrennt“. Nach dem ersten gültigen `STATUS` zeigt es „PC verbunden“ und die aktuellen Audiowerte. Die App sendet den Zustand beim Verbinden, nach Audioaktionen und ungefähr jede Sekunde. Damit erscheinen auch Änderungen aus den Windows-Einstellungen auf dem LCD.
- Das LCD zeigt beispielsweise „Ausgabe 45 %“, „Mikro 70 %“ und bei stummgeschaltetem Mikrofon „Mikro stumm“. Die Firmware zeichnet nur geänderte Anzeigebereiche neu. Fehlende Endgeräte werden mit `--` dargestellt.
- Bleiben gültige Statusmeldungen drei Sekunden aus, zeigt das weiterhin mit Strom versorgte Gerät wieder „PC getrennt“ und blendet alte Audiowerte aus. Die App zeigt einen abgezogenen Pico als getrennt an und erlaubt, ihn nach dem Einstecken erneut zu verbinden.

### Einstellungen und Aktionsausführung

- Das Einstellungsfenster zeigt COM-Port, Verbindungsstatus und die fünf Tasten mit ihrer aktuellen Aktion. Abhängig von der Aktion zeigt es eine Dateiauswahl oder ein Eingabefeld für die Tastenkombination. Der Nutzer wählt in den Audioeinstellungen den Mikrofoneingang für die Mikrofontaste und Drehknopf 2 aus.
- Die Konfigurationsdatei liegt unter `%LOCALAPPDATA%\DIY-Streamdeck\config.json`. Sie enthält den COM-Port, die gemeinsam genutzte Windows-Geräte-ID des Mikrofons und pro belegter Taste den Aktionstyp. Einträge für Programmaktionen enthalten zusätzlich den absoluten `.exe`-Pfad, Einträge für OBS-Aktionen die Tastenkombination.
- Die Anwendung prüft die Einstellungen vor dem Speichern, übernimmt sie nach erfolgreichem Speichern und lädt sie beim nächsten Start. Unbelegte Tasten bleiben inaktiv. Das Lesen am COM-Port läuft unabhängig vom Fenster, damit es auch minimiert auf Tastendrücke reagieren kann.
- Bei einem Tastendruck ruft die App die zugeordnete Aktion auf. Sie startet Programme mit `Popen` ohne Shell. Bei Mikrofonaktionen löst sie die gespeicherte ID zu einem Eingabegerät auf und schaltet anhand von `GetMute` und `SetMute` um. Fehlende Programme oder Eingabegeräte meldet sie im Fenster. [Microsoft-Dokumentation zur EndpointVolume-API](https://learn.microsoft.com/en-us/windows/win32/coreaudio/endpointvolume-api) · [Pycaw-Gerätefunktionen](https://andremiras.github.io/pycaw/_modules/pycaw/utils.html)
- Beim Drehen liest die App den aktuellen Pegel des jeweiligen Endgeräts über `GetMasterVolumeLevelScalar`. Sie addiert oder subtrahiert `0.05`, begrenzt auf `0.0` bis `1.0` und setzt den Wert mit `SetMasterVolumeLevelScalar`. Danach liest sie den tatsächlichen Pegel für das LCD zurück. Drehknopf 1 verwendet das Windows-Standardwiedergabegerät, Drehknopf 2 den gespeicherten Mikrofoneingang. Steht ein Ziel gerade nicht zur Verfügung, meldet die Anwendung das im Fenster und sendet für dieses Ziel `-`. [Windows-Audiopegel](https://learn.microsoft.com/en-us/windows/win32/api/endpointvolume/nn-endpointvolume-iaudioendpointvolume) · [Windows-Standardgerät](https://learn.microsoft.com/en-us/windows/win32/api/mmdeviceapi/nf-mmdeviceapi-immdeviceenumerator-getdefaultaudioendpoint)
- Für OBS in **Einstellungen → Hotkeys** zwei freie Kombinationen für „Spiel“ und „Pause“ einrichten. Unter **Einstellungen → Erweitert** die Hotkeys auch außerhalb des OBS-Fensters aktiv lassen und den Studio-Modus für den direkten Szenenwechsel ausschalten. Die gleichen Kombinationen im Streamdeck speichern. Die Desktop-App, OBS und das Testspiel mit normalen Benutzerrechten starten, da `SendInput` Eingaben an Prozesse mit höheren Rechten nicht senden kann. [OBS-Anleitung](https://obsproject.com/kb/obs-studio-overview) · [Microsoft-Dokumentation zu SendInput](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-sendinput)

## Einrichtung und Aufbaufolge

1. **Pico vorbereiten.** MicroPython-UF2 für den Pico 2 herunterladen. Beim Einstecken des USB-Kabels `BOOTSEL` gedrückt halten und die UF2-Datei auf das eingeblendete Laufwerk kopieren. Nach dem automatischen Neustart in Thonny den Pico am erkannten COM-Port auswählen. [MicroPython-Installationsanleitung](https://micropython.org/download/RPI_PICO2/)
2. **Hardware anschließen und einzeln testen.** Den Pico zum Verdrahten vom USB-Port trennen. Zuerst einen Taster und danach die restlichen Taster und Drehgeber anschließen. Beide Drehrichtungen in Thonny prüfen. Danach das LCD verdrahten, `st7789py.py` auf den Pico kopieren und ein Testbild sowie lesbaren Text anzeigen. Die Firmware als `main.py` auf dem Pico speichern und Thonny vom COM-Port trennen.
3. **PC-Umgebung vorbereiten.** Mit dem Python Install Manager Python 3.14 (64 Bit) bereitstellen. Im Projektordner eine virtuelle Umgebung anlegen und die Bibliotheken darin installieren. Der letzte Befehl öffnet ein Testfenster für `tkinter`. [Python-Installationsanleitung](https://docs.python.org/3.14/using/windows.html)

   ```powershell
   py -3.14 -m venv .venv
   .\.venv\Scripts\python.exe -m pip install pyserial pycaw
   .\.venv\Scripts\python.exe -m tkinter
   ```

4. **Autostart der Firmware prüfen.** Den Pico abziehen und wieder einstecken. In PowerShell mit `.\.venv\Scripts\python.exe -m serial.tools.miniterm COM3 115200` die Ausgabe öffnen. `COM3` durch den Port des Pico ersetzen. Prüfen, ob `BUTTON` und `ENCODER` ankommen, und das serielle Terminal anschließend schließen. [pySerial-Werkzeuge](https://pyserial.readthedocs.io/en/latest/tools.html)
5. **Windows-Anwendung entwickeln.** Zuerst den COM-Port und den Empfang von Tasten- und Drehmeldungen umsetzen. Dann die fünf Tastenaktionen, die Audioregelung, die gespeicherten Einstellungen und das Senden der Statusmeldungen ergänzen.
6. **Gesamtsystem prüfen.** Die fünf Abnahmetests aus den Decision Records und die zusätzlichen Tests unten durchführen. Beim Halten einer Taste darf deren Aktion nur einmal laufen. Nach dem erneuten Verbinden muss die gespeicherte Belegung wieder funktionieren.

### Abnahme für LCD und Drehknöpfe

| Test | Erwartetes Ergebnis |
| --- | --- |
| Drehknopf 1 bei laufender Musikwiedergabe in beide Richtungen drehen. | Die Windows-Ausgabelautstärke steigt oder sinkt. Das LCD zeigt den von Windows gemeldeten Wert. |
| Drehknopf 2 mit einem regelbaren Mikrofoneingang in beide Richtungen drehen und die Mikrofontaste drücken. | Der Eingangspegel ändert sich passend zur Drehung. Der Stummstatus erscheint nach Druck auf die Mikrofontaste und verschwindet beim erneuten Druck. |
| Bei 0 % weiter herunterdrehen und bei 100 % weiter heraufdrehen. Anschließend jeweils die Drehrichtung wechseln. | Der Wert bleibt im Bereich 0 bis 100. Beim Wechsel der Drehrichtung reagiert die Regelung wieder. |
| Die Ausgabelautstärke in Windows ändern, dann die PC-Anwendung schließen und neu starten. Der Pico bleibt am USB-Port. | Die Anzeige übernimmt den neuen Wert. Nach dem Schließen erscheint „PC getrennt“, nach dem Verbinden wieder der aktuelle Status. |

## Lizenzen und Kosten für Software

Für diesen Prototyp brauchst du eine **gültige Windows-Lizenz**. Bei einem bereits lizenzierten Windows-11-PC reicht die vorhandene Lizenz. Für die unten genannten Entwicklungswerkzeuge, Bibliotheken und OBS Studio fallen bei der vorgesehenen Nutzung keine Lizenzgebühren an. [Windows-Lizenzbestimmungen](https://www.microsoft.com/de-de/useterms) · [VS Code FAQ](https://code.visualstudio.com/docs/supporting/faq)

| Bestandteil | Lizenz oder Nutzungsbedingungen |
| --- | --- |
| Windows 11 | Microsoft-Softwarelizenz des PCs. [Windows-Aktivierung](https://support.microsoft.com/de-de/windows/activation/activate-windows) |
| Python | Python Software Foundation License Version 2. Bei Weitergabe einer vollständigen Python-Distribution gelten zusätzlich die Lizenzhinweise der mitgelieferten Komponenten. [Python-Lizenz und Lizenzhinweise](https://docs.python.org/3.14/license.html) |
| MicroPython · Thonny | MIT für den jeweiligen Projektcode. Bei Weitergabe fertiger Pakete auch die mitgelieferten Drittanbieterhinweise beachten. [MicroPython-Lizenz](https://github.com/micropython/micropython/blob/master/LICENSE) · [Thonny-Lizenz](https://github.com/thonny/thonny/blob/master/LICENSE.txt) |
| `pyserial` · `psutil` | BSD-3-Clause. [pySerial-Lizenz](https://github.com/pyserial/pyserial/blob/master/LICENSE.txt) · [psutil-Lizenz](https://github.com/giampaolo/psutil/blob/master/LICENSE) |
| `pycaw` · `comtypes` | MIT. [Pycaw-Lizenz](https://github.com/AndreMiras/pycaw/blob/main/LICENSE) · [comtypes-Lizenz](https://github.com/enthought/comtypes/blob/main/LICENSE.txt) |
| LCD-Treiber `st7789py` | MIT. Bei Weitergabe der Treiberdatei den Lizenz- und Urheberrechtshinweis beilegen. [Treiberlizenz](https://github.com/russhughes/st7789py_mpy/blob/master/LICENSE) |
| Visual Studio Code | Die fertig installierbare Microsoft-Version hat eigene Microsoft-Nutzungsbedingungen. Die Nutzung als Editor ist kostenlos. [VS Code-Lizenz](https://code.visualstudio.com/License/) · [VS Code FAQ](https://code.visualstudio.com/docs/supporting/faq) |
| OBS Studio | Kostenlos, GNU GPL v2 oder neuer. Die Streamdeck-Anwendung steuert die separat installierte Anwendung über Tastendrücke. Dafür ist keine GPL-Lizenzierung des eigenen Projekts erforderlich. [OBS](https://obsproject.com/) · [OBS-Lizenz](https://github.com/obsproject/obs-studio/blob/master/COPYING) · [GNU GPL FAQ](https://www.gnu.org/licenses/gpl-faq.html#MereAggregation) |

Für die USB-Verbindung und die Windows-Schnittstellen ist in diesem Aufbau keine separate Elgato-Lizenz nötig. Meeting-App, Musikplayer und Spiele unterliegen ihren eigenen Nutzungsbedingungen. Für die Tests kannst du bereits vorhandene Programme verwenden.

Bei einer Veröffentlichung kannst du für deinen eigenen Code eine Lizenz wie MIT wählen und sie in einer `LICENSE`-Datei angeben. Falls du Firmware, Python oder Bibliotheken mit deinem Projekt weitergibst, lege die für diese Komponenten erforderlichen Lizenz- und Urheberrechtshinweise bei.
