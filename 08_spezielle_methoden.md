# Kapitel 8 – Spezielle Python-Methoden

> **Kontext:** Python bietet eingebaute Funktionen, die häufige Muster elegant lösen: Eingaben in Zahlen umwandeln, Elemente mit Index durchlaufen, zwei Listen gleichzeitig verarbeiten.

---

## 8.1 `map()` – Funktion auf alle Elemente anwenden

```python
map(funktion, iterable)
```

`map()` wendet eine Funktion auf **jedes Element** einer Sequenz an und gibt ein map-Objekt zurück.  
Um eine Liste zu erhalten, muss man es mit `list()` konvertieren.

### Beispiel: Eingabe-String in Integer-Liste umwandeln

```python
# Koordinateneingabe: "2 7" → [2, 7]
eingabe = input("Zeile Spalte (z.B. '2 7'): ")
teile   = eingabe.split()           # ["2", "7"]
zahlen  = list(map(int, teile))     # [2, 7]

zeile  = zahlen[0]
spalte = zahlen[1]
print(f"Ziel: ({zeile}, {spalte})")
```

Noch kompakter mit Entpacken:

```python
zeile, spalte = map(int, input("Zeile Spalte: ").split())
print(f"Ziel: ({zeile}, {spalte})")
```

> Das ist eine in Python sehr verbreitete Idiom für mehrteilige Ganzzahl-Eingaben.

### Eigene Funktion mit `map()`

```python
def verdopple(x):
    return x * 2

zahlen     = [1, 2, 3, 4, 5]
verdoppelt = list(map(verdopple, zahlen))
print(verdoppelt)    # [2, 4, 6, 8, 10]
```

Mit Lambda-Funktion (anonyme Einzeiler-Funktion):

```python
verdoppelt = list(map(lambda x: x * 2, zahlen))
```

---

## 8.2 `filter()` – Elemente filtern

```python
filter(funktion, iterable)
```

`filter()` behält nur die Elemente, für die die Funktion `True` zurückgibt.

```python
zahlen = [0, 3, 7, -1, 5, 0, 2]

# Nur positive Zahlen
positiv = list(filter(lambda x: x > 0, zahlen))
print(positiv)    # [3, 7, 5, 2]
```

### Im Spielkontext: Noch nicht versenkter Schiffe herausfiltern

```python
def ist_noch_aktiv(schiff_status):
    """schiff_status ist True, wenn das Schiff noch nicht versenkt ist."""
    return schiff_status

status_liste = [True, False, True, True, False]
aktive = list(filter(ist_noch_aktiv, status_liste))
print("Aktive Schiffe:", len(aktive))
```

---

## 8.3 `enumerate()` – Index und Wert zusammen

```python
enumerate(iterable, start=0)
```

`enumerate()` liefert bei jeder Iteration ein Tupel `(index, wert)`.

```python
felder = ["~", "S", "X", "~", "O"]

for i, zelle in enumerate(felder):
    print(f"Index {i}: {zelle}")
```

Ausgabe:
```
Index 0: ~
Index 1: S
Index 2: X
Index 3: ~
Index 4: O
```

### Spielfeld mit Zeilennummern ausgeben

```python
GROESSE = 10
feld    = [["~"] * GROESSE for _ in range(GROESSE)]

for zeilennr, zeile in enumerate(feld):
    print(f"{zeilennr:2} ", end="")
    for zelle in zeile:
        print(f" {zelle}", end="")
    print()
```

### `enumerate` mit `start`-Parameter

```python
monate = ["Jan", "Feb", "Mar", "Apr"]
for nr, monat in enumerate(monate, start=1):
    print(f"{nr}. {monat}")
# 1. Jan  2. Feb  3. Mar  4. Apr
```

---

## 8.4 `zip()` – Mehrere Listen parallel durchlaufen

```python
zip(iterable1, iterable2, ...)
```

`zip()` kombiniert mehrere Sequenzen Element für Element zu Tupeln.

```python
spieler  = ["Anna", "Ben", "Clara"]
punkte   = [42, 37, 55]

for name, p in zip(spieler, punkte):
    print(f"{name}: {p} Punkte")
```

Ausgabe:
```
Anna: 42 Punkte
Ben: 37 Punkte
Clara: 55 Punkte
```

### Im Spielkontext: Zwei Spielfelder parallel ausgeben

