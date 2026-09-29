# DataWow für KI-Assistenten

DataWow-Daten direkt im KI-Assistenten nutzen: Umsatz, Deckungsbeiträge, Bestand und Retouren im Gespräch abfragen und auswerten – ohne Berichte zu bauen.

## Enthaltene Plugins

### DataWow

Controlling-Assistent für den Onlinehandel.

| Skill | Wofür |
|---|---|
| `/datawow:controlling` | Umsatz, Deckungsbeiträge (DB1–DB4), Bestand, Reichweite, Retouren, Plattformen, Artikel, Hersteller, Lieferanten |
| `/datawow:weather` | Umsatz im Zusammenhang mit dem Wetter je Postleitzahlregion (nur Deutschland) |
| `/datawow:census` | Umsatz nach Zensusmerkmalen wie Haushaltsgröße, Altersgruppe oder Wohnungsgröße |

Die Skills greifen automatisch, sobald eine Frage dazu passt – ein manueller Aufruf ist nicht nötig.

## Voraussetzungen

- Claude-Konto (claude.ai, Claude Desktop oder Claude Code)
- DataWow-Zugang

## Einrichtung in Claude (Web und Desktop)

1. **Einstellungen → Plugins** öffnen.
2. **Hinzufügen → Marktplatz hinzufügen → Aus einem Repository hinzufügen** wählen.
3. Diese Adresse eintragen und mit **Synchronisieren** bestätigen:
   ```
   https://github.com/datawow-de/datawow-agent-toolkit
   ```
4. Zu **Einstellungen → Plugins → Durchsuchen → Persönlich** wechseln.
5. Bei **DataWow** auf **+** klicken, um das Plugin zu installieren.
6. Eine erste Frage stellen, z. B. *„Wie hoch war der Umsatz im letzten Monat je Plattform?“*
7. Beim ersten Zugriff öffnet sich die DataWow-Anmeldung. Nach Anmeldung und Bestätigung des Zugriffs bleibt die Verbindung bestehen.

**Aktualisieren:** unter **Einstellungen → Plugins → DataWow** auf **Aktualisieren** klicken.

## Einrichtung in Claude Code

1. Claude Code starten – im Terminal mit `claude` oder im Code-Tab von Claude Desktop.
2. Marktplatz hinzufügen:
   ```
   /plugin marketplace add datawow-de/datawow-agent-toolkit
   ```
3. Plugin installieren:
   ```
   /plugin install datawow@datawow
   ```
4. Eine erste Frage stellen. Beim ersten Zugriff öffnet sich die DataWow-Anmeldung im Browser.

Startet die Anmeldung nicht automatisch: `/mcp` eingeben, **datawow** wählen und **Authenticate** ausführen.

**Aktualisieren:**
```
/plugin marketplace update datawow
```
Automatische Updates: `/plugin` → **Marketplaces** → **datawow** → **Enable auto-update**.

## Hilfe

Bei Fragen: [support@datawow.de](mailto:support@datawow.de)
