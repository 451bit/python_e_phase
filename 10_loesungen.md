# Kapitel 10 – Musterlösungen

> Die Lösungen stehen hier bewusst getrennt von den Aufgaben, damit du nicht versehentlich hineinschaust.  
> Versuche jede Aufgabe **zuerst selbst**, bevor du nachschaust!

---

## Lösungen zu Kapitel 1 – Variablen und Datentypen

### Lösung 1.1 – Spieler anlegen

```python
name        = input("Dein Name: ")
versuche    = 0
trefferquote = 0.0
gewonnen    = False

print("Name:", name)
print("Versuche:", versuche)
print("Trefferquote:", trefferquote)
print("Gewonnen:", gewonnen)
```

---

### Lösung 1.2 – Koordinateneingabe

```python
zeile  = int(input("Zeile  (0-9): "))
spalte = int(input("Spalte (0-9): "))

print(f"Du zielst auf Zeile {zeile}, Spalte {spalte}.")
```

---

### Lösung 1.3 – Typkonvertierung verstehen

```python
a = "5"
b = "3"

print(a + b)             # "53"  – String-Konkatenation
print(int(a) + int(b))   # 8     – Integer-Addition
```

**Erklärung:** `+` bei Strings fügt sie aneinander. Erst durch `int()` werden echte Zahlen addiert.

---

### Lösung 1.4 – Feldgröße berechnen

```python
n = int(input("Feldgröße n: "))

felder_gesamt = n * n

print(f"Spielfeld: {n} x {n}")
print(f"Felder gesamt: {felder_gesamt}")
```

---

## Lösungen zu Kapitel 2 – Operatoren und logische Ausdrücke

### Lösung 2.1 – Gültigkeitsprüfung

```python
zeile  = int(input("Zeile  (0-9): "))
spalte = int(input("Spalte (0-9): "))

gueltig = 0 <= zeile <= 9 and 0 <= spalte <= 9
print("Gültige Koordinate:", gueltig)
```

---

### Lösung 2.2 – Treffer-Prüfung

```python
schiff_zeile  = 3
schiff_spalte = 5

zeile  = int(input("Zeile : "))
spalte = int(input("Spalte: "))

treffer = (zeile == schiff_zeile and spalte == schiff_spalte)
print("Treffer:", treffer)
if treffer:
    print("Treffer!")
else:
    print("Wasser!")
```

---

### Lösung 2.3 – Punkteberechnung

```python
treffer    = int(input("Treffer: "))
fehlschuesse = int(input("Fehlschüsse: "))

punkte = treffer * 10 - fehlschuesse * 3
print(f"Punktestand: {punkte}")
```

---

### Lösung 2.4 – Spielende-Bedingung

```python
versenkter_schiffe = int(input("Versenkter Schiffe: "))
gesamt_schiffe     = int(input("Schiffe gesamt: "))
versuche_uebrig    = int(input("Versuche übrig: "))

spiel_vorbei = (versenkter_schiffe == gesamt_schiffe) or (versuche_uebrig == 0)
print("Spiel vorbei:", spiel_vorbei)
```

---

### Lösung 2.5 – Wahrheitstabelle

```python
# XOR – exklusives Oder
for A in [True, False]:
    for B in [True, False]:
        ergebnis = (A and not B) or (not A and B)
        print(f"A={A}, B={B} → {ergebnis}")
```

Ausgabe:
```
A=True,  B=True  → False
A=True,  B=False → True
A=False, B=True  → True
A=False, B=False → False
```

Das ist das **XOR (Exklusiv-Oder)**: genau dann wahr, wenn genau einer der beiden Werte wahr ist.

---

## Lösungen zu Kapitel 3 – Verzweigungen

### Lösung 3.1 – Gültigkeitsprüfung mit Meldung

```python
zeile  = int(input("Zeile  (0-9): "))
spalte = int(input("Spalte (0-9): "))

if 0 <= zeile <= 9 and 0 <= spalte <= 9:
    print(f"Koordinate gültig: ({zeile}, {spalte})")
else:
    print("Fehler: Koordinate liegt außerhalb des Spielfelds!")
```

---

### Lösung 3.2 – Treffer auswerten

