# Decision Record 5 – Tasten belegen

**Status** Vorgeschlagen · [Use Case 5](../User-cases/Usecase5.md)

## Entscheidung

Die Tastenbelegung wird in einem Einstellungsfenster der Windows-Anwendung geändert und lokal in einer JSON-Datei gespeichert. Die Zuordnung gehört zur Anwendung, weil sie die Aktionen ausführt und die Belegung direkt nach dem Speichern übernehmen kann.

## Umsetzung

1. Das Fenster zeigt die Streamdeck-Tasten anhand ihrer festen IDs an. Der Nutzer wählt eine Taste und eine Aktion aus, zum Beispiel „Programm starten“, „Mikrofon umschalten“, „OBS-Hotkey senden“ oder „Wiedergabe/Pause“.
2. Je nach Aktion blendet die Anwendung die passenden Einstellungen ein, etwa eine Dateiauswahl für den Programmpfad oder die Eingabe einer OBS-Tastenkombination. Sie prüft beim Speichern, ob die nötigen Angaben vorhanden sind.
3. Die Anwendung speichert zu jeder belegten ID den Aktionstyp und dessen Einstellungen im Windows-Benutzerprofil. Sie übernimmt Änderungen nach dem Speichern und lädt die Belegung beim nächsten Start erneut. Unbelegte Tasten lösen keine Aktion aus.

## Prüfung

Eine Taste von „Programm starten“ auf „Wiedergabe/Pause“ umstellen und speichern. Die Taste steuert danach den Musikplayer. Nach einem Neustart der Anwendung gilt dieselbe Belegung.
