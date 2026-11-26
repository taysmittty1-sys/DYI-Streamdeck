# Decision Record 3 – Stream-Szene wechseln

**Status** Vorgeschlagen · [Use Case 3](../User-cases/Usecase3.md)

## Entscheidung

Für den Prototyp wird OBS Studio verwendet. Die Spielansicht und die Pausenszene bekommen je eine eigene Streamdeck-Taste. Jede Taste wählt direkt die gewünschte Szene aus. OBS stellt dafür Hotkeys pro Szene bereit. [OBS Studio Overview Guide](https://obsproject.com/kb/obs-studio-overview#hotkeys)

## Umsetzung

1. In OBS die Szenen „Spiel“ und „Pause“ anlegen. Die Spielszene erhält eine Spielaufnahme, die Pausenszene eine Pausenanzeige.
2. Unter **Einstellungen → Hotkeys** für jede Szene eine freie Tastenkombination zum Wechseln einstellen.
3. Unter **Einstellungen → Erweitert** die Option **Hotkeys nie deaktivieren** wählen, damit OBS die Hotkeys auch bei aktivem Spielfenster auswertet. Für den Prototyp den Studio-Modus ausgeschaltet lassen, damit der Szenenwechsel direkt im Programmbild erscheint. [OBS-Hotkey-Einstellungen](https://github.com/obsproject/obs-studio/blob/master/frontend/settings/OBSBasicSettings.cpp) · [OBS-Anleitung zum Studio-Modus](https://obsproject.com/kb/obs-studio-overview#studio-mode)
4. Dieselben Tastenkombinationen den entsprechenden Streamdeck-Tasten zuweisen. Die Windows-Anwendung sendet bei einem Druck die gespeicherte Kombination mit `SendInput`. [Microsoft-Dokumentation zu SendInput](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-sendinput)

## Prüfung

OBS und ein Spiel öffnen. Bei aktivem Spielfenster die Pausentaste und danach die Spieltaste drücken. Im OBS-Programmbild erscheint zuerst die Pausenanzeige, anschließend wieder die Spielansicht.
