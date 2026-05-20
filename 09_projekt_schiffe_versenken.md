# Kapitel 9 – Projekt: Schiffe versenken

> Dieses Kapitel baut alle Konzepte der vorherigen Kapitel zu einem vollständigen **Konsolenspiel** zusammen.  
> Es gibt einen menschlichen Spieler gegen einen einfachen Computer.

---

## 9.1 Spielregeln

| Regel | Wert |
|-------|------|
| Spielfeldgröße | 10 × 10 |
| Zellzustände | `~` Wasser, `S` Schiff, `X` Treffer, `O` Fehlschuss |
| Flotte | 1× Schlachtschiff (4), 2× Kreuzer (3), 3× Zerstörer (2) |
| Gewinnbedingung | Alle Schiffe des Gegners versenkt |
| Verlustbedingung | Alle eigenen Schiffe versenkt |
| Eingabeformat | Zeile und Spalte getrennt mit Leerzeichen, z. B. `3 7` |

---

## 9.2 Programmstruktur

```
schiffe_versenken.py
│
├── Konstanten
├── Hilfsfunktionen
│   ├── feld_erstellen()
│   ├── zeige_spielfeld()
│   ├── schiff_eintragen()
│   ├── koordinate_gueltig()
│   ├── schuss_ausfuehren()
│   ├── ist_schiff_versenkt()
│   ├── alle_schiffe_versenkt()
│   ├── eingabe_einlesen()
│   └── computer_schuss()
└── Hauptprogramm (main)
```

---

## 9.3 Vollständiger Quellcode