```python
schiff = [(2, 3), (2, 4), (2, 5)]

zeile  = int(input("Zeile  (0-9): "))
spalte = int(input("Spalte (0-9): "))

if not (0 <= zeile <= 9 and 0 <= spalte <= 9):
    print("Ungültige Eingabe!")
elif (zeile, spalte) in schiff:
    print("Treffer!")
else:
    print("Wasser!")
```

---

### Lösung 3.3 – Mehrfachverzweigung: Spielstatus

```python
versenkter_schiffe = int(input("Versenkte Schiffe (0-3): "))

if versenkter_schiffe == 0:
    print("Spiel läuft – noch kein Treffer.")
elif versenkter_schiffe == 1:
    print("1 Schiff versenkt!")
elif versenkter_schiffe == 2:
    print("2 Schiffe versenkt – fast gewonnen!")
elif versenkter_schiffe == 3:
    print("Alle Schiffe versenkt – du hast gewonnen!")
else:
    print("Ungültiger Spielstand.")
```

---

### Lösung 3.4 – Wer beginnt?

```python
wuerfel1 = int(input("Würfel Spieler 1 (1-6): "))
wuerfel2 = int(input("Würfel Spieler 2 (1-6): "))

if wuerfel1 > wuerfel2:
    print("Spieler 1 beginnt.")
elif wuerfel2 > wuerfel1:
    print("Spieler 2 beginnt.")
else:
    print("Unentschieden – nochmals würfeln.")
```

---

### Lösung 3.5 – Schiff versenkt?

```python
feld1_getroffen = bool(int(input("Feld 1 getroffen? (0/1): ")))
feld2_getroffen = bool(int(input("Feld 2 getroffen? (0/1): ")))
feld3_getroffen = bool(int(input("Feld 3 getroffen? (0/1): ")))

if feld1_getroffen and feld2_getroffen and feld3_getroffen:
    print("Schiff versenkt!")
else:
    print("Schiff noch nicht versenkt.")
```

---

## Lösungen zu Kapitel 4 – Schleifen

### Lösung 4.1 – Spielfeld ausgeben

```python
GROESSE = 10

print("   ", end="")
for s in range(GROESSE):
    print(f"{s:2}", end="")
print()

for z in range(GROESSE):
    print(f"{z:2} ", end="")
    for s in range(GROESSE):
        print(" ~", end="")
    print()
```

---

### Lösung 4.2 – Countdown

```python
versuche = 20

while versuche > 0:
    if versuche == 10:
        print("Halbzeit!")
    print(versuche)
    versuche -= 1

print("Keine Versuche mehr!")
```

---

### Lösung 4.3 – Alle Koordinaten ausgeben

```python
schiff = [(1, 0), (1, 1), (1, 2), (1, 3)]

for zeile, spalte in schiff:
    print(f"Feld ({zeile}, {spalte}): Schiff")
```

---

### Lösung 4.4 – range() erkunden

```python
# a)
for i in range(10):
    print(i, end=" ")
print()

# b)
for i in range(1, 11):
    print(i, end=" ")
print()

# c)
for i in range(0, 10, 2):
    print(i, end=" ")
print()

# d)
for i in range(9, -1, -1):
    print(i, end=" ")
print()
```

---

### Lösung 4.5 – Schiff-Koordinaten prüfen

```python
flotte = [
    [(0, 0), (0, 1), (0, 2)],
    [(5, 3), (6, 3), (7, 3)],
    [(8, 8), (8, 9)],
]

zeile  = int(input("Zeile : "))
spalte = int(input("Spalte: "))

gefunden = False
for schiff in flotte:
    for z, s in schiff:
        if z == zeile and s == spalte:
            gefunden = True

if gefunden:
    print("Treffer!")
else:
    print("Wasser!")
```

---

### Lösung 4.6 – Spielrunden-Simulation

```python
SCHIFF = (4, 5)
versuche = 10
gewonnen = False

while versuche > 0:
    zeile  = int(input("Zeile : "))
    spalte = int(input("Spalte: "))
    versuche -= 1

    if (zeile, spalte) == SCHIFF:
        print("Treffer – gewonnen!")
        gewonnen = True
        break
    else:
        print(f"Wasser! Noch {versuche} Versuche.")

if not gewonnen:
    print("Verloren!")
```

