# Kapitel 7 – Funktionen

> **Kontext:** Spielfeld anzeigen, Schuss auswerten, Schiff eintragen – jede dieser Teilaufgaben wird in eine eigene Funktion ausgelagert. So bleibt das Programm übersichtlich und wiederverwendbar.

---

## 7.1 Was ist eine Funktion?

Eine **Funktion** fasst eine Folge von Anweisungen unter einem Namen zusammen.  
Sie kann aufgerufen werden, (optionale) **Parameter** entgegennehmen und (optional) einen **Rückgabewert** liefern.

```python
def funktionsname(parameter1, parameter2):
    # Anweisungen
    return ergebnis
```

- `def` leitet die Definition ein
- Die Parameter stehen in runden Klammern
- Der Körper ist eingerückt
- `return` liefert einen Wert zurück (und beendet die Funktion)

---

## 7.2 Funktion ohne Parameter und ohne Rückgabewert

```python
def willkommen():
    print("=" * 30)
    print("  Schiffe versenken")
    print("=" * 30)

# Aufruf
willkommen()
```

Solche Funktionen werden für *Seiteneffekte* eingesetzt (z. B. Ausgabe).

---

## 7.3 Funktion mit Parametern, ohne Rückgabewert

```python
def zeige_spielfeld(feld, groesse):
    print("   ", end="")
    for s in range(groesse):
        print(f"{s:2}", end="")
    print()
    for z in range(groesse):
        print(f"{z:2} ", end="")
        for s in range(groesse):
            print(f" {feld[z][s]}", end="")
        print()

# Aufruf
GROESSE = 10
feld = [["~"] * GROESSE for _ in range(GROESSE)]
zeige_spielfeld(feld, GROESSE)
```

Parameter sind **lokale Variablen** innerhalb der Funktion.  
Änderungen an `groesse` innerhalb der Funktion betreffen die Variable außen nicht.

> **Listen werden als Referenz übergeben!**  
> Änderungen an `feld[z][s]` innerhalb der Funktion wirken sich auf das Original aus.

---

## 7.4 Funktion mit Rückgabewert

```python
def koordinate_gueltig(zeile, spalte, groesse):
    return 0 <= zeile < groesse and 0 <= spalte < groesse

# Aufruf
if koordinate_gueltig(3, 7, 10):
    print("Gültige Koordinate!")
else:
    print("Außerhalb des Spielfelds!")
```

```python
def zaehle_verbleibende_schiffe(flotte, feld):
    anzahl = 0
    for schiff in flotte:
        versenkt = True
        for zeile, spalte in schiff:
            if feld[zeile][spalte] != "X":
                versenkt = False
        if not versenkt:
            anzahl += 1
    return anzahl
```

---

## 7.5 Funktion mit mehreren Rückgabewerten

Python kann über ein **Tupel** mehrere Werte zurückgeben:

```python
def eingabe_einlesen():
    zeile  = int(input("Zeile  (0-9): "))
    spalte = int(input("Spalte (0-9): "))
    return zeile, spalte       # gibt Tupel zurück

# Aufruf mit Entpacken
z, s = eingabe_einlesen()
print(f"Schuss auf ({z}, {s})")
```

---

## 7.6 Standardparameter (Default-Werte)

```python
def zeige_spielfeld(feld, groesse=10):
    ...

# Aufruf ohne Argument – nimmt den Default 10
zeige_spielfeld(feld)

# Aufruf mit explizitem Argument
zeige_spielfeld(feld, 5)
```

---

## 7.7 Lokale vs. globale Variablen

```python
punkte = 0         # globale Variable

def treffer_werten():
    # Ohne global-Deklaration kann man globale Variablen lesen, aber nicht schreiben
    # Um zu schreiben, muss man 'global' deklarieren – das sollte man vermeiden
    global punkte
    punkte += 10

treffer_werten()
print(punkte)      # 10
```

> **Empfehlung:** Statt `global` zu verwenden, lieber den aktuellen Wert als Parameter übergeben und den neuen Wert zurückgeben:
>
> ```python
> def treffer_werten(punkte):
>     return punkte + 10
>
> punkte = 0
> punkte = treffer_werten(punkte)
> print(punkte)   # 10
> ```

---

## 7.8 Vollständiges Beispiel: Spiellogik in Funktionen

