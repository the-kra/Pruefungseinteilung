# kranzlab · Matura-Einteilung

> Zeitoptimierter Prüfungsplan für mündliche Reife- & Diplomprüfungen an HTLs — eigenständige HTML-Datei, läuft offline im Browser.

**Live-Demo:** [https://https://the-kra.github.io/Pruefungseinteilung/](https://the-kra.github.io/Pruefungseinteilung/)

---

## Was macht das Tool?

Aus einer einfachen Excel-Matrix (Schüler × Fach → Prüfer-Kürzel) berechnet das Tool einen **zeitoptimierten Prüfungsplan**, der

- Lehrer in kompakte Blöcke packt (möglichst wenig Leerlauf),
- Schülerwartezeiten minimiert,
- die Vorbereitungszeit korrekt einplant,
- Volltag- und 2-Gruppen-Varianten gegenüberstellt und eine Empfehlung gibt.

Der Plan lässt sich manuell per Drag-and-drop nachjustieren — Constraints (Vorbereitung, Pausen, Lehrerblöcke) werden live geprüft und Konflikte rot markiert.

## Funktionen

- **Excel-Import** der Eingabematrix
- **Solver** mit einstellbaren Parametern (Prüfungsdauer, Vorbereitung, Pausen, max. Blocklänge, Tagesbeginn, Mittagspause)
- **Gantt-Plan** mit farbcodierten Prüfern und live mitlaufender roter „Jetzt"-Linie
- **Drag-and-drop** zur manuellen Umsortierung mit Konflikt-Wächter
- **Live-Tag-Modus** zum Mitloggen am Prüfungstag: pro Zeile +5/+10/−5-min-Buttons, alle folgenden Prüfungen verschieben sich automatisch
- **Beamer-Ansicht** (Vollbild, read-only) zum Aushängen oder Projizieren — mit großer 7-Segment-Live-Uhr, aktueller & nächster Prüfung, Status-Liste
- **Excel-Export** professionell formatiert: HTL-Logo, Titelblock, farbcodierte Prüferzellen, AV-Block, zwei Sheets (Schüler- und Lehrersicht)
- **Retro 7-Segment-Digitaluhr** im Header (DSEG7, live)

## Eingabeformat

Eine einfache `.xlsx`-Datei:

| Schüler            | FI  | AUT | ES  | MTSA |
| ------------------ | --- | --- | --- | ---- |
| Aigner Lukas       | kra | bod |     | rei  |
| Berger Florian     | kra | bod | wei |      |
| Fellner Jonas      | wal | bod | wei | rei  |
| Gruber Maximilian  | kra |     | wei | rei  |
| Hofer Tobias       | wal | bod | wei |      |
| Köll Daniel        |     | bod | wei | rei  |
| Moser Stefan       | kra | bod |     | rei  |
| Pichler Elias      | wal |     | wei | rei  |

- **Spalte A:** Schülername
- **Weitere Spalten:** Fachnamen (Header) — z.B. FI, AUT, ES, MTSA
- **Zelleninhalt:** Prüfer-Kürzel (leer = keine Prüfung)

### Lehrer-Kürzel (Beispiel)

| Kürzel | Lehrer            | Fächer        |
| ------ | ----------------- | ------------- |
| kra    | DI Theodor Kranz  | FI            |
| wal    | Mag. Anna Walter  | FI            |
| bod    | DI Michael Bodner | AUT           |
| wei    | DI Sabine Weiß    | ES            |
| rei    | Mag. Lukas Reiter | MTSA          |

> Die Kürzel sind frei wählbar — das Tool nutzt sie als farbcodierte Marker und übernimmt sie 1:1 in die Pläne und den Excel-Export.

Eine fertige Demo-Matrix kann im Tool per Klick geladen werden, oder du lädst [`Demo_Matrix.xlsx`](Demo_Matrix.xlsx) herunter.

## Schnellstart

1. **Live-Demo öffnen** oder `index.html` herunterladen und lokal im Browser öffnen
2. Eigene Excel-Matrix laden — oder „Demo-Matrix laden" für sofortiges Ausprobieren
3. Parameter anpassen (Standardwerte funktionieren in den meisten Fällen)
4. **„Plan berechnen"** klicken
5. Ergebnis als Gantt-Plan und Tabelle prüfen, ggf. Zeilen ziehen
6. **Excel-Plan exportieren** für Aushang/Verteilung
7. Am Prüfungstag: **Live-Tag-Modus** aktivieren, **Beamer-Ansicht** öffnen

## Datenschutz

Das Tool läuft **vollständig im Browser**. Hochgeladene Daten werden nicht an einen Server übertragen, nichts wird gespeichert. Auch der Excel-Export wird lokal im Browser erzeugt.

## Technik

Eine einzige `index.html`-Datei — keine Installation, kein Build, keine Abhängigkeiten zur Laufzeit. Eingebettet sind:

- [SheetJS](https://sheetjs.com/) für Excel-Import
- [ExcelJS](https://github.com/exceljs/exceljs) für formatierten Export inkl. Logo
- [DSEG7](https://www.keshikan.net/fonts-e.html)-Font für die Retro-Uhr (eingebettet als base64)
- [Lexend](https://fonts.google.com/specimen/Lexend) als Hauptschrift

## Hosting auf GitHub Pages

```bash
git clone https://github.com/DEIN-USERNAME/DEIN-REPO.git
cd DEIN-REPO
# index.html und Demo_Matrix.xlsx hinzufügen
git add index.html Demo_Matrix.xlsx README.md
git commit -m "Initial commit"
git push
```

Dann im Repo: **Settings → Pages → Source: Deploy from a branch → main / root → Save**.
Nach ein paar Minuten ist das Tool unter `https://DEIN-USERNAME.github.io/DEIN-REPO/` erreichbar.

> **Hinweis:** Die HTML-Datei muss in `index.html` umbenannt sein, damit GitHub Pages sie als Startseite anzeigt.

## Roadmap

- **Lehrer-Fixierung:** gewünschte Lehrer-Zeiten und Wünsche vorab festlegen, die der Solver berücksichtigt
- Parallele Prüfungskommissionen (mehrere Räume gleichzeitig)
- Live-Sync zwischen mehreren Geräten am Prüfungstag

## Autor

**DI Theodor Kranz** · [kranzlab](https://htl1-klagenfurt.at/elektrotechnik/)
HTL1 Lastenstraße Klagenfurt · Höhere Abteilung für Elektrotechnik
Kontakt: [kra@htl1-klu.at](mailto:kra@htl1-klu.at)

## Lizenz & Kontakt

Frei für den Einsatz an österreichischen HTLs. Bei Fragen, Wünschen oder Fehlerberichten gerne ein Issue öffnen oder per Mail an [kra@htl1-klu.at](mailto:kra@htl1-klu.at).
