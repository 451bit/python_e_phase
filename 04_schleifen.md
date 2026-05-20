# Kapitel 4 – Schleifen

> **Kontext:** Schleifen brauchen wir, um das Spielfeld zeilenweise auszugeben, Spielrunden zu wiederholen und alle Schiffkoordinaten zu durchlaufen.

---

## 4.1 Die `while`-Schleife

```python
while Bedingung:
    Anweisung(en)
```

Der Block wird **wiederholt**, solange die Bedingung `True` ist.

```python
versuche = 3

while versuche > 0:
    print("Versuche übrig:", versuche)
    versuche -= 1

print("Keine Versuche mehr!")
```

> **Typische Fehler:**  
> - Vergessen, die Laufvariable zu verändern → Endlosschleife  
> - Falsche Abbruchbedingung (< statt <=)

### Spielrunden-Schleife

```python
spiel_laeuft      = True
versuche_uebrig   = 20
versenkter_schiffe = 0
gesamt_schiffe    = 3

while spiel_laeuft:
    # Ein Spielzug (vereinfacht)
    print("Noch", versuche_uebrig, "Versuche.")
    zeile  = int(input("Zeile : "))
    spalte = int(input("Spalte: "))

    versuche_uebrig -= 1

    # Spielende prüfen
    if versenkter_schiffe == gesamt_schiffe:
        print("Gewonnen!")
        spiel_laeuft = False
    elif versuche_uebrig == 0:
        print("Verloren – keine Versuche mehr!")
        spiel_laeuft = False
```

---

## 4.2 Die `for`-Schleife

```python
for Variable in Sequenz:
    Anweisung(en)
```

Die Schleife durchläuft jeden Wert der Sequenz **einmal**.

```python
schiff = [(2, 3), (2, 4), (2, 5)]

for koordinate in schiff:
    print("Koordinate:", koordinate)
```

Ausgabe:
```
Koordinate: (2, 3)
Koordinate: (2, 4)
Koordinate: (2, 5)
```

---

## 4.3 Die `range()`-Funktion *(Python-Besonderheit)*

`range()` erzeugt eine Folge von Ganzzahlen. Das ist die häufigste Art, eine `for`-Schleife zu steuern.

### Formen von `range()`

| Aufruf | Bedeutung | Werte |
|--------|-----------|-------|
| `range(n)` | 0 bis n-1 | `0, 1, 2, …, n-1` |
| `range(start, stop)` | start bis stop-1 | `start, start+1, …, stop-1` |
| `range(start, stop, step)` | mit Schrittweite | z. B. `0, 2, 4, 6, 8` |

```python
# Zahlen 0 bis 4
for i in range(5):
    print(i)          # 0 1 2 3 4

# Zahlen 1 bis 10
for i in range(1, 11):
    print(i)          # 1 2 3 ... 10

# Nur gerade Zahlen 0–8
for i in range(0, 10, 2):
    print(i)          # 0 2 4 6 8

# Rückwärts 9 bis 0
for i in range(9, -1, -1):
    print(i)          # 9 8 7 ... 0
```

### Vergleich mit Java

```java
// Java
for (int i = 0; i < 10; i++) { ... }
```

```python
# Python – äquivalent
for i in range(10): ...
```

---

## 4.4 Spielfeld ausgeben – verschachtelte Schleifen

Für ein 2-D-Feld benötigt man zwei ineinander verschachtelte `for`-Schleifen:

```python
FELD_GROESSE = 10

# Kopfzeile mit Spaltennummern
print("  ", end="")
for spalte in range(FELD_GROESSE):
    print(spalte, end=" ")
print()  # Zeilenumbruch

# Zeilen ausgeben
for zeile in range(FELD_GROESSE):
    print(zeile, end=" ")          # Zeilennummer
    for spalte in range(FELD_GROESSE):
        print("~", end=" ")        # Wasserzeichen
    print()                         # Zeilenumbruch
```