```python
# ================================================================
#  Schiffe versenken – Konsolenspiel
#  Kapitel 9: Vollständiges Projekt
# ================================================================

import random

# ── Konstanten ────────────────────────────────────────────────────
GROESSE    = 10
WASSER     = "~"
SCHIFF     = "S"
TREFFER    = "X"
FEHLSCHUSS = "O"

# Flottendefinition: Liste von Schiffsgrößen
FLOTTEN_KONFIGURATION = [4, 3, 3, 2, 2, 2]


# ── Spielfeld ─────────────────────────────────────────────────────

def feld_erstellen():
    """Gibt ein leeres 10x10-Spielfeld zurück."""
    return [[WASSER] * GROESSE for _ in range(GROESSE)]


def zeige_spielfeld(feld, verstecke_schiffe=False):
    """
    Gibt das Spielfeld auf der Konsole aus.
    verstecke_schiffe=True: Schiffe werden als Wasser angezeigt (Gegnersicht).
    """
    print("    ", end="")
    for s in range(GROESSE):
        print(f"{s:2}", end="")
    print()
    print("   +" + "--" * GROESSE + "-+")
    for z in range(GROESSE):
        print(f"{z:2} |", end="")
        for s in range(GROESSE):
            zelle = feld[z][s]
            if verstecke_schiffe and zelle == SCHIFF:
                zelle = WASSER
            print(f" {zelle}", end="")
        print(" |")
    print("   +" + "--" * GROESSE + "-+")


def zeige_beide_felder(feld_spieler, feld_gegner):
    """Zeigt eigenes und Gegnersfeld nebeneinander."""
    trenner = "      "
    print("    EIGENES FELD" + trenner + "   GEGNERSFELD")
    kopf = "    " + " ".join(str(i) for i in range(GROESSE))
    print(kopf + trenner + kopf)
    for z, (z_sp, z_ge) in enumerate(zip(feld_spieler, feld_gegner)):
        # Eigene Zeile
        eigene = " ".join(z_sp)
        # Gegner-Zeile (Schiffe versteckt)
        gegner_zeile = ["~" if z == SCHIFF else z for z in z_ge]
        gegner = " ".join(gegner_zeile)
        print(f"{z:2}  {eigene}{trenner}{z:2}  {gegner}")


# ── Schiffe platzieren ────────────────────────────────────────────

def schiff_eintragen(feld, schiff):
    """Trägt ein Schiff (Liste von Tupeln) in das Feld ein."""
    for zeile, spalte in schiff:
        feld[zeile][spalte] = SCHIFF


def schiff_passt(feld, schiff):
    """Prüft, ob ein Schiff platziert werden darf (keine Kollision, innerhalb Feld)."""
    for zeile, spalte in schiff:
        if not (0 <= zeile < GROESSE and 0 <= spalte < GROESSE):
            return False
        if feld[zeile][spalte] != WASSER:
            return False
    return True


def schiff_erstellen(zeile, spalte, laenge, horizontal):
    """Erzeugt ein Schiff als Liste von Koordinaten-Tupeln."""
    if horizontal:
        return [(zeile, spalte + i) for i in range(laenge)]
    else:
        return [(zeile + i, spalte) for i in range(laenge)]


def flotte_zufaellig_platzieren(feld, konfiguration):
    """
    Platziert alle Schiffe der Flotte zufällig auf dem Feld.
    Gibt die Flotte als Liste von Schiffen zurück.
    """
    flotte = []
    for laenge in konfiguration:
        platziert = False
        while not platziert:
            zeile      = random.randint(0, GROESSE - 1)
            spalte     = random.randint(0, GROESSE - 1)
            horizontal = random.choice([True, False])
            schiff     = schiff_erstellen(zeile, spalte, laenge, horizontal)
            if schiff_passt(feld, schiff):
                schiff_eintragen(feld, schiff)
                flotte.append(schiff)
                platziert = True
    return flotte


def flotte_manuell_platzieren(feld, konfiguration):
    """
    Lässt den Spieler seine Schiffe manuell platzieren.
    Gibt die Flotte als Liste von Schiffen zurück.
    """
    flotte = []
    for i, laenge in enumerate(konfiguration):
        zeige_spielfeld(feld)
        print(f"\nSchiff {i + 1}/{len(konfiguration)}, Länge: {laenge}")

        platziert = False
        while not platziert:
            try:
                zeile, spalte = map(int, input("  Startposition (Zeile Spalte): ").split())
                richtung      = input("  Richtung (h=horizontal, v=vertikal): ").strip().lower()
                horizontal    = richtung == "h"
                schiff        = schiff_erstellen(zeile, spalte, laenge, horizontal)

                if schiff_passt(feld, schiff):
                    schiff_eintragen(feld, schiff)
                    flotte.append(schiff)
                    platziert = True
                else:
                    print("  Ungültige Position – Schiff passt dort nicht. Nochmal.")
            except (ValueError, IndexError):
                print("  Ungültige Eingabe. Bitte Zeile und Spalte als zwei Zahlen eingeben.")
    return flotte


# ── Spiellogik ────────────────────────────────────────────────────

def koordinate_gueltig(zeile, spalte):
    return 0 <= zeile < GROESSE and 0 <= spalte < GROESSE


def bereits_beschossen(feld, zeile, spalte):
    return feld[zeile][spalte] in (TREFFER, FEHLSCHUSS)


def schuss_ausfuehren(feld, zeile, spalte):
    """
    Führt einen Schuss aus.
    Gibt True zurück, wenn getroffen, sonst False.
    """
    if feld[zeile][spalte] == SCHIFF:
        feld[zeile][spalte] = TREFFER
        return True
    else:
        feld[zeile][spalte] = FEHLSCHUSS
        return False


def ist_schiff_versenkt(schiff, feld):
    """Gibt True zurück, wenn alle Felder des Schiffs getroffen sind."""
    for zeile, spalte in schiff:
        if feld[zeile][spalte] != TREFFER:
            return False
    return True


def alle_schiffe_versenkt(flotte, feld):
    """Gibt True zurück, wenn alle Schiffe der Flotte versenkt sind."""
    for schiff in flotte:
        if not ist_schiff_versenkt(schiff, feld):
            return False
    return True


def finde_versenktes_schiff(flotte, zeile, spalte, feld):
    """Gibt das soeben versenkter Schiff zurück (oder None)."""
    for schiff in flotte:
        if (zeile, spalte) in schiff and ist_schiff_versenkt(schiff, feld):
            return schiff
    return None


# ── Eingabe / Computer ────────────────────────────────────────────

def spieler_eingabe(feld_gegner):
    """Liest eine gültige, noch nicht beschossene Koordinate ein."""
    while True:
        try:
            zeile, spalte = map(int, input("Dein Schuss (Zeile Spalte): ").split())
            if not koordinate_gueltig(zeile, spalte):
                print("  Koordinate außerhalb des Felds (0–9).")
            elif bereits_beschossen(feld_gegner, zeile, spalte):
                print("  Diese Koordinate wurde bereits beschossen.")
            else:
                return zeile, spalte
        except (ValueError, IndexError):
            print("  Ungültige Eingabe – bitte zwei Zahlen eingeben, z. B. '3 7'.")


def computer_schuss(feld_spieler, bereits_geschossen):
    """Wählt zufällig eine noch nicht beschossene Koordinate."""
    while True:
        zeile  = random.randint(0, GROESSE - 1)
        spalte = random.randint(0, GROESSE - 1)
        if (zeile, spalte) not in bereits_geschossen:
            bereits_geschossen.append((zeile, spalte))
            return zeile, spalte


# ── Hauptprogramm ────────────────────────────────────────────────

def main():
    print("=" * 50)
    print("        S C H I F F E  V E R S E N K E N")
    print("=" * 50)
    spieler_name = input("Dein Name: ").strip()
    if not spieler_name:
        spieler_name = "Spieler"

    # Felder erstellen
    feld_spieler = feld_erstellen()
    feld_computer = feld_erstellen()

    # Flotten platzieren
    print(f"\n{spieler_name}, platziere deine Flotte!")
    modus = input("Automatisch platzieren? (j/n): ").strip().lower()
    if modus == "j":
        flotte_spieler = flotte_zufaellig_platzieren(feld_spieler, FLOTTEN_KONFIGURATION)
        print("Deine Flotte wurde zufällig platziert.")
    else:
        flotte_spieler = flotte_manuell_platzieren(feld_spieler, FLOTTEN_KONFIGURATION)

    flotte_computer = flotte_zufaellig_platzieren(feld_computer, FLOTTEN_KONFIGURATION)
    print("\nDer Computer hat seine Flotte platziert.")

    # Statistiken
    schuesse_spieler  = 0
    treffer_spieler   = 0
    schuesse_computer = 0
    treffer_computer  = 0
    computer_bereits_geschossen = []

    # Hauptschleife
    spiel_laeuft = True

    while spiel_laeuft:
        print("\n" + "─" * 50)
        zeige_beide_felder(feld_spieler, feld_computer)
        print()

        # ── Spieler ist dran ──────────────────────────
        print(f"▶  {spieler_name}, du bist dran!")
        z, s = spieler_eingabe(feld_computer)
        schuesse_spieler += 1

        treffer = schuss_ausfuehren(feld_computer, z, s)

        if treffer:
            treffer_spieler += 1
            print(f"  💥 Treffer auf ({z}, {s})!")
            versenktes = finde_versenktes_schiff(flotte_computer, z, s, feld_computer)
            if versenktes:
                print(f"  🚢 Schiff mit {len(versenktes)} Feldern versenkt!")
        else:
            print(f"  🌊 Wasser auf ({z}, {s}).")

        if alle_schiffe_versenkt(flotte_computer, feld_computer):
            print(f"\n🎉 {spieler_name} hat gewonnen! Alle Schiffe des Computers versenkt!")
            spiel_laeuft = False
            break

        # ── Computer ist dran ─────────────────────────
        print(f"\n▶  Computer ist dran ...")
        z, s = computer_schuss(feld_spieler, computer_bereits_geschossen)
        schuesse_computer += 1

        treffer = schuss_ausfuehren(feld_spieler, z, s)

        if treffer:
            treffer_computer += 1
            print(f"  💥 Computer trifft ({z}, {s})!")
            versenktes = finde_versenktes_schiff(flotte_spieler, z, s, feld_spieler)
            if versenktes:
                print(f"  🚢 Dein Schiff mit {len(versenktes)} Feldern wurde versenkt!")
        else:
            print(f"  🌊 Computer schießt Wasser auf ({z}, {s}).")

        if alle_schiffe_versenkt(flotte_spieler, feld_spieler):
            print(f"\n💀 Der Computer hat gewonnen! Alle deine Schiffe wurden versenkt.")
            spiel_laeuft = False

    # ── Endergebnis ───────────────────────────────────
    print("\n" + "=" * 50)
    print("           SPIELENDE – STATISTIK")
    print("=" * 50)

    q_spieler  = treffer_spieler  / schuesse_spieler  * 100 if schuesse_spieler  > 0 else 0
    q_computer = treffer_computer / schuesse_computer * 100 if schuesse_computer > 0 else 0

    print(f"{spieler_name:15}: {schuesse_spieler:3} Schüsse, {treffer_spieler:3} Treffer ({q_spieler:.1f}%)")
    print(f"{'Computer':15}: {schuesse_computer:3} Schüsse, {treffer_computer:3} Treffer ({q_computer:.1f}%)")
    print()
    print("Finales Spielfeld:")
    zeige_beide_felder(feld_spieler, feld_computer)


# ── Einstiegspunkt ────────────────────────────────────────────────
main()
```

