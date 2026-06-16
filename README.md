# kranzlab · Matura-Einteilung

> Zeitoptimierter Prüfungsplan für mündliche Reife- & Diplomprüfungen an HTLs — eigenständige HTML-Datei, läuft offline im Browser.

**Live-Demo:** [https://the-kra.github.io/Pruefungseinteilung/](https://the-kra.github.io/Pruefungseinteilung/)

---

## Was macht das Tool?

Aus einer einfachen Excel-Matrix (Schüler × Fach → Prüfer-Kürzel) berechnet das Tool einen **zeitoptimierten Prüfungsplan**, der

- Lehrer in kompakte Blöcke packt (möglichst wenig Leerlauf),
- Schülerwartezeiten minimiert,
- die Vorbereitungszeit korrekt einplant,
- Volltag- und 2-Gruppen-Varianten gegenüberstellt und eine Empfehlung gibt.

Der Plan lässt sich manuell per Drag-and-drop nachjustieren — Constraints (Vorbereitung, Pausen, Lehrerblöcke) werden live geprüft und Konflikte rot markiert.

## So funktioniert die Einteilung

Das Tool „lernt" nichts und ist keine KI — es arbeitet wie ein gewissenhafter Mensch mit Stundenplan und Stoppuhr, der den Tag von früh nach spät Block für Block füllt. Jede gleich lange Prüfung ist ein **Block**; gleiche Prüfer werden zu zusammenhängenden Blöcken gebündelt, damit niemand ständig kommen und gehen muss. Diese Logik ist im Tool als aufklappbarer Erklärblock direkt eingebaut.

Bei jedem freien Termin vergibt das Tool den Platz an die Prüfung, die sechs einfache **Spielregeln** am besten erfüllt:

1. **Schüler nicht hetzen** — zwischen zwei eigenen Prüfungen liegt mindestens die Vorbereitungszeit *(Felder: Vorbereitung, Max. Block Schüler)*
2. **Prüfer am Stück lassen** — kompakte Lehrerblöcke bis zum erlaubten Maximum *(Feld: Max. Block Lehrer)*
3. **Schülerpause einhalten** — fremde Prüfungen dazwischen, bevor derselbe Schüler wieder dran ist *(Feld: Min. Pause Schüler)*
4. **Lehrerpause einhalten** — Zeit für Administratives (Protokolle, Notenbesprechung) zwischen Lehrerblöcken *(Feld: Min. Pause Lehrer)*
5. **Keine unnötige Leerzeit** — es startet stets die früheste erlaubte Prüfung *(Feld: Max. Lücke Schüler)*
6. **Mittag & Gruppen trennen** — Mittagspause und der Wechsel Vormittag→Nachmittag unterbrechen jeden Block *(Felder: Mittagspause, Tagesaufteilung)*

Die interne „Punktezahl" ist dabei nur eine simple Rechenregel, die diese Spielregeln gegeneinander abwägt — kein maschinelles Lernen. Der eingebaute Erklärblock zeigt zusätzlich pro Regel, **welches Eingabefeld** sie steuert und **was passiert, wenn man daran dreht** (mehr/weniger).

## Funktionen

- **Excel-Import** der Eingabematrix
- **Solver** mit einstellbaren Parametern (Prüfungsdauer, Vorbereitung, Pausen, max. Blocklänge, Tagesbeginn, Mittagspause)
- **Gantt-Plan** mit farbcodierten Prüfern und live mitlaufender roter „Jetzt"-Linie
- **Drag-and-drop** zur manuellen Umsortierung mit Konflikt-Wächter
- **Live-Tag-Modus** zum Mitloggen am Prüfungstag: pro Zeile +5/+10/−5-min-Buttons, alle folgenden Prüfungen verschieben sich automatisch
- **Live-Dirigent** — manuelle Tagessteuerung: jeden Kandidaten per Klick weiterschalten (wartet → in Vorbereitung → in Prüfung → fertig). Der Klick verankert die echte Uhrzeit, alle folgenden Prüfungen rücken automatisch nach. Mitlaufender Timer zeigt die Vorbereitungsdauer; der Status bleibt im Browser gespeichert (übersteht einen Reload).
- **Beamer-Ansicht** (Vollbild, read-only) zum Aushängen oder Projizieren — mit großer 7-Segment-Live-Uhr, getrennten Zonen für *Jetzt in Prüfung*, *Als Nächstes*, *In Vorbereitung* (mehrere gleichzeitig, je mit Timer) und *Nächste Vorbereitung* (rot hervorgehoben), plus Status-Liste mit Auto-Scroll
- **Live-Spiegelung** (optional, Supabase): ein Steuergerät (Master) treibt den Tag, beliebig viele Anzeige-Geräte (Beamer, Schülerhandys) sehen den Plan live mit — Teilen per Link oder QR-Code. Ohne Konfiguration/Session bleibt die App rein lokal.
- **Excel-Export** professionell formatiert: HTL-Logo, Titelblock, farbcodierte Prüferzellen, AV-Block, zwei Sheets (Schüler- und Lehrersicht)
- **Retro 7-Segment-Digitaluhr** im Header (DSEG7, live) mit Wochentag & Datum
- **Integrierte Erklärung** — aufklappbarer „So funktioniert die Einteilung"-Block direkt im Tool: erklärt die Block-Idee und die Logik in einfachen Worten, ohne Programmierwissen

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
7. Am Prüfungstag: **Live-Tag-Modus** aktivieren, Kandidaten im **Live-Dirigent** weiterschalten, **Beamer-Ansicht** öffnen
8. Optional mit mehreren Geräten: Steuergerät über `index.html?mode=master&key=…` öffnen, **„Prüfungstag starten"**, dann Anzeige-Link/QR an Beamer & Schüler verteilen

## Datenschutz

Die **Planung** läuft vollständig im Browser: hochgeladene Excel-Daten verlassen das Gerät nicht, und der Excel-Export wird lokal erzeugt.

Zwei Dinge weichen davon bewusst ab und werden nur aktiv, wenn du sie nutzt:

- **Live-Dirigent-Status** wird lokal im Browser gespeichert (`localStorage`), damit ein versehentlicher Reload am Prüfungstag den Stand nicht verliert. Diese Daten bleiben auf dem Gerät.
- **Live-Spiegelung:** Sobald du am Master-Gerät **„Prüfungstag starten"** drückst, wird der aktuelle Plan (Schülernamen, Fächer, Prüfer, Zeiten) an eine **Supabase-Datenbank** übertragen, damit die Anzeige-Geräte ihn live sehen. Wer den Anzeige-Link bzw. QR-Code kennt, kann den laufenden Plan einsehen. Ohne aktive Session – und ohne hinterlegte Supabase-Konfiguration – wird nichts übertragen und die App bleibt rein lokal.

## Technik

Eine einzige `index.html`-Datei — keine Installation, kein Build. Die Kernfunktionen (Planung, Gantt, Excel-Export, Beamer, Live-Dirigent) laufen vollständig offline. **Eingebettet** (offline verfügbar) sind:

- [SheetJS](https://sheetjs.com/) für Excel-Import
- [ExcelJS](https://github.com/exceljs/exceljs) für formatierten Export inkl. Logo
- [DSEG7](https://www.keshikan.net/fonts-e.html)-Font für die Retro-Uhr (eingebettet als base64)
- [Lexend](https://fonts.google.com/specimen/Lexend) als Hauptschrift

Nur für die **optionale Live-Spiegelung** werden zwei Bibliotheken per CDN nachgeladen (also Internet nötig):

- [Supabase JS](https://supabase.com/) für die geräteübergreifende Echtzeit-Spiegelung
- [qrcode.js](https://github.com/davidshimjs/qrcodejs) für den Anzeige-QR-Code

## Hosting auf GitHub Pages

```bash
git clone https://github.com/the-kra/Pruefungseinteilung.git
cd Pruefungseinteilung
# index.html und Demo_Matrix.xlsx hinzufügen
git add index.html Demo_Matrix.xlsx README.md
git commit -m "Initial commit"
git push
```

Dann im Repo: **Settings → Pages → Source: Deploy from a branch → main / root → Save**.
Nach ein paar Minuten ist das Tool unter `https://the-kra.github.io/Pruefungseinteilung/` erreichbar.

> **Hinweis:** Die HTML-Datei muss in `index.html` umbenannt sein, damit GitHub Pages sie als Startseite anzeigt.

## Roadmap

- **Lehrer-Fixierung:** gewünschte Lehrer-Zeiten und Wünsche vorab festlegen, die der Solver berücksichtigt
- Parallele Prüfungskommissionen (mehrere Räume gleichzeitig)

## Autor

**DI Theodor Kranz** · [kranzlab](https://htl1-klagenfurt.at/elektrotechnik/)
HTL1 Lastenstraße Klagenfurt · Höhere Abteilung für Elektrotechnik
Kontakt: [kra@htl1-klu.at](mailto:kra@htl1-klu.at)

## Lizenz & Kontakt

Frei für den Einsatz an österreichischen HTLs. Bei Fragen, Wünschen oder Fehlerberichten gerne ein Issue öffnen oder per Mail an [kra@htl1-klu.at](mailto:kra@htl1-klu.at).
