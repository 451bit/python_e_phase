# Kapitel 6 – Listen und Tupel

> **Kontext:** Das Spielfeld ist ein 10 × 10-Raster – eine Liste von Listen. Jedes Schiff ist eine Liste von Koordinaten-Tupeln. Diese Datenstrukturen sind das Herzstück von Schiffe versenken.

---

## 6.1 Listen – Grundlagen

Eine **Liste** ist eine geordnete, änderbare Sammlung von Elementen.

```python
felder = ["~", "~", "~", "~", "~"]
```

- Elemente stehen in eckigen Klammern `[]`
- Elemente werden durch Komma getrennt
- Elemente haben eine feste Reihenfolge (Indizes 0, 1, 2, …)
- Elemente können geändert werden

### Zugriff per Index

```python
felder = ["~", "S", "~", "X", "O"]
#          0    1    2    3    4

print(felder[0])    # "~"
print(felder[1])    # "S"
print(felder[-1])   # "O"  – letztes Element
```

### Element ändern

```python
felder[2] = "X"     # Index 2 auf "X" setzen
print(felder)       # ["~", "S", "X", "X", "O"]
```

### Länge

```python
print(len(felder))  # 5
```

---

## 6.2 Listen wie Arrays in Java behandeln

> **Kursbesonderheit:** Wir behandeln Listen vorerst wie Arrays fixer Größe – kein `append()`, kein `remove()`.  
> Die Größe wird beim Erstellen festgelegt und danach nicht mehr verändert.

### Liste mit fixer Größe erstellen

```java
// Java
String[] felder = new String[10];
Arrays.fill(felder, "~");
```

```python
# Python – Äquivalent
GROESSE = 10
felder  = ["~"] * GROESSE      # ["~", "~", "~", ..., "~"]
```

`["~"] * n` erstellt eine Liste mit `n` Wiederholungen des Elements `"~"`.

```python
felder = [0] * 5        # [0, 0, 0, 0, 0]
felder = [False] * 3    # [False, False, False]
```

### Elemente nur per Index setzen

```python
GROESSE = 5
treffer = [False] * GROESSE    # [False, False, False, False, False]

treffer[2] = True              # Index 2 getroffen
treffer[4] = True              # Index 4 getroffen

print(treffer)    # [False, False, True, False, True]
```

---

## 6.3 Über Listen iterieren

```python
felder = ["~", "S", "X", "~", "O"]

# Mit for-in
for zelle in felder:
    print(zelle)

# Mit range() und Index (wie in Java)
for i in range(len(felder)):
    print(i, felder[i])
```

---

## 6.4 Zweidimensionale Listen (2-D-Arrays)

Ein **2-D-Array** ist eine Liste, deren Elemente selbst Listen sind.

```java
// Java
String[][] feld = new String[10][10];
```

```python
# Python
GROESSE = 10
feld = [["~"] * GROESSE for _ in range(GROESSE)]
```

> **Warum kein `[["~"] * 10] * 10`?**  
> Diese Kurzschreibweise erzeugt 10 *Referenzen auf dieselbe* innere Liste – Änderungen an einer Zeile verändern alle Zeilen! Deshalb verwendet man den Ausdruck mit `for`.

### Zugriff auf eine Zelle: `feld[zeile][spalte]`

```python
feld[2][3] = "S"               # Zelle in Zeile 2, Spalte 3
print(feld[2][3])              # "S"
```

### Spielfeld ausgeben

```python
GROESSE = 10
feld = [["~"] * GROESSE for _ in range(GROESSE)]

# Ein Schiff eintragen
feld[2][3] = "S"
feld[2][4] = "S"
feld[2][5] = "S"

# Kopfzeile
print("   ", end="")
for s in range(GROESSE):
    print(f"{s:2}", end="")
print()

# Zeilen
for z in range(GROESSE):
    print(f"{z:2} ", end="")
    for s in range(GROESSE):
        print(f" {feld[z][s]}", end="")
    print()
```

Ausgabe (Ausschnitt):
```
     0 1 2 3 4 5 6 7 8 9
 0   ~ ~ ~ ~ ~ ~ ~ ~ ~ ~
 1   ~ ~ ~ ~ ~ ~ ~ ~ ~ ~
 2   ~ ~ ~ S S S ~ ~ ~ ~
...
```

---

## 6.5 Tupel

Ein **Tupel** ist eine geordnete, **unveränderliche** Sammlung von Elementen.

```python
koordinate = (3, 7)           # Zeile 3, Spalte 7
print(koordinate[0])          # 3
print(koordinate[1])          # 7
```

