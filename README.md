# Python – Einführungsphase Informatik

Dieses Lernmaterial begleitet die Einführungsphase Informatik (Themenfeld E.3: *Grundlagen der Programmierung*) nach dem Kerncurriculum des Hessischen Ministeriums für Kultus, Bildung und Chancen (HMKB).

Alle Beispiele und Aufgaben sind **reine Konsolenprogramme** – keine grafischen Oberflächen.  
Als roter Faden dient das Spiel **Schiffe versenken** (Battleship), das Schritt für Schritt aufgebaut wird.

---

## Inhaltsverzeichnis

| Kapitel | Thema |
|---------|-------|
| [01 – Variablen und Datentypen](01_variablen_und_datentypen.md) | Variablen, Datentypen, Typkonvertierung, Ein-/Ausgabe |
| [02 – Operatoren und logische Ausdrücke](02_operatoren_und_ausdruecke.md) | Arithmetik, Vergleiche, logische Operatoren |
| [03 – Verzweigungen](03_verzweigungen.md) | if / elif / else, Mehrfachverzweigungen |
| [04 – Schleifen](04_schleifen.md) | while, for, range(), break, continue |
| [05 – String-Operationen](05_strings.md) | Methoden, Slicing, f-Strings |
| [06 – Listen und Tupel](06_listen_und_tupel.md) | Listen als Arrays, 2-D-Felder, Tupel |
| [07 – Funktionen](07_funktionen.md) | Definition, Parameter, Rückgabewerte |
| [08 – Spezielle Python-Methoden](08_spezielle_methoden.md) | map, enumerate, zip, list comprehension |
| [09 – Projekt: Schiffe versenken](09_projekt_schiffe_versenken.md) | Vollständiges Konsolenspiel |
| [10 – Lösungen](10_loesungen.md) | Musterlösungen zu allen Übungsaufgaben |

---

## Verwendete Python-Version

```
Python 3.x  (getestet mit Python 3.10+)
```

Kein Drittanbieter-Paket erforderlich. Alle Programme laufen direkt im Terminal mit:

```bash
python3 dateiname.py
```

---

## Besonderheiten dieses Kurses

- **Listen werden zunächst wie Arrays in Java behandelt** – die Größe wird vorab festgelegt, Elemente werden per Index gesetzt, *nicht* mit `append()`.
- Grafische Oberflächen (GUI) sind *nicht* Bestandteil dieser Materialien.
- Die objektorientierte Modellierung (Klassen/Objekte) ist Thema des Folgekurshalbjahres Q1.

---

## Spielkonzept "Schiffe versenken"

Das Spielfeld ist ein **10 × 10-Raster** (2-D-Liste). Jede Zelle enthält einen von vier Zuständen:

| Zeichen | Bedeutung |
|---------|-----------|
| `~` | Wasser (unbekannt) |
| `S` | Schiff (auf dem eigenen Feld) |
| `X` | Treffer |
| `O` | Wasser (bereits beschossen) |

Ein Schiff wird als **Liste von Koordinaten-Tupeln** gespeichert, z. B.:

```python
schiff = [(2, 3), (2, 4), (2, 5)]   # horizontales 3er-Schiff in Reihe 2
```

Die Flotte ist eine **Liste von Schiffen**:

```python
flotte = [
    [(0, 0), (0, 1), (0, 2), (0, 3)],   # Schlachtschiff (Länge 4)
    [(3, 5), (4, 5), (5, 5)],            # Kreuzer (Länge 3)
    [(7, 7), (7, 8)],                    # Zerstörer (Länge 2)
]
```

Dieses Datenmodell zieht sich durch alle Kapitel.