---

## Lösungen zu Kapitel 5 – String-Operationen

### Lösung 5.1 – Willkommensbanner

```python
name = input("Dein Name: ")
text1 = f"  Hallo, {name}!"
text2 = "  Schiffe versenken"
breite = max(len(text1), len(text2)) + 2
linie  = "=" * breite

print(linie)
print(text1)
print(text2)
print(linie)
```

---

### Lösung 5.2 – Koordinateneingabe "A5"

```python
eingabe = input("Koordinate (z.B. A5): ").strip().upper()

buchstabe = eingabe[0]
zahl      = int(eingabe[1:])

spalte = ord(buchstabe) - ord("A")
zeile  = zahl - 1

print(f"Zeile: {zeile}, Spalte: {spalte}")
```

---

### Lösung 5.3 – Spielfeldreihe formatieren

```python
reihe      = ["~", "S", "~", "X", "~", "~", "O", "~", "S", "~"]
zeilennr   = 3
zeile_str  = " ".join(reihe)
print(f"{zeilennr} | {zeile_str}")
```

---

### Lösung 5.4 – Zelle ermitteln

```python
eingabe = input("Koordinate (z.B. C4): ").strip().upper()

if (len(eingabe) >= 2
        and eingabe[0].isalpha()
        and "A" <= eingabe[0] <= "J"
        and eingabe[1:].isdigit()
        and 1 <= int(eingabe[1:]) <= 10):
    spalte = ord(eingabe[0]) - ord("A")
    zeile  = int(eingabe[1:]) - 1
    print(f"Zeile: {zeile}, Spalte: {spalte}")
else:
    print("Ungültige Eingabe!")
```

---

### Lösung 5.5 – String-Methoden erkunden

```python
eingabe = "  b5  "
print(repr(eingabe))               # "  b5  "

schritt1 = eingabe.strip()
print(repr(schritt1))              # "b5"

schritt2 = schritt1.upper()
print(repr(schritt2))              # "B5"

erster_buchstabe = schritt2[0]
print(repr(erster_buchstabe))      # "B"

rest = schritt2[1:]
print(repr(rest))                  # "5"

print(rest.isdigit())              # True
```

---

## Lösungen zu Kapitel 6 – Listen und Tupel

### Lösung 6.1 – Spielfeld initialisieren

```python
GROESSE = 10
feld = [["~"] * GROESSE for _ in range(GROESSE)]

print(len(feld))       # 10
print(len(feld[0]))    # 10
```

---

### Lösung 6.2 – Schiff eintragen

```python
GROESSE = 10
feld = [["~"] * GROESSE for _ in range(GROESSE)]

schiff1 = [(0, 0), (0, 1), (0, 2)]
schiff2 = [(5, 3), (6, 3), (7, 3), (8, 3)]

for z, s in schiff1:
    feld[z][s] = "S"
for z, s in schiff2:
    feld[z][s] = "S"

for zeile in feld:
    print(" ".join(zeile))
```

---

### Lösung 6.3 – Treffer markieren

```python
GROESSE = 10
feld = [["~"] * GROESSE for _ in range(GROESSE)]
feld[3][4] = "S"

zeile  = int(input("Zeile : "))
spalte = int(input("Spalte: "))

if feld[zeile][spalte] == "S":
    feld[zeile][spalte] = "X"
    print("Treffer!")
else:
    feld[zeile][spalte] = "O"
    print("Wasser!")
```

---

### Lösung 6.4 – Schiff versenkt?

```python
GROESSE = 10
feld = [["~"] * GROESSE for _ in range(GROESSE)]

schiff = [(2, 3), (2, 4), (2, 5)]
# Alle Felder als Treffer markieren (Simulation)
for z, s in schiff:
    feld[z][s] = "X"

alle_getroffen = True
for z, s in schiff:
    if feld[z][s] != "X":
        alle_getroffen = False

if alle_getroffen:
    print("Versenkt!")
else:
    print("Noch nicht versenkt.")
```

---

### Lösung 6.5 – Flotte auswerten

