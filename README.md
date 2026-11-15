# stellenradar-import

Übergabe zwischen Grok und Claude für Fabians Stellenradar (seit 15.11.2026).

| Zeit (Europe/Zurich) | Wer | Datei |
|---|---|---|
| 06:00 | Grok «Radar 1 – Stellen finden» | schreibt `eingang/finden_<JJJJ-MM-TT>.txt` (bei grossen Listen `_teil1.txt`, `_teil2.txt`, …) |
| 06:40 | Claude «Stellenradar: suchen und Inserate lesen» | importiert die Liste in den Radar, schreibt `eingang/zu_lesen_<JJJJ-MM-TT>.json` |
| 07:00 | Grok «Radar 2 – Inserate lesen» | liest die Inserate, schreibt `eingang/urteile_<JJJJ-MM-TT>.json` |
| 07:45 | Claude «Stellenradar: Grok-Urteile übernehmen» | schreibt die Urteile in den Radar, verschiebt alle Dateien nach `archiv/` |

- `auftraege/`: die Grok-Aufträge, je Änderung eine neue Versionsdatei; ältere Versionen nach `auftraege/archiv/`.
- Fehlt eine Grok-Datei, arbeitet Claude wie früher selbst (Rückfall).
- Radar: https://claude.ai/artifact/7hSYRJjdcohBjct9yYZ3RL
