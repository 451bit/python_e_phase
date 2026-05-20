# Kapitel 1 – Variablen und Datentypen

> **Kontext:** Wir legen die Grundbausteine des Spiels an: Spielernamen, Spielfeldgröße, Punktestand und Koordinaten.

---

## 1.1 Variablen

Eine **Variable** ist ein benannter Speicherplatz für einen Wert.  
In Python muss kein Typ vorab deklariert werden – der Typ ergibt sich automatisch aus der Zuweisung.

```python
spieler_name = "Anna"       # Zeichenkette
punkte       = 0            # Ganzzahl
feld_groesse = 10           # Ganzzahl
trefferquote = 0.0          # Kommazahl
spiel_laeuft = True         # Wahrheitswert
```

### Regeln für Variablennamen

- Nur Buchstaben, Ziffern und Unterstrich `_`
- Kein Anfang mit einer Ziffer
- Groß-/Kleinschreibung wird unterschieden (`Punkte ≠ punkte`)
- Python-Schlüsselwörter sind verboten (`if`, `for`, `while`, …)

**Konvention:** `snake_case` – alles klein, Wörter durch `_` getrennt.

---

## 1.2 Datentypen

| Typ | Name | Beispiel |
|-----|------|---------|
| Ganzzahl | `int` | `10`, `-3`, `0` |
| Kommazahl | `float` | `3.14`, `-0.5` |
| Wahrheitswert | `bool` | `True`, `False` |
| Zeichenkette | `str` | `"Hallo"`, `'A5'` |

Den Typ einer Variable gibt `type()` zurück:

```python
x = 42
print(type(x))   # <class 'int'>
```

### Wahrheitswerte

```python
spiel_laeuft = True
schiff_versenkt = False
print(type(spiel_laeuft))   # <class 'bool'>
```

---

## 1.3 Ein- und Ausgabe

### Ausgabe mit `print()`

```python
print("Willkommen bei Schiffe versenken!")
print("Spieler:", spieler_name)
print("Punkte:", punkte)
```

Mehrere Werte trennt man mit Komma – `print()` fügt automatisch ein Leerzeichen ein.

### Eingabe mit `input()`

`input()` gibt **immer eine Zeichenkette** zurück:

```python
spieler_name = input("Wie heißt du? ")
print("Hallo,", spieler_name)
```

---

## 1.4 Typkonvertierung

Da `input()` immer `str` liefert, muss für Zahlen konvertiert werden:

| Funktion | Umwandlung | Beispiel |
|----------|-----------|---------|
| `int(x)` | → Ganzzahl | `int("5")` → `5` |
| `float(x)` | → Kommazahl | `float("3.5")` → `3.5` |
| `str(x)` | → Zeichenkette | `str(42)` → `"42"` |
| `bool(x)` | → Wahrheitswert | `bool(0)` → `False` |

### Beispiel: Koordinateneingabe

Im Spiel gibt der Spieler Zeile und Spalte als Zahl ein:

```python
eingabe_zeile  = input("Zeile  (0-9): ")
eingabe_spalte = input("Spalte (0-9): ")

zeile  = int(eingabe_zeile)
spalte = int(eingabe_spalte)

print("Du zielst auf:", zeile, spalte)
```

Kurzform (ohne Zwischenvariable):

```python
zeile  = int(input("Zeile  (0-9): "))
spalte = int(input("Spalte (0-9): "))
```

### Konvertierung kann scheitern

Gibt der Nutzer kein gültige Zahl ein, wirft Python einen `ValueError`.  
Fehlerbehandlung (try/except) ist Stoff späterer Kapitel.

---

## 1.5 Zusammenfassung

```python
# Spieler-Setup
spieler_name = input("Name des Spielers: ")
feld_groesse = 10          # Konstante – wird nicht verändert
punkte       = 0
spiel_laeuft = True

# Ausgabe
print("Spieler:", spieler_name)
print("Feldgröße:", feld_groesse, "x", feld_groesse)
print("Punkte:", punkte)
```

---

## Übungsaufgaben

### Aufgabe 1.1 – Spieler anlegen
Schreibe ein Programm, das nach dem Spielernamen fragt und folgende Variablen anlegt:
- `name` (str): der eingegebene Name  
- `versuche` (int): anfangs 0  
- `trefferquote` (float): anfangs 0.0  
- `gewonnen` (bool): anfangs `False`  

Gib alle vier Werte mit `print()` aus.

---

### Aufgabe 1.2 – Koordinateneingabe
Schreibe ein Programm, das den Spieler nach einer Zeile (0–9) und einer Spalte (0–9) fragt.  
Beide Eingaben sollen als `int` gespeichert und dann ausgegeben werden, z. B.:  
```
Du zielst auf Zeile 3, Spalte 7.
```

---

### Aufgabe 1.3 – Typkonvertierung verstehen
Gegeben:
```python
a = "5"
b = "3"
```
Was gibt `print(a + b)` aus? Was gibt `print(int(a) + int(b))` aus?  
Erkläre den Unterschied und schreibe das Programm, um es zu prüfen.

---

### Aufgabe 1.4 – Feldgröße berechnen
Ein Spielfeld ist `n × n` groß. Der Nutzer gibt `n` ein.  
Berechne und gib aus:
- Gesamtzahl der Felder  
- Anzahl der Felder in einer Zeile (= `n`)  
- Anzahl der Felder in einer Spalte (= `n`)  

Beispielausgabe für `n = 10`:
```
Spielfeld: 10 x 10
Felder gesamt: 100
```

---

> Musterlösungen zu allen Aufgaben: [Kapitel 10 – Lösungen](10_loesungen.md)
