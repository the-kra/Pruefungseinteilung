# Verbesserungsvorschläge & Ideen

Sammlung möglicher Verbesserungen für die Prüfungseinteilung / den Live-Dirigenten.
Stand: 2026-06-15. Reihenfolge ≈ nach Nutzen/Aufwand.

---

## Offen (vorgeschlagen, noch nicht umgesetzt)

### 1. Wartende Zeiten als „frühestens ab jetzt" statt reiner Plan-Schätzung
**Beobachtung:** Eine wartende Prüfung kann eine Beginn-Zeit anzeigen, die real nicht
mehr erreichbar ist (z. B. „Beginn 09:27", obwohl es schon 09:15 ist und noch 30 min
Vorbereitung nötig wären). Die Zahl ist eine reine Reihenfolge-Schätzung aus dem Plan.

**Idee:** Für wartende Prüfungen die projizierte Zeit nie kleiner als
`jetzt + Vorbereitungszeit` ansetzen, damit die Schätzung realistisch bleibt.

- Nutzen: realistischere Vorschau am Beamer/in der Liste.
- Risiko: gering–mittel (Eingriff in `retime`-Kaskade, gut testbar).
- Status: bewusst zurückgestellt (kosmetisch, kein Ablauf-Fehler).

### 2. Vorbereitungs-Balken behält die echte Vorbereitungszeit
**Beobachtung:** Drückt man „Prüfung" früher/später als nach genau der Vorbereitungszeit,
zeigt die Spalte „Vorber." danach `Beginn − Vorbereitungszeit` (nominal), nicht den
tatsächlichen Vorbereitungsbeginn (`prepAt`).

**Idee:** Bei laufender/fertiger Prüfung `prepStart = prepAt` verwenden (echter Wert),
analog zur „echten Dauer" der fertigen Prüfung.

- Nutzen: korrekter Vorbereitungs-Balken im Gantt.
- Risiko: gering (nur Anzeige).
- Status: bewusst zurückgestellt (rein kosmetisch).

### 3. Laufende Prüfung im Gantt farblich markieren
**Idee:** Im Gantt die **laufende** Prüfung hervorheben (z. B. rot umranden) und
**erledigte** abdunkeln – analog zu den Status-Badges in der Liste.

- Nutzen: bessere Ablesbarkeit am Beamer.
- Risiko: gering (nur CSS/Render in `renderGantt`).

### 4. Sanfter Hinweis bei „gleicher Name 2× gleichzeitig in Prüfung"
**Idee:** Wird ein Kandidat in „Prüfung" gesetzt, obwohl eine andere seiner Prüfungen
schon „Prüfung" ist, eine **nicht blockierende** Rückfrage zeigen (kein harter Block).

- Nutzen: fängt seltene Fehlklicks ab.
- Risiko: gering, ABER bewusst zurückgestellt, um den stabilen Live-Ablauf und
  schnelle Korrekturen nicht zu stören. Nur als Hinweis, niemals als Sperre umsetzen.

### 5. Laufende Prüfung wächst über die Plandauer hinaus (Live-Balken)
**Idee:** Solange eine Prüfung „läuft", den Balken/das Ende mit der echten Uhr
mitwachsen lassen (statt fix Beginn + Plandauer), bis „Fertig" gedrückt wird.

- Nutzen: zeigt am Beamer den echten Überzug in Echtzeit.
- Risiko: mittel (sekündliches Re-Rendern nötig, Performance beachten).

### 6. QR-Code für die Schüleransicht direkt im Beamer einbetten
**Idee:** In der Beamer-Ansicht (projizierter Schirm) einen **QR-Code** für die
Schüler-Anzeige einblenden, damit Schüler ihn direkt vom Beamer scannen können –
ohne dass der Lehrer den Link separat teilen muss.

- Nutzen: Schüler kommen mit einem Blick aufs Handy zur Live-Ansicht.
- Risiko: gering (QR-Lib ist schon vorhanden).
- Hinweis: Der QR muss den **rotierenden Schüler-Link** (mit `sid`) zeigen, nicht den
  dauerhaften Beamer-Link.

### 7. Beamer-Adresse vor Schülern verbergen
**Idee:** Die Adresszeile/den Steuer-Link am Beamer **nicht offen anzeigen** – entweder
ausblenden oder so lang/unleserlich gestalten, dass Schüler sie nicht einfach
abfotografieren oder abtippen können (z. B. nur QR zeigen, URL kürzen/verstecken).

