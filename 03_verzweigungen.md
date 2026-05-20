# Kapitel 3 – Verzweigungen

> **Kontext:** Je nach Ergebnis eines Schusses (Treffer, Wasser, bereits beschossen, versenkt) muss das Spiel unterschiedlich reagieren.

---

## 3.1 Einfache Verzweigung – `if`

```python
if Bedingung:
    Anweisung(en)
```

Der eingerückte Block wird **nur dann ausgeführt**, wenn die Bedingung `True` ist.

```python
zeile = int(input("Zeile (0-9): "))

if zeile < 0 or zeile > 9:
    print("Ungültige Eingabe!")
```

> **Wichtig:** Python verwendet **Einrückung** (4 Leerzeichen) statt geschweifte Klammern.  
> Der Doppelpunkt `:` nach der Bedingung darf nicht fehlen.

---

## 3.2 Zweiseitige Verzweigung – `if / else`

```python
if Bedingung:
    # wird ausgeführt, wenn Bedingung wahr
else:
    # wird ausgeführt, wenn Bedingung falsch
```

```python
schiff_koordinaten = [(2, 3), (2, 4), (2, 5)]

zeile  = int(input("Zeile  (0-9): "))
spalte = int(input("Spalte (0-9): "))

if (zeile, spalte) in schiff_koordinaten:
    print("Treffer!")
else:
    print("Wasser!")
```

---

## 3.3 Mehrfachverzweigung – `if / elif / else`

Wenn mehr als zwei Fälle unterschieden werden müssen:

```python
if Bedingung1:
    # Fall 1
elif Bedingung2:
    # Fall 2
elif Bedingung3:
    # Fall 3
else:
    # alle anderen Fälle
```

Python prüft die Bedingungen **von oben nach unten** und führt nur den **ersten** zutreffenden Block aus.

### Beispiel: Spielfeld-Zustand

```python
# Zustand einer Zelle
WASSER      = "~"
SCHIFF      = "S"
TREFFER     = "X"
FEHLSCHUSS  = "O"

zelle = input("Zustand der Zelle: ")

if zelle == TREFFER:
    print("Diese Koordinate wurde bereits getroffen.")
elif zelle == FEHLSCHUSS:
    print("Hier hast du schon geschossen – Wasser.")
elif zelle == SCHIFF:
    print("Treffer! Ein Schiff wurde getroffen.")
elif zelle == WASSER:
    print("Wasser. Kein Schiff hier.")
else:
    print("Unbekannter Zustand.")
```

---

## 3.4 Verschachtelte Verzweigungen

Verzweigungen können ineinander geschachtelt werden:

```python
zeile  = int(input("Zeile  (0-9): "))
spalte = int(input("Spalte (0-9): "))

if 0 <= zeile <= 9 and 0 <= spalte <= 9:
    # Koordinate gültig – jetzt prüfen was dort ist
    if zelle == SCHIFF:
        print("Treffer!")
    else:
        print("Wasser!")
else:
    print("Koordinate liegt außerhalb des Spielfelds!")
```

> Tipp: Starke Verschachtelung (mehr als 2–3 Ebenen) macht Code schwer lesbar.  
> Lieber Bedingungen kombinieren oder in Funktionen auslagern (→ Kapitel 7).

---

## 3.5 Vollständiges Beispiel: Ein Schuss auswerten

```python
# Konstanten für Zellzustände
WASSER     = "~"
SCHIFF     = "S"
TREFFER    = "X"
FEHLSCHUSS = "O"

# Simulierter Zustand einer Zelle (normalerweise aus dem Spielfeld)
zelle = SCHIFF

# Schuss auswerten
if zelle == TREFFER or zelle == FEHLSCHUSS:
    print("Du hast diese Koordinate bereits beschossen!")
elif zelle == SCHIFF:
    print("Treffer! Das Schiff wurde getroffen.")
    zelle = TREFFER
else:
    print("Wasser! Kein Schiff an dieser Stelle.")
    zelle = FEHLSCHUSS

print("Neuer Zustand der Zelle:", zelle)
```

---

## 3.6 Besonderheit: Vergleich mit `in`

Python bietet das Schlüsselwort `in`, um zu prüfen, ob ein Element in einer Sequenz enthalten ist:

```python
schiff = [(2, 3), (2, 4), (2, 5)]

if (2, 4) in schiff:
    print("Koordinate gehört zum Schiff.")
```

```python
gueltige_zeichen = ["X", "O", "~", "S"]
if zelle in gueltige_zeichen:
    print("Gültiger Zustand.")
```

---

## Übungsaufgaben

### Aufgabe 3.1 – Gültigkeitsprüfung mit Meldung
Schreibe ein Programm, das Zeile und Spalte einliest.  
- Ist die Koordinate ungültig (außerhalb 0–9): Gib eine Fehlermeldung aus.  
- Ist sie gültig: Gib `"Koordinate gültig: (zeile, spalte)"` aus.

---

### Aufgabe 3.2 – Treffer auswerten
Simuliere eine Spielsituation:  
- Lege eine Liste mit 3 Schiffkoordinaten fest (Tupel).  
- Lies Zeile und Spalte ein.  
- Gib je nach Ergebnis aus: `"Treffer!"`, `"Wasser!"` oder `"Ungültige Eingabe!"`.

---

### Aufgabe 3.3 – Mehrfachverzweigung: Spielstatus
Schreibe ein Programm, das `versenkten_schiffe` (int) einliest und folgenden Status ausgibt:
- 0 Schiffe versenkt → `"Spiel läuft – noch kein Treffer."`
- 1 Schiff versenkt → `"1 Schiff versenkt!"`
- 2 Schiffe versenkt → `"2 Schiffe versenkt – fast gewonnen!"`
- 3 Schiffe versenkt → `"Alle Schiffe versenkt – du hast gewonnen!"`
- jede andere Zahl → `"Ungültiger Spielstand."`

---

### Aufgabe 3.4 – Wer beginnt?
Zu Spielbeginn würfeln beide Spieler. Schreibe ein Programm, das zwei Würfelwerte einliest und ausgibt:
- Wer beginnt (wer die höhere Zahl hat).
- Bei Gleichstand: `"Unentschieden – nochmals würfeln."`

---

### Aufgabe 3.5 – Schiff versenkt?
Ein Schiff hat 3 Felder. Lege drei bool-Variablen an: `feld1_getroffen`, `feld2_getroffen`, `feld3_getroffen`.  
Lies für jedes Feld `0` (nicht getroffen) oder `1` (getroffen) ein und konvertiere in `bool`.  
Prüfe: Ist das Schiff versenkt (alle drei Felder getroffen)?  
Gib eine entsprechende Meldung aus.

---

> Musterlösungen zu allen Aufgaben: [Kapitel 10 – Lösungen](10_loesungen.md)
