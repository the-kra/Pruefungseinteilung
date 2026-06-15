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
