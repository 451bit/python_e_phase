# Kapitel 5 – String-Operationen

> **Kontext:** Spielernamen formatieren, das Spielfeld als Text ausgeben, Koordinateneingaben wie `"A5"` oder `"35"` verarbeiten – all das erfordert String-Operationen.

---

## 5.1 Strings erstellen

```python
name      = "Anna"
zustand   = 'Treffer!'
mehrzeile = """Willkommen
bei Schiffe versenken!"""
```

Einfache und doppelte Anführungszeichen sind gleichwertig.

---

## 5.2 Konkatenation und Wiederholung

```python
trennlinie = "-" * 20          # "--------------------"
ausgabe    = "Zeile: " + "3"   # "Zeile: 3"
```

> **Achtung:** `+` funktioniert nur bei zwei Strings. Zahlen müssen zuerst konvertiert werden:
> ```python
> zeile = 3
> ausgabe = "Zeile: " + str(zeile)   # OK
> ```

---

## 5.3 f-Strings (formatierte Zeichenketten)

f-Strings sind die modernste und lesbarste Art, Werte in Strings einzubetten:

```python
name   = "Anna"
punkte = 42
print(f"Spieler {name} hat {punkte} Punkte.")
# → Spieler Anna hat 42 Punkte.
```

In den geschwungenen Klammern kann **beliebiger Python-Ausdruck** stehen:

```python
zeile  = 3
spalte = 7
print(f"Schuss auf ({zeile}, {spalte})")

feld_groesse = 10
print(f"Spielfeld: {feld_groesse} x {feld_groesse} = {feld_groesse ** 2} Felder")
```

### Formatierung von Zahlen

```python
trefferquote = 0.6667
print(f"Trefferquote: {trefferquote:.1%}")   # 66.7%
print(f"Trefferquote: {trefferquote:.2f}")   # 0.67

zahl = 7
print(f"{zahl:2d}")    # " 7"  – rechtsbündig, 2 Stellen
```

---

## 5.4 Länge und Indexzugriff

```python
name = "Schiff"
print(len(name))      # 6

print(name[0])        # "S"  – erstes Zeichen
print(name[5])        # "f"  – letztes Zeichen
print(name[-1])       # "f"  – letztes (von hinten)
```

> Strings sind **unveränderlich** (immutable). `name[0] = "X"` führt zu einem Fehler.

---

## 5.5 Slicing

```python
text = "Spielfeld"
#       0123456789

print(text[0:5])      # "Spiel"    – Index 0 bis 4
print(text[5:])       # "feld"     – ab Index 5
print(text[:5])       # "Spiel"    – bis Index 4
print(text[::2])      # "Seled"    – jedes zweite Zeichen
print(text[::-1])     # "dlefleiS" – rückwärts
```

---

## 5.6 Wichtige String-Methoden

| Methode | Wirkung | Beispiel |
|---------|---------|---------|
| `upper()` | Großbuchstaben | `"hallo".upper()` → `"HALLO"` |
| `lower()` | Kleinbuchstaben | `"HALLO".lower()` → `"hallo"` |
| `strip()` | Leerzeichen entfernen | `"  hi  ".strip()` → `"hi"` |
| `lstrip()` | Links entfernen | `"  hi  ".lstrip()` → `"hi  "` |
| `rstrip()` | Rechts entfernen | `"  hi  ".rstrip()` → `"  hi"` |
| `replace(a, b)` | Ersetzen | `"~".replace("~", "X")` → `"X"` |
| `split(trennz)` | Aufteilen | `"3 7".split()` → `["3", "7"]` |
| `find(teil)` | Position finden | `"Hallo".find("ll")` → `2` |
| `startswith(s)` | Anfang prüfen | `"A5".startswith("A")` → `True` |
| `isdigit()` | Nur Ziffern? | `"35".isdigit()` → `True` |

---

## 5.7 Beispiele im Spielkontext

