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
- **Live-Tag-Modus** zum Mitloggen am Prüfungstag: pro **wartende** Zeile +1/+5/+10/−1/−5-min-Buttons, alle folgenden **wartenden** Prüfungen verschieben sich automatisch
- **Live-Dirigent** — manuelle Tagessteuerung: jeden Kandidaten per Klick weiterschalten (wartet → Vorbereitung → Prüfung → fertig). Der Klick verankert die **echte Uhrzeit** (Vorbereitung = jetzt + Vorbereitungszeit, Prüfung = jetzt). Ein **„↩ zurück"-Knopf** macht Fehlklicks rückgängig (Prüfung → Vorbereitung → wartet).
- **Gestartete/fertige Prüfungen sind fixiert** — wer schon dran war oder läuft, behält seine echte Zeit; nur **wartende** Prüfungen verschieben sich, wenn man eine andere anstößt oder verzögert. **Fertige** Prüfungen zeigen ihre **tatsächliche Dauer** (Beginn → „Fertig").
- **Chronologische Live-Liste** — die Tabelle sortiert sich nach Uhrzeit (Vergangenheit oben → Zukunft unten); die laufende Prüfung wandert nach oben. Klicks treffen trotz Umsortierung immer die richtige Person.
- **Beamer-Ansicht** (Vollbild, read-only) zum Aushängen oder Projizieren — große 7-Segment-Live-Uhr, Raumschild, Zonen für *Jetzt in Prüfung*, *Als Nächstes*, *In Vorbereitung* (mehrere, je mit Timer) und *Nächste Vorbereitung* (rot), plus Status-Liste mit Auto-Scroll. Zeigt **Prüfer und Beisitz/2. Prüfer**. Öffnet **automatisch im Vollbild** (Adresszeile weg); nur bei eingeschaltetem Live-Tag-Modus.
- **Live-Stand-Sicherung** — der komplette Stand (Plan, Reihenfolge, Verschiebungen, Status, Zeiten) wird laufend lokal gesichert. Nach Absturz/Neuladen bietet ein Banner **„Wiederherstellen"** an; **„↺ Live zurücksetzen"** startet sauber neu.
- **Live-Spiegelung** (optional, Supabase): ein Steuergerät (Master) treibt den Tag, beliebig viele Anzeige-Geräte sehen live mit. **Zwei getrennte Links:** dauerhafter **Beamer-Link** (einmal einrichten) und **Schüler-QR mit Session-Code** (pro Prüfungstag neu; „↻ Neuen Schüler-Link erzeugen" macht alte ungültig). Ohne Konfiguration/Session bleibt die App rein lokal.
- **Prüfungskommission editierbar** — nach einem Import lassen sich **Beisitz/Vorsitz/KV** direkt in der Liste eintragen; „↻ Vorsitz/KV auf alle" übernimmt die Einstellungswerte auf alle Zeilen. Alles wird gespeichert und mit-exportiert.
- **Prüfungstag laden** (PDF/Excel) — fertige Einteilung 1:1 übernehmen (Zeiten, Fächer, Kommission) statt neu zu rechnen.
- **Excel-Export** professionell formatiert: HTL-Logo, Titelblock, farbcodierte Prüferzellen, Kommission, zwei Sheets (Schüler- und Lehrersicht)
- **Retro 7-Segment-Digitaluhr** im Header (DSEG7, live) mit Wochentag & Datum
- **Integrierte Erklärung** — aufklappbarer „So funktioniert die Einteilung"-Block direkt im Tool
- **QR-Code in der Beamer-Ansicht** — „Live mitschauen": Schüler scannen direkt von der Projektion den **Schüler-Link** (mit Session-Code).
- **Versionskennung im Footer** (und Untertitel) — z. B. „Version 2026-06-16 … (r5)" als verlässlicher Cache-Check nach einem Update.
- **Technik-/Blueprint-Look** — Hintergrund mit Raster, Akzent-Orbs, Schaltsymbolen und animierter Oszilloskop-Sinuskurve; glasige Panels.

## Die zwei Schalter: lokal steuern vs. live spiegeln

Am Prüfungstag gibt es **zwei getrennte Schalter** — sie machen Unterschiedliches:

| Schalter | Was er tut | Internet/Cloud? |
| --- | --- | --- |
| **Live-Tag-Modus** (Schalter im Tool) | Schaltet den **lokalen Prüfungstag-Modus auf DIESEM Gerät** ein: der **Live-Dirigent** (Kandidaten per Klick weiterschalten, +5/−5 min) wird aktiv, und die **Beamer-Ansicht** lässt sich öffnen. | **Nein** – rein lokal, funktioniert offline. |
| **„Prüfungstag starten"** (im Live-Spiegelung-Fenster) | Schaltet die **Cloud-Spiegelung (Supabase)** ein: der Plan wird an **andere Geräte** gesendet, die den **Anzeige-Link/QR** geöffnet haben (zweiter Beamer, Schülerhandys). Ohne ihn zeigen externe Geräte nur den Ruhe-Bildschirm. | **Ja** – sendet live an alle Anzeige-Geräte. |

**Kurzregel:**
- Du projizierst **nur am Beamer-PC** → es reicht **Live-Tag-Modus**.
- Auch **Schüler/zweiter Beamer** sollen live mitsehen → zusätzlich **„Prüfungstag starten"** und QR/Link verteilen.

Typischer Ablauf: **Live-Tag an** → Kandidaten im **Live-Dirigent** durchsteuern → bei Bedarf **„Prüfungstag starten"** für die Multi-Geräte-Spiegelung.

## Master-Sperre (nur ein steuerndes Gerät)

Damit sich nicht zwei Steuergeräte gegenseitig überschreiben, gilt: Es kann immer nur **ein aktiver Master** schreiben.
- Öffnet ein zweites Gerät den Master-Link, geht es automatisch in **passiv** und zeigt das Banner **„Ein anderes Gerät steuert gerade – Steuerung übernehmen"** (mit „✕ Später").
- Per Klick auf **„Steuerung übernehmen"** (oder einfach durch eine Steueraktion) wird dieses Gerät aktiv; das andere wird automatisch passiv.
- Fällt der aktive Master aus (kein Heartbeat > 9 s), meldet das andere „Aktuell steuert kein Gerät" und kann konfliktfrei übernehmen.
- Technisch: der Besitzer-Stempel steckt nur im Live-Payload (JSON) — **keine Datenbank-Änderung nötig**.

> **Wichtig — der Plan ist pro Gerät:** Die Master-Sperre ist nur ein Banner und blendet **nichts** aus. Über die Cloud läuft nur die **Anzeige-Spiegelung** für Zuschauer, **nicht** der Plan zwischen Steuergeräten. Öffnet ein **anderes/zweites Gerät** den Master-Link, sieht es zunächst die **leere Startseite** — die Schülerliste erscheint dort erst, wenn dieses Gerät den Prüfungstag **selbst lädt** (gleiche Excel/PDF) oder eine **eigene** Sicherung wiederherstellt. Auf dem ursprünglichen Gerät bleibt alles sichtbar. Für eine **Übergabe/Vertretung** also die Prüfungstag-Datei am Ersatzgerät laden.

## Am Prüfungstag — Ablauf & sauber beenden

**Starten**
1. Plan berechnen *oder* fertigen **Prüfungstag laden** (PDF/Excel).
2. **Live-Tag-Modus** einschalten.
3. **Beamer-Ansicht** öffnen → geht automatisch in **Vollbild** (Adresszeile weg; `✕`/Esc beendet es).
4. Sollen Schüler/zweiter Beamer mitsehen: im Live-Spiegelung-Fenster **„▶ Prüfungstag starten"** und **Schüler-QR** verteilen.

**Steuern (Live-Dirigent)**
- Pro Kandidat: **▶ Vorbereitung → ▶ Prüfung → ✓ Fertig**. Falsch geklickt? **↩ zurück**.
- **Verzögerungen** (+/−min) nur bei **wartenden** Zeilen; gestartete/fertige bleiben fix.
- Die Liste sortiert sich automatisch chronologisch; die laufende Prüfung steht oben.

**Sauber beenden**
1. **„■ Prüfungstag beenden"** drücken → Beamer und alle Schülerhandys schalten sofort auf den **Ruheschirm** (keine Live-Daten mehr).
2. Beamer mit **`✕` / Esc** schließen (beendet auch das Vollbild).
3. Optional fürs Archiv vorher **Excel exportieren** (enthält die Kommission).

> Der Stand bleibt **lokal gespeichert** (Absturzschutz) — du verlierst nichts. Um Mitternacht schaltet die Anzeige ohnehin selbst auf den Ruheschirm. Für einen **ganz neuen** Prüfungstag zusätzlich **„↺ Live zurücksetzen"** und ggf. **„↻ Neuen Schüler-Link erzeugen"**.

## Beamer-Link vs. Schüler-QR (Session-Codes)

Im Live-Spiegelung-Fenster gibt es **zwei Links** mit unterschiedlichem Zweck:

| Link | Zweck | Verhalten |
| --- | --- | --- |
| **Beamer-Link** (`…?mode=anzeige`) | Einmal am **Beamer-/Presenter-PC** einrichten | **Gilt dauerhaft**, läuft nie ab, zeigt immer die aktuelle Session |
| **Schüler-Link / QR** (`…?mode=anzeige&sid=…`) | An **Schüler** verteilen | Trägt einen **Session-Code**; pro Prüfungstag automatisch neu. **„↻ Neuen Schüler-Link erzeugen"** macht alte Links sofort ungültig (sie zeigen dann „Link abgelaufen"). Der Beamer-Link bleibt davon unberührt. |

