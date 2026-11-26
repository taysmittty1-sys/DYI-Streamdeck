# Decision Record 1 – Programme starten

**Status** Vorgeschlagen · [Use Case 1](../User-cases/Usecase1.md)

## Entscheidung

Für den ersten Prototyp ist eine Python-Anwendung unter Windows vorgesehen. Der Mikrocontroller meldet Tastendrücke über eine serielle USB-Verbindung. Die Anwendung führt die hinterlegte Aktion aus. Dadurch kann der Nutzer verschiedene Programme auswählen, ohne die Firmware ändern zu müssen. [pySerial-Dokumentation](https://pyserial.readthedocs.io/en/latest/shortintro.html)

## Umsetzung

1. Der Mikrocontroller entprellt die Taste und sendet pro Tastendruck einmal ihre ID, zum Beispiel `BUTTON 1` als Textzeile.
2. Die Anwendung liest die Zeile mit `pySerial` und ermittelt anhand der ID den in der Tastenbelegung gespeicherten Pfad zur `.exe`-Datei.
3. Sie startet das Programm mit `subprocess.Popen`. Falls die Datei fehlt oder der Start fehlschlägt, zeigt sie eine Fehlermeldung an. [Python-Dokumentation zu subprocess](https://docs.python.org/3/library/subprocess.html)

## Prüfung

Eine Taste mit einem installierten Programm belegen. Ein Tastendruck startet das Programm einmal. Mit einem ungültigen Pfad erscheint eine Fehlermeldung.
