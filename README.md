# stellenradar-import

Übergabe zwischen Grok und Claude für Fabians Stellenradar (seit 15.11.2026).

## Zeitplan (gültig ab 20.11.2026, Europe/Zurich)

| Zeit | Wer | Datei | Dauer bisher |
|---|---|---|---|
| 00:30 | Grok «Radar 1 – Stellen finden» | schreibt `eingang/finden_<JJJJ-MM-TT>.txt` (bei > 150 KB `_teil1.txt`, `_teil2.txt`, …; `#END` im letzten Teil) | ca. 50–60 Min. |
| 02:30, 04:30 | Grok «Radar 1» – Nachlauf | nur falls der Lauf von 00:30 abgebrochen ist: macht ab der gespeicherten Stelle weiter, sonst nichts | – |
| 02:30 | Grok «Radar 3 – weitere Quellen» | schreibt `eingang/extern_<JJJJ-MM-TT>.txt` (LinkedIn, jobs.ch, Indeed) | ca. 30–40 Min. (fertig ~03:10) |
| **05:45** | Claude «Stellenradar: suchen und Inserate lesen» | importiert `finden_…` (alle Teile!) in den Radar, schreibt `eingang/zu_lesen_<JJJJ-MM-TT>.json` | – |
| **06:15** | Grok «Radar 2 – Inserate lesen» | liest `zu_lesen_…`, schreibt `eingang/urteile_<JJJJ-MM-TT>.json` | ca. 1 Std. bei ~1'700 Stellen |
| **08:00** | Claude «Stellenradar: Grok-Urteile übernehmen» | übernimmt `urteile_…` und `extern_…`, **erst danach** alle Dateien nach `archiv/` verschieben | – |
| **08:30** | Claude «Stellenradar: Alert-Mails einlesen und Inserate lesen» | bestehende Aufgabe | – |

Wichtig für Claude:
- Das Archivieren lief bisher schon um **06:46** (Commits «Archiv …»), also vor Radar 2 und vor dem Übernehmen der Urteile. Archivieren bitte erst im Schritt um 08:00.
- `zu_lesen_<Datum>.json` wurde bisher nie geschrieben, weil `finden_…` um 06:40 noch fehlte oder in Teilen vorlag. Bitte `finden_<Datum>.txt` **und** `finden_<Datum>_teil*.txt` einlesen.
- Fehlt `finden_…` um 05:45, bitte nicht abbrechen, sondern einmal um 06:00 erneut prüfen (Radar-1-Nachlauf um 04:30 kann bis ca. 05:45 dauern).

- `auftraege/`: die gültigen Grok-Aufträge. Grok liest sie bei jedem Lauf aus dem Repository; Änderungen hier wirken ab dem nächsten Lauf, ohne dass die Automation in Grok angepasst wird.
  - `radar1_stellen_finden.txt`, `radar2_inserate_lesen.txt`, `radar3_weitere_quellen.txt` – Versionsnummer steht in der ersten Zeile.
  - Vor jeder Änderung wird die bisherige Fassung als `auftraege/archiv/<name>_v<N>.txt` abgelegt.
- Fehlt eine Grok-Datei, arbeitet Claude wie früher selbst (Rückfall).
- Radar: https://claude.ai/artifact/7hSYRJjdcohBjct9yYZ3RL
