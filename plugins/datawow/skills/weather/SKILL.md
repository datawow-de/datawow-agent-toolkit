---
name: weather
description: Verbindet das Wetter je deutscher Postleitzahlregion (Temperatur, Regen, Sonnenschein, Wind) mit dem Umsatz aus den DataWow-Daten, um Wettereffekte auf den Absatz zu untersuchen. Verwenden, wenn der Nutzer nach Wetter, Regen, Temperatur, Hitze, Kälte, Sonne oder Saisoneffekten im Zusammenhang mit Umsatz fragt. Nur für Deutschland.
---

# DataWow Wetter

## Ziel

Wetterauswertungen verbinden das gemessene Wetter je Postleitzahlregion mit dem Umsatz. `weather_regions` liefert nur Wetter, keinen Umsatz. Der Umsatz kommt über `execute` aus den DataWow-Daten. Verbunden wird über die Postleitzahl und den Zeitraum.

Ausgewertet wird nur **Deutschland**. Die Wetterdaten kennen nur deutsche Postleitzahlen.

Für Umsatzabfragen gelten alle Regeln des Skills `controlling` (Datenmodell zur Laufzeit abrufen, keine geratenen Bezeichner, keine Technik im Gespräch).

## Postleitzahlebenen

| Ebene | Bedeutung | `postalCodeDigits` |
|---|---|---|
| PLZ-Zone | erste Ziffer, 10 Zonen | 1 |
| PLZ-Region | erste zwei Ziffern, 100 Regionen | 2 |

Beide Ebenen liegen in der Kundenhierarchie unterhalb des Landes. Die exakten Bezeichner der Ebenen, ihres Postleitzahl-Attributs (Verbindungsschlüssel) und ihrer Bezeichnung aus `get_cube_schema` nehmen. Feinere Ebenen als PLZ-Region sind nicht vorgesehen.

## Ablauf

1. **Ebene wählen**: PLZ-Zone für Überblicke und lange Zeiträume; PLZ-Region für regionale Detailfragen, dann mit kurzen Zeiträumen (siehe Zeilenlimit).
2. **Zeitraum und Raster wählen**: `grain` = `MONTH`, `WEEK` oder `DAY`. `from` und `to` sind **Periodenanfänge**, beide inklusive: eine Periode zählt, wenn ihr Beginn im Intervall liegt. Bei `MONTH` den Monatsersten angeben, bei `WEEK` einen Montag. Vorab gegen das Zeilenlimit prüfen.
3. **Deutschland bestimmen**: Den Eintrag für Deutschland auf der Länderebene der Kundenhierarchie aus den Daten ermitteln, nicht raten.
4. **Umsatz je Postleitzahl und Periode abrufen** mit `execute`: Zeilen = gewählte PLZ-Ebene innerhalb Deutschlands, dazu die Periode im gewählten Raster; das Postleitzahl-Attribut als Eigenschaft mitgeben. Die Umsatzdefinition aus `get_calculated_measures` nehmen.
5. **Wetter abrufen** mit `weather_regions` (`grain`, `postalCodeDigits`, `from`, `to`). `postalCodePrefix` nur setzen, wenn nur ein Teil Deutschlands betrachtet wird.
6. **Verbinden** über Postleitzahl **und** Periodenanfang (siehe Verbindungsregeln).
7. **Auswerten und darstellen**: Zusammenhänge als Beobachtung bzw. Korrelation formulieren, nie als Ursache. Auf Einflussfaktoren hinweisen, die nichts mit Wetter zu tun haben (Saison, Aktionen, Feiertage).

## Zeilenlimit

`weather_regions` liefert höchstens **1000 Zeilen** und lehnt größere Anfragen ab, statt zu kürzen. Zeilen = Perioden × Regionen.

| Ebene | Regionen | max. Perioden | DAY | WEEK | MONTH |
|---|---|---|---|---|---|
| PLZ-Zone | 10 | 100 | 100 Tage | ≈ 2 Jahre | ≈ 8 Jahre |
| PLZ-Region | 100 | 10 | 10 Tage | 10 Wochen | 10 Monate |

Ein volles Jahr auf Tagesebene geht nur mit `postalCodePrefix` (eine Zone, 365 Tage). PLZ-Region auf Tagesebene erfordert 10-Tages-Blöcke; für längere Zeiträume besser `WEEK` oder `MONTH`. Eine Ablehnung nennt Perioden und Regionen getrennt - daran ablesen, welcher Faktor kleiner werden muss.

## Verbindungsregeln

Schlüssel ist die Postleitzahl als **Zeichenkette mit führenden Nullen**.

- `weather_regions` liefert `postalCode` als führende Ziffern: `"2"` bei 1 Stelle, `"20"` bei 2 Stellen.
- Es gibt Regionen mit führender Null (`"01"`, `"04"`, `"09"`). Wird ein Wert als Zahl behandelt, wird aus `"01"` eine `1` und die Region fällt still aus der Verbindung.
- Vor dem Verbinden prüfen, in welcher Form das Postleitzahl-Attribut aus den Daten kommt, und beide Seiten angleichen: Zeichenkette, links mit Nullen auf 1 bzw. 2 Stellen aufgefüllt, ohne Länderpräfix.
- Einträge ohne gültige Postleitzahl (z. B. „ohne Zuordnung“) nicht verbinden, aber ihren Umsatz separat ausweisen.
- Zweiter Schlüsselteil: `periodStart` gegen den Periodenanfang aus den Umsatzdaten.

Regionen ohne Gegenstück auf einer Seite nicht stillschweigend weglassen, sondern kenntlich machen.

## Wetterwerte richtig lesen

- Alle Werte sind über die Wetterstationen einer Region **gemittelt**, nicht summiert. `precipitationMmMean` ist der mittlere Niederschlag der Region, keine Summe über Stationen.
- `precipitationMmMax` ist die nasseste einzelne Station.
- `tempMean` ist „die Temperatur“ einer Region und Periode.
- `tempMedianOfStations` ist das Mittel der Stationsmediane, nicht der Median der Region.
- `sunshineMinutesMean` ist die mittlere Sonnenscheindauer in Minuten je Periode; für die Ausgabe in Stunden umrechnen.
- `stations` ist die Zahl der Stationen, die gemeldet haben, nicht die Zahl der Stationen in der Region.
- `complete` zählt vorhandene Stunden, nicht gemessene: Prognosestunden zählen mit, daher kann eine laufende Periode vollständig wirken. Für belastbare Vergleiche abgeschlossene Perioden verwenden und laufende kennzeichnen.
- Eine fehlende Zeile bedeutet „keine Station hat gemeldet“ - **nicht** 0 und nicht durch Nachbarregionen ersetzen.

## Fehler

- „Result set is too large“: Anfrage verkleinern (gröberes Raster, kürzerer Zeitraum, weniger Stellen oder `postalCodePrefix`). Nie unverändert wiederholen.
- Ungültiger Parameter: `postalCodePrefix` nur Ziffern, `grain` nur `DAY`/`WEEK`/`MONTH`, `postalCodeDigits` nur 1-5.
- Serverfehler: neutral melden, keine Daten simulieren; ein erneuter Versuch ist erlaubt.
