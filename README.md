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

Die Skills greifen automatisch, sobald eine Frage dazu passt – ein manueller Aufruf ist nicht nötig. Ein Aufruf der Skills ist mit '/datawow' direkt im Chat möglich.

## Voraussetzungen

- Claude-Konto (Web, Desktop oder Claude Code) oder ChatGPT-Konto (Desktop-App)
- DataWow-Zugang

## Einrichtung in Claude (Web und Desktop)

In Team- und Enterprise-Organisationen legt der Owner fest, ob Mitglieder eigene Marktplätze hinzufügen dürfen. Fehlt die Option **Marktplatz hinzufügen**, muss ein Owner sie freigeben.

1. **Einstellungen → Plugins** öffnen.
2. **Hinzufügen → Marktplatz hinzufügen → Aus einem Repository hinzufügen** wählen.
3. Diese Adresse eintragen und mit **Synchronisieren** bestätigen:
   ```
   https://github.com/datawow-de/datawow-agent-toolkit
   ```
4. Zu **Einstellungen → Plugins → Durchsuchen → Persönlich** wechseln.
5. Bei **DataWow** auf **+** klicken, um das Plugin zu installieren.
6. Das Plugin öffnen und im Reiter **Konnektoren** den Connector **datawow** verbinden. Nach Anmeldung mit dem DataWow-Zugang und Bestätigung des Zugriffs bleibt die Verbindung bestehen. Ohne verbundenen Connector laden die Skills zwar, erreichen aber keine Daten.
7. Eine erste Frage stellen, z. B. *„Wie hoch war der Umsatz im letzten Monat je Plattform?“*

**Update:** unter **Einstellungen → Plugins → DataWow** auf **Aktualisieren** klicken.

## Einrichtung in Claude Code

Wurde das Plugin bereits über Claude Web oder Desktop installiert, ist in Claude Code nichts weiter nötig: Es wird mit demselben Claude-Konto automatisch übernommen und nach `/reload-plugins` oder einem Neustart geladen.

In Cloud-Sitzungen im Browser (claude.ai/code) stehen Plugins nicht zur Verfügung.

1. Claude Code starten – im Terminal mit `claude` oder im Code-Tab von Claude Desktop.
2. Marktplatz hinzufügen:
   ```
   /plugin marketplace add https://github.com/datawow-de/datawow-agent-toolkit
   ```
3. Plugin installieren:
   ```
   /plugin install datawow@datawow
   ```
4. Eine erste Frage stellen. Beim ersten Zugriff öffnet sich die DataWow-Anmeldung im Browser.

Startet die Anmeldung nicht automatisch: `/mcp` eingeben, **datawow** wählen und **Authenticate** ausführen.

**Update:**
```
/plugin marketplace update datawow
```
Automatische Updates: `/plugin` → **Marketplaces** → **datawow** → **Enable auto-update**.

## Einrichtung in ChatGPT

Das Plugin ist im Browser sichtbar, lässt sich aber nur in der ChatGPT-Desktop-App installieren und nutzen.

### Für die ganze Organisation

Voraussetzung: Admin-Rechte im ChatGPT-Workspace und ein GitHub-Account.

1. **Workspace Einstellungen → Plugins** öffnen.
2. **Hinzufügen → Import Marketplace** wählen.
3. Diese Adresse eintragen und mit **Marktplatz importieren** bestätigen:
   ```
   https://github.com/datawow-de/datawow-agent-toolkit
   ```
4. Bei Aufforderung den GitHub-Account verbinden.
5. Unter **Plugins** nach **datawow** suchen und das Plugin installieren.

### In der Desktop-App (für einen einzelnen Benutzer)

1. **Einstellungen → Plugins → Hinzufügen** öffnen.
2. **Marketplace hinzufügen** wählen und diese Adresse eintragen:
   ```
   https://github.com/datawow-de/datawow-agent-toolkit
   ```
3. Unter **Plugins** nach **datawow** suchen und das Plugin installieren.

### Anmeldung und Nutzung

- Bei der Installation öffnet sich automatisch die Anmeldeaufforderung. Erscheint sie nicht automatisch, lässt sich die Anmeldung unter **Einstellungen → Plugins → MCPs** starten.
- Im Chat wird das Plugin mit `@datawow` angesprochen, z. B. *„@datawow Wie hoch war der Umsatz im letzten Monat je Plattform?“*

## Hilfe

Bei Fragen: [support@datawow.de](mailto:support@datawow.de)