- Elemente stehen in runden Klammern `()`
- Elemente können **nicht** geändert werden (`koordinate[0] = 5` → Fehler)
- Ideal für feste Datenpakete wie Koordinaten

### Tupel entpacken

```python
koordinate = (3, 7)
zeile, spalte = koordinate    # Entpacken
print(f"Zeile {zeile}, Spalte {spalte}")
```

### Vergleich Liste vs. Tupel

| Eigenschaft | Liste | Tupel |
|-------------|-------|-------|
| Syntax | `[1, 2, 3]` | `(1, 2, 3)` |
| Änderbar | ja | nein |
| Verwendung | veränderliche Daten | feste Werte |
| als Schlüssel in dict | nein | ja |

---

## 6.6 Schiffe als Liste von Tupeln

```python
# Ein Schiff: Liste von Koordinaten-Tupeln
schlachtschiff = [(0, 0), (0, 1), (0, 2), (0, 3)]
kreuzer        = [(3, 5), (4, 5), (5, 5)]
zerstoerer     = [(7, 7), (7, 8)]

# Flotte: Liste von Schiffen
flotte = [schlachtschiff, kreuzer, zerstoerer]

# Auf alle Koordinaten zugreifen
for schiff in flotte:
    for koordinate in schiff:
        zeile, spalte = koordinate
        print(f"  ({zeile}, {spalte})")
```

### Schiff auf dem Feld markieren

```python
GROESSE = 10
feld    = [["~"] * GROESSE for _ in range(GROESSE)]

def schiff_eintragen(feld, schiff):
    for zeile, spalte in schiff:
        feld[zeile][spalte] = "S"

schiff_eintragen(feld, kreuzer)
```

---

## 6.7 Treffer prüfen

```python
def ist_treffer(schiff, zeile, spalte):
    return (zeile, spalte) in schiff
```

### Schiff vollständig versenkt?

```python
def ist_versenkt(schiff, feld):
    for zeile, spalte in schiff:
        if feld[zeile][spalte] != "X":
            return False
    return True
```

---

## 6.8 Slicing bei Listen

```python
zahlen = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

print(zahlen[2:5])    # [2, 3, 4]
print(zahlen[:3])     # [0, 1, 2]
print(zahlen[7:])     # [7, 8, 9]
print(zahlen[::2])    # [0, 2, 4, 6, 8]
```

---

## Übungsaufgaben

### Aufgabe 6.1 – Spielfeld initialisieren
Erstelle ein 10 × 10-Spielfeld als Liste von Listen, gefüllt mit `"~"`.  
Prüfe mit `print(len(feld))` und `print(len(feld[0]))`, dass es wirklich 10 × 10 ist.

---

### Aufgabe 6.2 – Schiff eintragen
Trage folgende Schiffe in das Spielfeld ein (Zeichen `"S"`):
- Schiff 1: `[(0, 0), (0, 1), (0, 2)]`
- Schiff 2: `[(5, 3), (6, 3), (7, 3), (8, 3)]`

Gib das Spielfeld anschließend aus.

---

### Aufgabe 6.3 – Treffer markieren
Lese Zeile und Spalte ein.  
Prüfe, ob `feld[zeile][spalte] == "S"`:
- Ja → setze die Zelle auf `"X"` und gib `"Treffer!"` aus.
- Nein → setze die Zelle auf `"O"` und gib `"Wasser!"` aus.

---

### Aufgabe 6.4 – Schiff versenkt?
Gegeben ein Schiff als Liste von Tupeln und ein Spielfeld.  
Schreibe Code (ohne Funktion), der prüft, ob alle Koordinaten des Schiffs den Wert `"X"` haben.  
Gib `"Versenkt!"` oder `"Noch nicht versenkt."` aus.

---

### Aufgabe 6.5 – Flotte auswerten
Gegeben:
```python
flotte = [
    [(0, 0), (0, 1), (0, 2), (0, 3)],
    [(3, 5), (4, 5), (5, 5)],
    [(7, 7), (7, 8)],
]
```
Schreibe Code, der:
- Die Anzahl der Schiffe ausgibt.
- Die Gesamtzahl der Schiff-Felder berechnet und ausgibt.
- Alle Koordinaten zeilenweise ausgibt.

---

### Aufgabe 6.6 – Koordinaten-Tupel entpacken
Gegeben:
```python
koordinaten = [(1, 2), (3, 4), (5, 6), (7, 8)]
```
Durchlaufe die Liste mit einer `for`-Schleife und gib jede Koordinate aus als:  
`"Zeile 1, Spalte 2"` (mit Tupel-Entpacken).

---

> Musterlösungen zu allen Aufgaben: [Kapitel 10 – Lösungen](10_loesungen.md)