- Nutzen: verhindert, dass Schüler die Steuer-/Master-Adresse mitlesen.
- Risiko: gering (nur Anzeige).
- **Wichtiger Hinweis:** Echte Sicherheit gibt das nicht – der Link/Schlüssel steht
  ohnehin im **öffentlichen Quellcode** der einen `index.html`. Für echten Schutz
  müsste die App sauber aufgeteilt werden (siehe #8).

### 8. Projekt aus der einen großen `index.html` sauber aufteilen
**Idee:** Die App ist aktuell eine einzige `index.html` (HTML + CSS + gesamtes JS +
Konfiguration inkl. Schlüssel/Links). Künftig sinnvoll: in getrennte Dateien
(HTML / CSS / JS-Module) aufteilen und **Geheimnisse/Steuer-Schlüssel** nicht im
öffentlich ausgelieferten Code halten (z. B. Master-Steuerung serverseitig / per
geschütztem Bereich).

- Nutzen: Wartbarkeit, echte Trennung von Master/Anzeige, echter Zugriffsschutz.
- Risiko: hoch (größerer Umbau) – bewusst als Zukunftsthema.

### 9. ⚠ Masteransicht/Steuerung gegen Sabotage schützen (SICHERHEIT, wichtig)
**Problem:** Die Steuerung wird per `?mode=master&key=…` freigeschaltet, und dieser
**Schlüssel steht im öffentlich ausgelieferten Quellcode** (eine `index.html` auf
GitHub Pages). Ebenso der Schreib-Schlüssel für die Datenbank (`set_live_state`).
Wer den Quelltext liest, kann sich also als Master ausgeben und den laufenden
Prüfungstag **sabotieren**: Status umstellen, Reihenfolge/Zeiten verändern oder
falsche Daten an **alle** Anzeigen (Beamer + Schülerhandys) senden.

**Mögliche Maßnahmen (zunehmend sicher):**
1. **Sofort/billig (nur abschreckend, kein echter Schutz):** Schlüssel weniger
   offensichtlich; Master-URL nicht teilen; Beamer-/Steuer-Adresse verbergen (#7).
   → Hilft nur gegen Zufallsfunde, nicht gegen jemanden, der den Code anschaut.
2. **Datenbank absichern (Supabase):** Row-Level-Security so, dass **Schreiben**
   nur mit einem Geheimnis geht, das **nicht** im Client liegt – z. B. Schreibpfad
   über eine **Edge Function** mit serverseitigem Schlüssel; Clients dürfen nur lesen.
3. **Echte Anmeldung:** Master-Steuerung hinter **Login** (Supabase Auth / Passwort),
   Anzeige bleibt offen. Erfordert Aufteilung der App (#8).

**Empfehlung:** Mindestens Punkt 2 (Schreibrecht serverseitig schützen), damit nicht
jeder die Live-Daten überschreiben kann. Punkt 3 für vollen Schutz.
- Risiko der Umsetzung: mittel–hoch; hängt mit #8 (Aufteilung) zusammen.
- Priorität: **hoch**, sobald die App über den internen Gebrauch hinaus genutzt wird.

### 10. Plan beim „Steuerung übernehmen" aus der Cloud laden (Übergabe/Vertretung)
**Kurz gesagt:** Ein zweites Master-Gerät soll die Schülerliste **sofort sehen**, ohne erst die
Datei laden zu müssen — reine Sichtbarkeit/Komfort, keine Rechtefrage.

**Problem:** Der Plan/​die Schülerliste liegt pro Gerät lokal. Öffnet ein zweites Gerät den
Master-Link (z. B. Vertretung, Ersatz-Laptop), sieht es die leere Startseite — die Liste
erscheint erst, wenn dieses Gerät den Prüfungstag selbst lädt.

**Idee:** Der Live-Payload enthält inzwischen die **komplette** Einteilung (sched, Zeiten,
Kommission, Status). Beim Klick auf **„Steuerung übernehmen"** (oder beim Master-Start ohne
eigenen Plan) könnte das Gerät den **aktuellen Plan aus der Cloud rekonstruieren** (wie es die
Anzeige-Ansicht bereits tut) und als CUR übernehmen → sofortige, nahtlose Übergabe.

- Nutzen: echte Übergabe/Vertretung ohne Datei-Hantieren; Ausfallsicherheit.
- Risiko: mittel (Master müsste Payload → CUR/MATRIX zurückbauen; Konflikt mit lokalem Stand klären).
- Priorität: mittel–hoch (sehr praktisch am Prüfungstag).

### 11. Übernahme nur mit Bestätigung des aktuellen Masters (Schutz vor versehentlichem Kapern)
**Heute:** Eine Übernahme ist **immer erzwingbar** — wer „Steuerung übernehmen" klickt (oder
eine Steueraktion macht), wird sofort Master.

**Wunsch:** Solange jemand wirklich steuert, soll eine Übernahme **eine Zustimmung** des
aktuellen Masters brauchen (kein versehentliches/fremdes Kapern).

**Knackpunkt:** Ist der aktuelle Master **abgestürzt**, kann er nicht zustimmen → sonst Sackgasse.

**Saubere Lösung (nutzt den vorhandenen Heartbeat):**
- **Master lebt** (Heartbeat frisch): Übernahme-Anfrage → der aktuelle Master sieht
  „Gerät X will übernehmen — erlauben?" und bestätigt/lehnt ab.
- **Master tot** (kein Heartbeat > Zeitlimit, z. B. 9 s): Übernahme **sofort erlaubt**, ohne
  Bestätigung (niemand da, der zustimmen könnte).
- Konkret: der aktuelle Master bekommt einen **eigenen „Übergabe erlauben"-Knopf**; erst nach
  dessen Klick wird das anfragende Gerät Master. Ohne Heartbeat entfällt der Knopf → direkt übernehmbar.

- Nutzen: kein versehentliches Überschreiben bei laufender Steuerung; trotzdem kein Deadlock.
- Risiko: mittel (bidirektionale Anfrage/Bestätigung über den Payload; mehr Zustände).
- Hinweis: Erhöht die Komplexität spürbar — nur umsetzen, wenn Mehrfach-Geräte real ein Problem werden.

### 12. Excel-Export: Rahmen um die erste Zeile (Prüfungstag-Export)
Beim exportierten Prüfungstag fehlt der ersten Zeile (Titel-/Kopfzeile) ein **Rahmen**.
- Idee: erste Zeile des Export-Blatts mit Rahmen/Border versehen (sauberer Druck/Optik).
- Risiko: gering (nur Export-Formatierung in ExcelJS).

### 13. Live-Nummern stabil oder manuell vergebbar
Beim „in Vorbereitung schicken" ändert sich durch die chronologische Sortierung die **#-Nummer**.
- Idee A: Nummer **nicht** ändern, wenn jemand in Vorbereitung geht (stabile Plan-Nummer behalten).
- Idee B: Möglichkeit, **in Vorbereitung Nummern manuell zu vergeben / zu sortieren** (eigene Reihenfolge).
- Risiko: gering–mittel (Anzeige-/Sortierlogik; #-Spalte von der Sortierung entkoppeln).

### 14. ETA / voraussichtliche Startzeit am Beamer (ohne Layout zu verschieben)
Nach dem Setzen der **ersten Vorbereitung** und nach **Abschluss jeder Prüfung** Zeit & **ETA**
neu berechnen und am Beamer die **voraussichtliche Startzeit pro Schüler** anzeigen.
- **Wichtig:** Der Anzeigebereich darf **nicht nach unten wandern** — es muss alles am Beamer
  sichtbar bleiben.
- Layout-Idee: die **zwei linken Karten breiter** machen (Platz für die ETA in einer **neuen Zeile**),
  die **rechten schmaler** → asymmetrische 4-Karten-Aufteilung (links etwas breiter).
- Risiko: mittel (ETA-Berechnung aus laufenden Zeiten + Layout-Umbau, Höhe konstant halten).

### 15. Prüfungszeit mit „+" erhöhen, wenn zu spät aktiviert
Wird ein/e Kandidat/in **zu spät** in die Prüfung geschickt (Aktivieren vergessen, läuft real schon),
soll man die **Prüfungszeit nachträglich mit „+"** erhöhen können (rückwirkender Start/Dauer-Korrektur),
damit die echte Prüfungsdauer/ETA wieder stimmt.
- Risiko: gering–mittel (examAt rückdatieren bzw. Dauer-Offset; Auswirkung auf ETA/Folgezeiten beachten).

---

## Bewusst NICHT umgesetzt (mit Begründung)

- **Harte Sperre gegen doppelten Namen in Prüfung:** könnte Korrekturen/Rückgängig
  blockieren; das Tool ist menschengesteuert. Falls überhaupt, nur als sanfter Hinweis
  (siehe Offen #4).

---

## Bereits umgesetzt (Kurzüberblick)

- Stabile, zuverlässige Klicks (Event-Delegation, kein „Hängen").
- Live-Tabelle chronologisch sortiert; korrekte Klick-Indizes trotz Sortierung.
- Gestartete/fertige Prüfungen werden festgenagelt – keine Verschiebungs-Kaskade auf
  bereits laufende/erledigte Prüfungen; nur Wartende verschieben sich.
- „Zurück"-Knopf bei Fehlklick (Prüfung → Vorbereitung → wartet).
- Fertige Prüfungen zeigen die echte Dauer (Beginn → „Fertig").
- Voller Live-Stand übersteht Absturz/Reload (localStorage-Snapshot + Wiederherstellen),
  „Live zurücksetzen"-Knopf.
- Dauerhafter Beamer-Link (ohne Code) getrennt vom rotierenden Schüler-QR (mit Code);
  alte Schüler-Links laufen ab.
- Beamer-Ansicht nur bei eingeschaltetem Live-Tag-Modus.