```python
# ──────────────────────────────────────────
# Hilfsfunktionen für Schiffe versenken
# ──────────────────────────────────────────

GROESSE = 10
WASSER     = "~"
SCHIFF     = "S"
TREFFER    = "X"
FEHLSCHUSS = "O"

def feld_erstellen(groesse):
    return [[WASSER] * groesse for _ in range(groesse)]

def schiff_eintragen(feld, schiff):
    for zeile, spalte in schiff:
        feld[zeile][spalte] = SCHIFF

def zeige_spielfeld(feld, groesse):
    print("   ", end="")
    for s in range(groesse):
        print(f"{s:2}", end="")
    print()
    for z in range(groesse):
        print(f"{z:2} ", end="")
        for s in range(groesse):
            print(f" {feld[z][s]}", end="")
        print()

def koordinate_gueltig(zeile, spalte, groesse):
    return 0 <= zeile < groesse and 0 <= spalte < groesse

def schuss_ausfuehren(feld, zeile, spalte):
    """Wertet einen Schuss aus. Gibt True bei Treffer, False bei Wasser zurück."""
    if feld[zeile][spalte] == SCHIFF:
        feld[zeile][spalte] = TREFFER
        return True
    elif feld[zeile][spalte] == WASSER:
        feld[zeile][spalte] = FEHLSCHUSS
        return False
    else:
        return False   # bereits beschossen

def ist_schiff_versenkt(schiff, feld):
    for zeile, spalte in schiff:
        if feld[zeile][spalte] != TREFFER:
            return False
    return True

def alle_schiffe_versenkt(flotte, feld):
    for schiff in flotte:
        if not ist_schiff_versenkt(schiff, feld):
            return False
    return True

# ──────────────────────────────────────────
# Hauptprogramm
# ──────────────────────────────────────────

feld   = feld_erstellen(GROESSE)
flotte = [[(2, 3), (2, 4), (2, 5)], [(7, 1), (7, 2)]]

for schiff in flotte:
    schiff_eintragen(feld, schiff)

zeige_spielfeld(feld, GROESSE)

z, s = 2, 4
treffer = schuss_ausfuehren(feld, z, s)
print("Treffer?" , treffer)

zeige_spielfeld(feld, GROESSE)
```

---

## Übungsaufgaben

### Aufgabe 7.1 – Funktion: Spielfeld erstellen
Schreibe eine Funktion `feld_erstellen(groesse)`, die ein `groesse × groesse`-Spielfeld (Liste von Listen) mit `"~"` erstellt und zurückgibt.  
Rufe sie mit `groesse=10` auf und gib das Ergebnis aus.

---

### Aufgabe 7.2 – Funktion: Koordinaten einlesen
Schreibe eine Funktion `koordinate_einlesen()`, die:
- nach Zeile und Spalte fragt,
- beide als `int` konvertiert,
- das Tupel `(zeile, spalte)` zurückgibt.

---

### Aufgabe 7.3 – Funktion: Gültigkeit prüfen
Schreibe eine Funktion `koordinate_gueltig(zeile, spalte, groesse)`.  
Sie soll `True` zurückgeben, wenn beide Koordinaten im Bereich `[0, groesse-1]` liegen, sonst `False`.

---

### Aufgabe 7.4 – Funktion: Schuss auswerten
Schreibe eine Funktion `schuss_ausfuehren(feld, zeile, spalte)`, die:
- `"X"` setzt, wenn `feld[zeile][spalte] == "S"` → `True` zurückgeben
- `"O"` setzt, wenn `feld[zeile][spalte] == "~"` → `False` zurückgeben
- nichts ändert, wenn bereits beschossen → `False` zurückgeben

---

### Aufgabe 7.5 – Funktion: Spielfeld anzeigen
Schreibe eine Funktion `zeige_spielfeld(feld, groesse, verstecke_schiffe)`.  
Wenn `verstecke_schiffe=True`, sollen `"S"`-Felder als `"~"` angezeigt werden (Gegnersicht).  
Default-Wert: `verstecke_schiffe=False`.

---

### Aufgabe 7.6 – Funktion: Schiff versenkt?
Schreibe eine Funktion `ist_schiff_versenkt(schiff, feld)`, die `True` zurückgibt, wenn alle Koordinaten des Schiffs im Feld den Wert `"X"` haben.

---

> Musterlösungen zu allen Aufgaben: [Kapitel 10 – Lösungen](10_loesungen.md)
