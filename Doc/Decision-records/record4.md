# Decision Record 4 – Musik steuern

**Status** Vorgeschlagen · [Use Case 4](../User-cases/Usecase4.md)

## Entscheidung

Eine Streamdeck-Taste übernimmt die Funktion der Windows-Medientaste für Wiedergabe und Pause. Damit kann der Nutzer die Musik aus dem aktuellen Programm heraus steuern, sofern sein Musikplayer auf die Medientaste reagiert. [Microsoft-Dokumentation zu virtuellen Tastencodes](https://learn.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes)

## Umsetzung

1. Der gewünschten Streamdeck-Taste die Aktion „Wiedergabe/Pause“ zuweisen.
2. Bei einem Tastendruck sendet die Windows-Anwendung über `SendInput` ein Drücken und Loslassen der Medientaste `VK_MEDIA_PLAY_PAUSE`. Der Musikplayer wechselt damit zwischen Wiedergabe und Pause. [Microsoft-Dokumentation zu SendInput](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-sendinput)

## Prüfung

Musik starten und den Fokus auf ein anderes Programm setzen. Mit dem ersten Tastendruck pausiert die Musik. Mit dem zweiten läuft sie weiter. Das Arbeitsfenster bleibt dabei aktiv.
