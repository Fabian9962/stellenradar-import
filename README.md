# stellenradar-import

Übergabe zwischen Grok und Claude für Fabians Stellenradar (seit 15.11.2026).

| Zeit (Europe/Zurich) | Wer | Datei |
|---|---|---|
| 05:30 | Grok «Radar 3 – weitere Quellen» | schreibt `eingang/extern_<JJJJ-MM-TT>.txt` (LinkedIn, jobs.ch, Indeed) |
| 06:00 | Grok «Radar 1 – Stellen finden» | schreibt `eingang/finden_<JJJJ-MM-TT>.txt` (bei grossen Listen `_teil1.txt`, `_teil2.txt`, …) |
| 06:40 | Claude «Stellenradar: suchen und Inserate lesen» | importiert die Liste in den Radar, schreibt `eingang/zu_lesen_<JJJJ-MM-TT>.json` |
| 07:00 | Grok «Radar 2 – Inserate lesen» | liest die Inserate, schreibt `eingang/urteile_<JJJJ-MM-TT>.json` |
| 07:45 | Claude «Stellenradar: Grok-Urteile übernehmen» | schreibt die Urteile in den Radar, übernimmt `extern_…` in die Sammlung «extern», verschiebt alle Dateien nach `archiv/` |
| 08:10 | Claude «Stellenradar: Alert-Mails einlesen und Inserate lesen» | liest die Inserate der neuen LinkedIn-/jobs.ch-/Indeed-Stellen (bestehende Aufgabe) |

- `auftraege/`: die gültigen Grok-Aufträge. Grok liest sie bei jedem Lauf aus dem Repository; Änderungen hier wirken ab dem nächsten Lauf, ohne dass die Automation in Grok angepasst wird.
  - `radar1_stellen_finden.txt`, `radar2_inserate_lesen.txt`, `radar3_weitere_quellen.txt` – Versionsnummer steht in der ersten Zeile.
  - Vor jeder Änderung wird die bisherige Fassung als `auftraege/archiv/<name>_v<N>.txt` abgelegt.
- Fehlt eine Grok-Datei, arbeitet Claude wie früher selbst (Rückfall).
- Radar: https://claude.ai/artifact/7hSYRJjdcohBjct9yYZ3RL
