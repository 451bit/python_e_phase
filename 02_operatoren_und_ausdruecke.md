# Kapitel 2 – Operatoren und logische Ausdrücke

> **Kontext:** Um zu prüfen, ob eine Koordinate gültig ist, ein Schiff getroffen wurde oder das Spiel vorbei ist, brauchen wir Vergleiche und logische Verknüpfungen.

---

## 2.1 Arithmetische Operatoren

| Operator | Bedeutung | Beispiel | Ergebnis |
|----------|-----------|---------|---------|
| `+` | Addition | `3 + 4` | `7` |
| `-` | Subtraktion | `10 - 3` | `7` |
| `*` | Multiplikation | `4 * 5` | `20` |
| `/` | Division (float) | `7 / 2` | `3.5` |
| `//` | Ganzzahl-Division | `7 // 2` | `3` |
| `%` | Modulo (Rest) | `7 % 2` | `1` |
| `**` | Potenz | `2 ** 3` | `8` |

### Anwendung im Spielkontext

```python
feld_groesse = 10
felder_gesamt = feld_groesse * feld_groesse   # 100

schuss_nummer = 7
zeile  = schuss_nummer // feld_groesse        # 0
spalte = schuss_nummer %  feld_groesse        # 7
```

---

## 2.2 Vergleichsoperatoren

Vergleiche geben immer einen **Wahrheitswert** (`True` oder `False`) zurück.

| Operator | Bedeutung | Beispiel | Ergebnis |
|----------|-----------|---------|---------|
| `==` | gleich | `5 == 5` | `True` |
| `!=` | ungleich | `5 != 3` | `True` |
| `<` | kleiner | `3 < 5` | `True` |
| `>` | größer | `7 > 9` | `False` |
| `<=` | kleiner gleich | `5 <= 5` | `True` |
| `>=` | größer gleich | `6 >= 7` | `False` |

> **Achtung:** `=` ist Zuweisung, `==` ist Vergleich!

```python
zeile  = 3
spalte = 7

gueltig_zeile  = zeile  >= 0 and zeile  <= 9
gueltig_spalte = spalte >= 0 and spalte <= 9
```

---

## 2.3 Logische Operatoren

Logische Operatoren verknüpfen Wahrheitswerte.

### `and` – beide Bedingungen müssen wahr sein

| A | B | A and B |
|---|---|---------|
| True | True | **True** |
| True | False | False |
| False | True | False |
| False | False | False |

### `or` – mindestens eine Bedingung muss wahr sein

| A | B | A or B |
|---|---|--------|
| True | True | **True** |
| True | False | **True** |
| False | True | **True** |
| False | False | False |

### `not` – Negation

| A | not A |
|---|-------|
| True | False |
| False | True |

---

## 2.4 Zusammengesetzte Ausdrücke

```python
zeile  = int(input("Zeile  (0-9): "))
spalte = int(input("Spalte (0-9): "))

# Koordinate gültig?
koordinate_gueltig = (zeile >= 0 and zeile <= 9) and (spalte >= 0 and spalte <= 9)

# Alternativer Stil mit Python-eigener Kurzschreibweise
koordinate_gueltig = 0 <= zeile <= 9 and 0 <= spalte <= 9

print("Gültige Koordinate:", koordinate_gueltig)
```

> Python erlaubt **verkettete Vergleiche** wie `0 <= zeile <= 9` – das ist pythonisch und lesbar.

---

## 2.5 Wahrheitswert von Werten

In Python haben auch Nicht-`bool`-Werte einen Wahrheitswert:

| Wert | bool-Wert |
|------|-----------|
| `0` | `False` |
| jede andere Zahl | `True` |
| `""` (leerer String) | `False` |
| jeder nicht-leere String | `True` |

```python
verbleibende_schiffe = 3
print(bool(verbleibende_schiffe))   # True – noch Schiffe vorhanden
verbleibende_schiffe = 0
print(bool(verbleibende_schiffe))   # False – alle versenkt
```

---

## 2.6 Zuweisungsoperatoren (Kurzschreibweise)

| Operator | Bedeutung | Beispiel |
|----------|-----------|---------|
| `+=` | addieren und zuweisen | `punkte += 10` |
| `-=` | subtrahieren und zuweisen | `versuche -= 1` |
| `*=` | multiplizieren und zuweisen | `multiplikator *= 2` |
| `//=` | ganzzahlig dividieren und zuweisen | `wert //= 2` |

```python
punkte = 0
punkte += 10    # Treffer: 10 Punkte gutschreiben
punkte += 20    # noch ein Treffer
print(punkte)   # 30
```

---

## Übungsaufgaben

### Aufgabe 2.1 – Gültigkeitsprüfung
Schreibe ein Programm, das Zeile und Spalte einliest und prüft, ob beide im Bereich 0–9 liegen.  
Gib `True` oder `False` aus.

---

### Aufgabe 2.2 – Treffer-Prüfung
Ein Schiff liegt auf den Koordinaten `(3, 5)`. Der Spieler gibt eine Koordinate ein.  
Schreibe ein Programm, das mit einem Vergleich prüft, ob es ein Treffer ist.  
Gib `"Treffer!"` oder `"Wasser!"` aus (ohne if – nur mit `print` und dem bool-Ergebnis zunächst).

---

### Aufgabe 2.3 – Punkteberechnung
Der Spieler bekommt 10 Punkte pro Treffer, verliert aber 3 Punkte pro Fehlschuss.  
Schreibe ein Programm, das `treffer` und `fehlschuesse` einliest, den Punktestand berechnet und ausgibt.

---

### Aufgabe 2.4 – Spielende-Bedingung
Das Spiel endet, wenn alle Schiffe versenkt sind (`versenkte_schiffe == gesamt_schiffe`)  
**oder** der Spieler keine Versuche mehr hat (`versuche_uebrig == 0`).  
Schreibe einen logischen Ausdruck, der dies prüft, und teste ihn mit verschiedenen Werten.

---

### Aufgabe 2.5 – Wahrheitstabelle
Erstelle für folgende Bedingung eine Wahrheitstabelle (von Hand oder als Programm):  
`(A and not B) or (not A and B)` – für alle vier Kombinationen von A und B.  
Wie nennt man diese logische Operation?

---

> Musterlösungen zu allen Aufgaben: [Kapitel 10 – Lösungen](10_loesungen.md)