```python
def zeige_beide_felder(feld_spieler, feld_gegner, groesse):
    """Zeigt eigenes Feld und Gegnersfeld nebeneinander."""
    print("Eigenes Feld          Gegnersfeld")
    print("  " + " ".join(str(i) for i in range(groesse))
          + "     "
          + "  " + " ".join(str(i) for i in range(groesse)))
    for nr, (zeile_s, zeile_g) in enumerate(zip(feld_spieler, feld_gegner)):
        # Eigenes Feld (mit Schiffen)
        print(f"{nr} " + " ".join(zeile_s), end="   ")
        # Gegnersfeld (Schiffe versteckt)
        gegner_anzeige = ["~" if z == "S" else z for z in zeile_g]
        print(f"{nr} " + " ".join(gegner_anzeige))
```

---

## 8.5 `sorted()` und `reversed()`

```python
zahlen  = [5, 2, 8, 1, 9, 3]

aufsteigend  = sorted(zahlen)               # [1, 2, 3, 5, 8, 9]
absteigend   = sorted(zahlen, reverse=True) # [9, 8, 5, 3, 2, 1]

print(list(reversed(zahlen)))  # [3, 9, 1, 8, 2, 5]
```

---

## 8.6 `sum()`, `min()`, `max()`, `len()`

```python
schiff_laengen = [4, 3, 3, 2, 2, 2, 1, 1, 1, 1]   # Klassische Flotte

print(sum(schiff_laengen))   # 20  – Gesamtfelder belegt
print(max(schiff_laengen))   # 4   – längstes Schiff
print(min(schiff_laengen))   # 1   – kürzestes Schiff
print(len(schiff_laengen))   # 10  – Anzahl Schiffe
```

---

## 8.7 List Comprehension – kurze Listenerzeugung

Eine **List Comprehension** ist eine kompakte Schreibweise, um Listen zu erzeugen:

```python
# Lange Schreibweise
quadrate = []
for i in range(10):
    quadrate.append(i ** 2)

# List Comprehension
quadrate = [i ** 2 for i in range(10)]
print(quadrate)   # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

Mit Bedingung:

```python
gerade = [i for i in range(20) if i % 2 == 0]
print(gerade)     # [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

### Spielfeld initialisieren mit List Comprehension

```python
GROESSE = 10

# Einzeilige Variante
feld = [["~"] * GROESSE for _ in range(GROESSE)]

# Alle Koordinaten eines Schiffs aus Startkoordinate berechnen
start_zeile, start_spalte, laenge = 2, 3, 4
schiff = [(start_zeile, start_spalte + i) for i in range(laenge)]
# [(2, 3), (2, 4), (2, 5), (2, 6)]
```

---

## Übungsaufgaben

### Aufgabe 8.1 – `map()` für Koordinateneingabe
Schreibe ein Programm, das mit **einer Eingabezeile** Zeile und Spalte einliest (Format `"3 7"`) und diese mit `map()` in zwei Integer-Variablen umwandelt.

---

### Aufgabe 8.2 – `enumerate()` für Spielfeldausgabe
Schreibe eine Funktion `zeige_feld(feld)`, die `enumerate()` verwendet, um die Zeilennummern automatisch zu erzeugen.

---

### Aufgabe 8.3 – `zip()` für Spieler-Statistik
Gegeben:
```python
spieler = ["Anna", "Ben", "Clara"]
treffer = [7, 5, 9]
schuesse = [15, 12, 14]
```
Berechne mit `zip()` für jeden Spieler die Trefferquote (`treffer / schuesse * 100`) und gib sie aus.

---

### Aufgabe 8.4 – `filter()` für aktive Schiffe
Gegeben eine Liste von Schiffsgrößen und eine Liste mit `True`/`False` (ob versenkt):
```python
groessen = [4, 3, 3, 2, 2]
versenkt = [True, False, True, False, False]
```
Filtere die Größen der noch **nicht** versenkten Schiffe heraus und gib sie aus.

---

### Aufgabe 8.5 – List Comprehension: Schiff erzeugen
Schreibe eine Funktion `schiff_erstellen(start_zeile, start_spalte, laenge, horizontal)`:
- Wenn `horizontal=True`: Schiff wächst in Spaltenrichtung.
- Wenn `horizontal=False`: Schiff wächst in Zeilenrichtung.
- Nutze eine **List Comprehension**.
- Rückgabe: Liste von Tupeln.

---

### Aufgabe 8.6 – `map()` mit eigener Funktion
Schreibe eine Funktion `zelle_fuer_gegner(zelle)`:
- `"S"` → `"~"` (Schiff verstecken)
- alle anderen Zeichen → unverändert

Wende sie mit `map()` auf eine komplette Spielfeldzeile an.

---

> Musterlösungen zu allen Aufgaben: [Kapitel 10 – Lösungen](10_loesungen.md)
