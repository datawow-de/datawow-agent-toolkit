---
name: census
description: Zensus-Auswertungen aus den DataWow-Daten - Umsatz aufgeteilt nach demografischen und strukturellen Merkmalen der Kundenregionen (Haushaltsgröße, Altersgruppe, Wohnungsgröße, Gebäudetyp, Familientyp, Kaltmiete, Eigentumsquote, Einwohner, ...), optional gefiltert nach Plattform oder Artikelgruppe. Verwenden, wenn der Nutzer nach Zensus, Demografie, Bevölkerungsstruktur, Zielgruppen oder Umsatz nach einem solchen Merkmal fragt.
---

# DataWow Zensus

## Ziel

Zensus-Auswertungen verbinden aggregierte demografische bzw. strukturelle Merkmale der Wohngegend der Kunden mit dem Umsatz. Die einzige Kennzahl ist der **Umsatz** (gleiche Definition wie im Skill `controlling`, nur Kunden in Deutschland). Andere Kennzahlen wie Deckungsbeiträge, Mengen oder Bestand liefert der Zensus nicht.

Für Abfragen der DataWow-Daten (Plattformen, Artikelgruppen) gelten alle Regeln des Skills `controlling`.

## Ablauf

1. **Merkmal bestimmen**
   - `census_metrics` aufrufen; die verfügbaren Merkmale unterscheiden sich je Kunde.
   - Nur exakt zurückgegebene Namen verwenden. Ist unklar, welches Merkmal der Nutzer meint, nachfragen und die verfügbaren nennen.

2. **Jahr bestimmen**
   - `year` immer explizit setzen. Ohne Angabe liefert `census_dataset` das Vorjahr - nicht alle Jahre.
   - Nennt der Nutzer kein Jahr, das letzte abgeschlossene Jahr nehmen und das in der Antwort erwähnen.

3. **Plattformfilter auflösen** (nur wenn der Nutzer Plattformen oder Kanäle nennt)
   - Die Plattform-IDs mit `execute` aus den DataWow-Daten ermitteln: das ID-Attribut der Plattformebene, zur Kontrolle mit Bezeichnung. Bezeichner aus `get_cube_schema`.
   - Bei einer Plattformgruppe (z. B. „Amazon“) die IDs aller zugehörigen Plattformen ermitteln.
   - Mehrere IDs mit Semikolon trennen. IDs nie raten oder aus Bezeichnungen ableiten.

4. **Artikelgruppenfilter auflösen** (nur wenn der Nutzer Artikelgruppe, Kategorie oder Warengruppe nennt)
   - Die Artikelgruppen-IDs mit `execute` ermitteln: das ID-Attribut der Artikelgruppenebene, zur Kontrolle mit Bezeichnung.
   - Mehrere IDs mit Semikolon trennen. IDs nie raten.

5. **Unterkategorien festlegen**
   - Standard: `useParentChildForProductCategories = false` (nur die genannten Artikelgruppen).
   - `true` nur, wenn ausdrücklich die ganze Kategorie inklusive Unterkategorien gemeint ist.

6. **Abrufen** mit `census_dataset`, nur mit den nötigen Parametern. Filter, die nicht gebraucht werden, weglassen statt leer zu übergeben.

7. **Darstellen**
   - Tabelle mit Merkmalsausprägung, Umsatz und Anteil.
   - Ausprägungen in fachlich sinnvoller Reihenfolge sortieren (z. B. „unter30“, „30bis39“, … „200undmehr“), nicht in der gelieferten Reihenfolge. Bezeichnungen lesbar machen (z. B. „100bis109“ → „100-109 m²“, „2_Personen“ → „2 Personen“).
   - Eine kurze Einordnung: Wo liegt der Umsatzschwerpunkt? Zusammenhänge als Beobachtung formulieren, nicht als Ursache.
   - Keine Kennzahlen erfinden, die der Zensus nicht liefert.

## Antwort von `census_dataset`

Eine Liste von Zeilen, z. B.:

```json
[
  { "Flaechengruppe": "100bis109", "Umsatz": 1287099.80, "AnteilProzent": 8.09 }
]
```

- Die erste Spalte ist die Ausprägung. Ihr Name hängt vom Merkmal ab und ist **nicht immer** gleich dem Merkmalsnamen (bei `Wohnungsgroesse` heißt sie `Flaechengruppe`). Spaltennamen immer aus der Antwort lesen.
- `Umsatz`: Umsatz der Ausprägung. Eine explizite `0` ist ein gültiger Wert und wird als 0 ausgegeben.
- `AnteilProzent`: Anteil am Gesamtumsatz in Prozentpunkten (`8.09` = 8,09 %).
- Eine fehlende Zeile ist nicht dasselbe wie 0. Ist ein Wert leer (`null`), das Feld leer lassen.

## Fehler

- **Unbekannte Plattform- oder Artikelgruppen-IDs**: Die Meldung nennt die IDs, die es nicht gibt. Diese weglassen, erneut abrufen und dem Nutzer sagen, welche Plattform bzw. Artikelgruppe keine Zensusdaten hat.
- **Unbekanntes Merkmal**: `census_metrics` erneut abrufen und nur einen exakt vorhandenen Namen verwenden.
- **Serverfehler**: neutral melden, keine Daten simulieren; ein erneuter Versuch ist erlaubt.