```python
flotte = [
    [(0, 0), (0, 1), (0, 2), (0, 3)],
    [(3, 5), (4, 5), (5, 5)],
    [(7, 7), (7, 8)],
]

print("Anzahl Schiffe:", len(flotte))

gesamt = 0
for schiff in flotte:
    gesamt += len(schiff)
print("Schiff-Felder gesamt:", gesamt)

for i, schiff in enumerate(flotte):
    print(f"Schiff {i + 1}:")
    for z, s in schiff:
        print(f"  ({z}, {s})")
```

---

### Lösung 6.6 – Koordinaten-Tupel entpacken

```python
koordinaten = [(1, 2), (3, 4), (5, 6), (7, 8)]

for zeile, spalte in koordinaten:
    print(f"Zeile {zeile}, Spalte {spalte}")
```

---

## Lösungen zu Kapitel 7 – Funktionen

### Lösung 7.1 – Funktion: Spielfeld erstellen

```python
def feld_erstellen(groesse):
    return [["~"] * groesse for _ in range(groesse)]

feld = feld_erstellen(10)
print(len(feld), len(feld[0]))   # 10 10
```

---

### Lösung 7.2 – Funktion: Koordinaten einlesen

```python
def koordinate_einlesen():
    zeile  = int(input("Zeile  (0-9): "))
    spalte = int(input("Spalte (0-9): "))
    return (zeile, spalte)

z, s = koordinate_einlesen()
print(f"Eingelesen: ({z}, {s})")
```

---

### Lösung 7.3 – Funktion: Gültigkeit prüfen

```python
def koordinate_gueltig(zeile, spalte, groesse):
    return 0 <= zeile < groesse and 0 <= spalte < groesse

print(koordinate_gueltig(3, 7, 10))    # True
print(koordinate_gueltig(10, 0, 10))   # False
print(koordinate_gueltig(-1, 5, 10))   # False
```

---

### Lösung 7.4 – Funktion: Schuss auswerten

```python
def schuss_ausfuehren(feld, zeile, spalte):
    if feld[zeile][spalte] == "S":
        feld[zeile][spalte] = "X"
        return True
    elif feld[zeile][spalte] == "~":
        feld[zeile][spalte] = "O"
        return False
    else:
        return False   # bereits beschossen
```

---

### Lösung 7.5 – Funktion: Spielfeld anzeigen (mit Verstecken)

```python
def zeige_spielfeld(feld, groesse, verstecke_schiffe=False):
    print("   ", end="")
    for s in range(groesse):
        print(f"{s:2}", end="")
    print()
    for z in range(groesse):
        print(f"{z:2} ", end="")
        for s in range(groesse):
            zelle = feld[z][s]
            if verstecke_schiffe and zelle == "S":
                zelle = "~"
            print(f" {zelle}", end="")
        print()
```

---

### Lösung 7.6 – Funktion: Schiff versenkt?

```python
def ist_schiff_versenkt(schiff, feld):
    for zeile, spalte in schiff:
        if feld[zeile][spalte] != "X":
            return False
    return True
```

---

## Lösungen zu Kapitel 8 – Spezielle Python-Methoden

### Lösung 8.1 – `map()` für Koordinateneingabe

```python
zeile, spalte = map(int, input("Zeile Spalte (z.B. '3 7'): ").split())
print(f"Ziel: ({zeile}, {spalte})")
```

---

### Lösung 8.2 – `enumerate()` für Spielfeldausgabe

```python
def zeige_feld(feld):
    groesse = len(feld)
    print("   ", end="")
    for s in range(groesse):
        print(f"{s:2}", end="")
    print()
    for zeilennr, zeile in enumerate(feld):
        print(f"{zeilennr:2} ", end="")
        for zelle in zeile:
            print(f" {zelle}", end="")
        print()
```

---

### Lösung 8.3 – `zip()` für Spieler-Statistik

```python
spieler  = ["Anna", "Ben", "Clara"]
treffer  = [7, 5, 9]
schuesse = [15, 12, 14]

for name, t, s in zip(spieler, treffer, schuesse):
    quote = t / s * 100
    print(f"{name}: {t}/{s} Treffer = {quote:.1f}%")
```

---

### Lösung 8.4 – `filter()` für aktive Schiffe