### Spielfeldzeile als String zusammenbauen

```python
FELD_GROESSE = 5
zeile_daten  = ["~", "~", "S", "~", "~"]

# Variante 1: Konkatenation in Schleife
zeile_str = ""
for zelle in zeile_daten:
    zeile_str += zelle + " "
print(zeile_str)   # "~ ~ S ~ ~ "

# Variante 2: join (eleganter)
zeile_str = " ".join(zeile_daten)
print(zeile_str)   # "~ ~ S ~ ~"
```

### Koordinateneingabe "A5" verarbeiten

Im Schiffe-versenken-Klassiker gibt man die Spalte als Buchstabe (A–J) und die Zeile als Zahl an:

```python
eingabe = input("Koordinate (z.B. A5): ").strip().upper()

buchstabe = eingabe[0]                      # "A"
zahl      = int(eingabe[1:])                # 5

spalte = ord(buchstabe) - ord("A")         # 0
zeile  = zahl - 1                           # 4 (0-basiert)

print(f"Zeile: {zeile}, Spalte: {spalte}")
```

> `ord()` gibt den ASCII-Wert eines Zeichens zurück: `ord("A")` = 65, `ord("B")` = 66, …

### Eingabe validieren

```python
eingabe = input("Koordinate (z.B. A5): ").strip().upper()

if len(eingabe) >= 2 and eingabe[0].isalpha() and eingabe[1:].isdigit():
    print("Gültige Eingabe")
else:
    print("Ungültige Eingabe!")
```

---

## 5.8 `join()` und `split()`

```python
# split: String → Liste
zeile_eingabe = "3 7"
teile = zeile_eingabe.split()    # ["3", "7"]
zeile  = int(teile[0])           # 3
spalte = int(teile[1])           # 7

# join: Liste → String
felder = ["~", "S", "X", "~", "O"]
ausgabe = " ".join(felder)       # "~ S X ~ O"
print(ausgabe)
```

---

## Übungsaufgaben

### Aufgabe 5.1 – Willkommensbanner
Schreibe ein Programm, das den Namen des Spielers einliest und einen formatierten Willkommenstext ausgibt:
```
====================
  Hallo, Anna!
  Schiffe versenken
====================
```
Die Breite der Linie soll sich nach dem längsten Text richten.

---

### Aufgabe 5.2 – Koordinateneingabe "A5"
Schreibe ein Programm, das eine Eingabe im Format `"B7"` verarbeitet und Zeile und Spalte (0-basiert) als Integer ausgibt.  
`A` → Spalte 0, `B` → Spalte 1, …, `J` → Spalte 9.  
Zeile: Ziffer – 1 (also `"1"` → 0, `"10"` → 9).

---

### Aufgabe 5.3 – Spielfeldreihe formatieren
Gegeben:
```python
reihe = ["~", "S", "~", "X", "~", "~", "O", "~", "S", "~"]
```
Erstelle daraus den String `"~ S ~ X ~ ~ O ~ S ~"` mit `join()`.  
Gib ihn zusammen mit der Zeilennummer aus, z. B. `"3 | ~ S ~ X ~ ~ O ~ S ~"`.

---

### Aufgabe 5.4 – Zelle ermitteln
Schreibe ein Programm, das eine Koordinate `"C4"` einliest.  
Prüfe zuerst die Gültigkeit (erster Buchstabe A–J, Zahl 1–10).  
Berechne dann Zeile und Spalte und gib sie aus.

---

### Aufgabe 5.5 – String-Methoden erkunden
Gegeben:
```python
eingabe = "  b5  "
```
Wende nacheinander an: `strip()`, `upper()`, `[0]`, `[1:]`, `isdigit()`.  
Gib nach jedem Schritt das Ergebnis aus und erkläre, was passiert.

---

> Musterlösungen zu allen Aufgaben: [Kapitel 10 – Lösungen](10_loesungen.md)
