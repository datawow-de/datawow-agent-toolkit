# DataWow

Controlling-Assistent für den Onlinehandel auf Basis der DataWow-Daten.

## Skills

| Skill | Zweck |
|---|---|
| `/datawow:controlling` | Umsatz, Deckungsbeiträge (DB1-DB4), Bestand, Reichweite, Retouren, Plattformen, Artikel, Hersteller, Lieferanten |
| `/datawow:weather` | Umsatz im Zusammenhang mit dem Wetter je Postleitzahlregion (nur Deutschland) |
| `/datawow:census` | Umsatz nach Zensusmerkmalen wie Haushaltsgröße, Altersgruppe oder Wohnungsgröße |

Die Skills werden automatisch genutzt, wenn eine Frage dazu passt.

## Verbindung

Das Plugin bringt den DataWow-Connector mit (`.mcp.json`). Beim ersten Zugriff öffnet Claude die DataWow-Anmeldung im Browser; danach bleibt die Verbindung bestehen.

Das Datenmodell, die Begriffe und die kundenspezifischen Merkmale liefert der Connector zur Laufzeit. Sie sind bewusst nicht im Plugin enthalten, weil sie je Kunde abweichen.