Ausgabe (Ausschnitt):
```
   0 1 2 3 4 5 6 7 8 9
0  ~ ~ ~ ~ ~ ~ ~ ~ ~ ~
1  ~ ~ ~ ~ ~ ~ ~ ~ ~ ~
...
```

---

## 4.5 `break` und `continue`

### `break` – Schleife sofort verlassen

```python
versuchsliste = [5, 3, 7, 2, 8]

for wert in versuchsliste:
    if wert == 7:
        print("Gefunden!")
        break          # Schleife abbrechen
    print("Wert:", wert)
```

### `continue` – Aktuellen Durchlauf überspringen

```python
for i in range(10):
    if i % 2 == 0:
        continue       # Gerade Zahlen überspringen
    print(i)           # Gibt 1 3 5 7 9 aus
```

---

## 4.6 `while` vs. `for` – wann was?

| Situation | Empfehlung |
|-----------|-----------|
| Anzahl der Wiederholungen vorher bekannt | `for` mit `range()` |
| Über eine Liste/Sequenz iterieren | `for` |
| Schleife läuft, bis eine Bedingung eintritt | `while` |
| Spielrunde / Benutzerdialog | `while` |

---

## 4.7 Vollständiges Beispiel: Schiffe zählen

```python
flotte = [
    [(0, 0), (0, 1), (0, 2), (0, 3)],   # Schlachtschiff
    [(3, 5), (4, 5), (5, 5)],            # Kreuzer
    [(7, 7), (7, 8)],                    # Zerstörer
]

gesamt_felder = 0

for schiff in flotte:
    for koordinate in schiff:
        gesamt_felder += 1

print("Schiffe:", len(flotte))
print("Schiff-Felder gesamt:", gesamt_felder)
```

---

## Übungsaufgaben

### Aufgabe 4.1 – Spielfeld ausgeben
Schreibe ein Programm, das ein 10 × 10-Spielfeld als Gitter aus `~`-Zeichen ausgibt.  
Die Zeilen (0–9) und Spalten (0–9) sollen beschriftet sein.

---

### Aufgabe 4.2 – Countdown
Schreibe eine `while`-Schleife, die von 20 rückwärts bis 1 zählt.  
Bei 10 soll `"Halbzeit!"` ausgegeben werden.  
Am Ende: `"Keine Versuche mehr!"`.

---

### Aufgabe 4.3 – Alle Koordinaten eines Schiffes ausgeben
Gegeben:
```python
schiff = [(1, 0), (1, 1), (1, 2), (1, 3)]
```
Gib jede Koordinate in der Form `"Feld (1, 0): Schiff"` aus.

---

### Aufgabe 4.4 – range() erkunden
Schreibe `for`-Schleifen mit `range()`, die folgende Zahlenfolgen ausgeben:
- a) `0 1 2 3 4 5 6 7 8 9`
- b) `1 2 3 4 5 6 7 8 9 10`
- c) `0 2 4 6 8`
- d) `9 8 7 6 5 4 3 2 1 0`

---

### Aufgabe 4.5 – Schiff-Koordinaten prüfen
Gegeben eine Flotte als Liste von Schiffen (jedes Schiff = Liste von Tupeln).  
Lies eine Koordinate ein und prüfe mit verschachtelten `for`-Schleifen, ob sie zu einem Schiff gehört.  
Gib aus: `"Treffer!"` oder `"Wasser!"`.

---

### Aufgabe 4.6 – Spielrunden-Simulation (while)
Simuliere eine vereinfachte Spielrunde:
- Beginne mit 10 Versuchen.
- Frage nach Zeile und Spalte.
- Ist die Koordinate `(4, 5)` (das einzige Schiff) → `"Treffer – gewonnen!"` und Schleife beenden.
- Sonst: `"Wasser!"`, Versuche dekrementieren.
- Keine Versuche mehr → `"Verloren!"` und Schleife beenden.

---

> Musterlösungen zu allen Aufgaben: [Kapitel 10 – Lösungen](10_loesungen.md)
