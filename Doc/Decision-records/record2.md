# Decision Record 2 – Mikrofon stummschalten

**Status** Vorgeschlagen · [Use Case 2](../User-cases/Usecase2.md)

## Entscheidung

Die Windows-Anwendung schaltet den ausgewählten Mikrofoneingang über Windows Core Audio stumm. So kann dieselbe Taste für verschiedene Meeting-Programme verwendet werden, sofern diese denselben Eingang verwenden. [Microsoft-Dokumentation zur EndpointVolume-API](https://learn.microsoft.com/en-us/windows/win32/coreaudio/endpointvolume-api)

## Umsetzung

1. In der Tastenbelegung wählt der Nutzer das Mikrofon aus, das er auch im Meeting verwendet.
2. Bei einem Tastendruck liest die Anwendung den aktuellen Zustand des Eingangs mit `IAudioEndpointVolume.GetMute` aus und setzt mit `SetMute` den jeweils anderen Zustand. So berücksichtigt sie auch Änderungen, die der Nutzer in den Windows-Einstellungen vorgenommen hat. [GetMute](https://learn.microsoft.com/en-us/windows/win32/api/endpointvolume/nf-endpointvolume-iaudioendpointvolume-getmute) · [SetMute](https://learn.microsoft.com/en-us/windows/win32/api/endpointvolume/nf-endpointvolume-iaudioendpointvolume-setmute)
3. Wenn das ausgewählte Mikrofon fehlt, meldet die Anwendung, dass der Eingang gerade nicht verfügbar ist.

## Prüfung

In einem Testmeeting sprechen und das Mikrofon per Streamdeck-Taste stummschalten. Ein Gesprächspartner bestätigt, dass nach dem ersten Druck kein Mikrofonsignal mehr ankommt und nach dem zweiten Druck wieder Ton übertragen wird.