```python
groessen = [4, 3, 3, 2, 2]
versenkt = [True, False, True, False, False]

aktive_groessen = list(filter(lambda g: not versenkt[groessen.index(g)],
                               groessen))

# Sicherer mit zip:
aktive = [g for g, v in zip(groessen, versenkt) if not v]
print("Aktive Schiffsgrößen:", aktive)
```

---

### Lösung 8.5 – List Comprehension: Schiff erzeugen

```python
def schiff_erstellen(start_zeile, start_spalte, laenge, horizontal):
    if horizontal:
        return [(start_zeile, start_spalte + i) for i in range(laenge)]
    else:
        return [(start_zeile + i, start_spalte) for i in range(laenge)]

print(schiff_erstellen(2, 3, 4, True))    # [(2,3),(2,4),(2,5),(2,6)]
print(schiff_erstellen(1, 5, 3, False))   # [(1,5),(2,5),(3,5)]
```

---

### Lösung 8.6 – `map()` mit eigener Funktion

```python
def zelle_fuer_gegner(zelle):
    if zelle == "S":
        return "~"
    return zelle

reihe = ["~", "S", "X", "~", "O", "S", "~", "~", "S", "~"]
gegner_reihe = list(map(zelle_fuer_gegner, reihe))
print(gegner_reihe)
# ['~', '~', 'X', '~', 'O', '~', '~', '~', '~', '~']
```

---

## Lösungen zu Kapitel 9 – Projektaufgaben

### Lösung 9.1 – Intelligenter Computer

```python
def computer_schuss_intelligent(feld_spieler, bereits_geschossen, letzte_treffer):
    """
    Verbesserte Computer-KI:
    Hat der Computer zuletzt getroffen, versucht er benachbarte Felder.
    """
    GROESSE = len(feld_spieler)

    if letzte_treffer:
        lz, ls = letzte_treffer[-1]
        nachbarn = [(lz - 1, ls), (lz + 1, ls), (lz, ls - 1), (lz, ls + 1)]
        for nz, ns in nachbarn:
            if (0 <= nz < GROESSE and 0 <= ns < GROESSE
                    and (nz, ns) not in bereits_geschossen):
                bereits_geschossen.append((nz, ns))
                return nz, ns

    # Kein Treffer-Folgeschuss möglich → zufällig
    import random
    while True:
        z = random.randint(0, GROESSE - 1)
        s = random.randint(0, GROESSE - 1)
        if (z, s) not in bereits_geschossen:
            bereits_geschossen.append((z, s))
            return z, s
```

---

### Lösung 9.2 – Statistik nach jeder Runde

```python
def zeige_spielstand(flotte_spieler, feld_spieler, flotte_computer, feld_computer):
    def zaehle_versenkte(flotte, feld):
        count = 0
        for schiff in flotte:
            alle_x = all(feld[z][s] == "X" for z, s in schiff)
            if alle_x:
                count += 1
        return count

    vs = zaehle_versenkte(flotte_spieler, feld_spieler)
    vc = zaehle_versenkte(flotte_computer, feld_computer)
    print(f"Versenkt – Spieler: {vs}/{len(flotte_spieler)}  "
          f"Computer: {vc}/{len(flotte_computer)}")
```

---

### Lösung 9.3 – Wiederholung

```python
# Am Ende von main():
nochmal = input("Nochmal spielen? (j/n): ").strip().lower()
if nochmal == "j":
    main()
```

---

### Lösung 9.4 – Spielfeld als String

```python
def feld_als_string(feld, verstecke_schiffe=False):
    groesse = len(feld)
    zeilen  = []
    kopf    = "    " + " ".join(str(s) for s in range(groesse))
    zeilen.append(kopf)
    for z, zeile in enumerate(feld):
        anzeige = []
        for zelle in zeile:
            if verstecke_schiffe and zelle == "S":
                anzeige.append("~")
            else:
                anzeige.append(zelle)
        zeilen.append(f"{z:2}  " + " ".join(anzeige))
    return "\n".join(zeilen)

# Verwendung
feld = [["~"] * 10 for _ in range(10)]
feld[2][3] = "S"
print(feld_als_string(feld))
print(feld_als_string(feld, verstecke_schiffe=True))
```