Der **QR-Code in der Beamer-Ansicht** zeigt immer auf den aktuellen **Schüler-Link** (mit Code), nicht auf den Beamer-Link.

## Live-Stand: sichern, wiederherstellen, zurücksetzen

- **Automatisch gesichert:** Nach jeder Änderung wird der komplette Live-Stand (Plan, Reihenfolge/Tausche, Verschiebungen, Status fertig/aktiv/wartend inkl. Zeitstempel, Kommission) lokal im Browser (`localStorage`) gespeichert.
- **Wiederherstellen:** Stürzt der Master ab oder lädt die Seite neu, erscheint oben ein Banner **„Letzter Live-Stand … Wiederherstellen / Verwerfen"** (nur für den heutigen Stand). Ein Klick holt alles exakt zurück; die Spiegelung nimmt automatisch wieder auf. Beim **Berechnen** oder **Datei-Laden** verschwindet das Banner (dann ist alles neu).
- **Zurücksetzen:** **„↺ Live zurücksetzen"** löscht alle Status + Verschiebungen + den gespeicherten Stand (der **Plan bleibt**).

> Hinweis: Der Stand liegt **pro Gerät + Browser**. Ein Reload auf demselben Laptop stellt wieder her; ein anderes Gerät hat den Stand nicht.

## Prüfungstag laden & Kommission bearbeiten

- **Prüfungstag laden (PDF/Excel):** Übernimmt eine bereits erstellte Einteilung 1:1 (Zeiten, Fächer, Prüfer, Kommission) — kein Neurechnen.
- **Kommission bearbeiten:** Im **Planungsmodus** sind die Spalten **Beisitz / Vorsitz / KV** direkt in der Tabelle editierbar (klicken, Kürzel eintragen). **„↻ Vorsitz/KV auf alle"** überträgt die Werte aus den Einstellungen auf alle Zeilen. Beisitz/2. Prüfer erscheint auch im **Beamer** und in der **Schüler-/QR-Ansicht**.
- **Export:** Alle manuellen Änderungen landen wieder im **Excel-Export** (Reiter „Prüfungskommission"), der sich erneut laden lässt.

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