---

## 9.4 Erweiterungsideen

| Idee | Beschreibung |
|------|-------------|
| Intelligenter Computer | Computer merkt sich Treffer und schießt benachbarte Felder |
| Zwei Spieler | Zwei menschliche Spieler am selben Computer (Bildschirm abdecken) |
| Highscore | Trefferquoten mehrerer Spiele speichern und ausgeben |
| Schwierigkeitsgrade | Computer: zufällig / mittel (Trefferfolgung) / schwer (Suche) |
| ASCII-Art | Schiffssymbole je nach Länge verschieden darstellen |

---

## 9.5 Eingesetzte Konzepte – Überblick

| Konzept | Kapitel | Verwendung im Projekt |
|---------|---------|----------------------|
| Variablen & Typen | 1 | Konstanten, Zähler, Namen |
| Operatoren | 2 | Validierung, Trefferprüfung |
| Verzweigungen | 3 | Schuss auswerten, Spielende |
| Schleifen | 4 | Spielrunde, Feld ausgeben, Platzierung |
| Strings | 5 | Ausgabe, Eingabe verarbeiten |
| Listen & Tupel | 6 | Spielfeld (2D), Flotte, Koordinaten |
| Funktionen | 7 | Modularer Aufbau, Wiederverwendbarkeit |
| map, enumerate, zip | 8 | Eingabe, Feldausgabe, Statistik |

---

## Übungsaufgaben (Erweiterungen)

### Aufgabe 9.1 – Intelligenter Computer
Erweitere `computer_schuss()`: Wenn der Computer in der letzten Runde getroffen hat, soll er zuerst die **benachbarten Felder** (oben, unten, links, rechts) versuchen.

---

### Aufgabe 9.2 – Spielstatistik nach jeder Runde
Gib nach jedem Zug die aktuelle Anzahl versenkter Schiffe beider Seiten aus.

---

### Aufgabe 9.3 – Wiederholung
Frage am Ende des Spiels, ob nochmals gespielt werden soll. Wenn ja, starte das Spiel neu.

---

### Aufgabe 9.4 – Spielfeld als String
Schreibe eine Funktion `feld_als_string(feld, verstecke_schiffe)`, die das Spielfeld als **einen einzigen String** (mit Zeilenumbrüchen) zurückgibt, statt direkt zu drucken.  
Dies ermöglicht z. B. das Schreiben in eine Datei.

---

> Musterlösungen zu allen Aufgaben: [Kapitel 10 – Lösungen](10_loesungen.md)
