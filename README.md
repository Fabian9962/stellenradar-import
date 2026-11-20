# stellenradar-import

Übergabe zwischen Grok und Claude für Fabians Stellenradar (seit 15.11.2026).

## Zeitplan (abgestimmt am 20.11.2026, gültig ab 21.11.2026, Europe/Zurich)

| Zeit | Wer | Datei |
|---|---|---|
| 00:30 (Nachläufe 02:30, 04:30) | Grok «Radar 1 – Stellen finden» | schreibt `eingang/finden_<JJJJ-MM-TT>.txt` (bei > 150 KB `_teil1.txt`, `_teil2.txt`, …; `#END` im letzten Teil). Muss bis 05:45 vollständig sein. |
| 02:30 | Grok «Radar 3 – weitere Quellen» | schreibt `eingang/extern_<JJJJ-MM-TT>.txt` (LinkedIn, jobs.ch, Indeed) |
| **04:50 und 05:50** | Claude «Stellenradar: Grok-Liste importieren (zu_lesen)» – **neu, läuft in der Cloud, ohne Fabians Rechner** | importiert `finden_…` (alle Teile) in den Radar und schreibt `eingang/zu_lesen_<JJJJ-MM-TT>.json`. Der zweite Lauf tut nichts, wenn die Datei schon da ist. |
| **06:15** | Grok «Radar 2 – Inserate lesen» (Auftrag v4) | liest nur `zu_lesen_…` (wartet bis 07:30, falls sie fehlt), schreibt `eingang/urteile_<JJJJ-MM-TT>.json` in Zwischenständen alle 25 Stellen |
| **08:15 und 11:15** | Claude «Stellenradar: Grok-Urteile übernehmen» | übernimmt den jeweiligen Stand von `urteile_…` und `extern_…`; archiviert erst um 11:15 und nur, was übernommen ist. `finden_<heute>` bleibt bis zum nächsten Tag liegen. |
| **13:31** | Claude «Stellenradar: suchen und Inserate lesen» (braucht Fabians Rechner) | RAV-Abgleich; liest selbst, was Grok nicht beurteilt hat. Ist der Rechner aus, fällt nur dieser Lauf aus. |

Warum es bis zum 20.11. nicht ging: `zu_lesen_…` sollte die Aufgabe um 06:40 schreiben. Sie braucht Fabians Rechner (RAV-Liste) und wurde bei ausgeschaltetem Rechner ausgesetzt; zudem lief sie nach Radar 2. Deshalb fehlte Radar 2 jeden Tag die Liste. Das Archivieren um 06:46 kam ausserdem vor den Urteilen.

- `auftraege/`: die gültigen Grok-Aufträge. Grok liest sie bei jedem Lauf aus dem Repository; Änderungen hier wirken ab dem nächsten Lauf, ohne dass die Automation in Grok angepasst wird.
  - `radar1_stellen_finden.txt`, `radar2_inserate_lesen.txt`, `radar3_weitere_quellen.txt` – Versionsnummer steht in der ersten Zeile.
  - Vor jeder Änderung wird die bisherige Fassung als `auftraege/archiv/<name>_v<N>.txt` abgelegt.
- Fehlt eine Grok-Datei, arbeitet Claude wie früher selbst (Rückfall).
- Radar: https://claude.ai/artifact/7hSYRJjdcohBjct9yYZ3RL
