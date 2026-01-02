# Notizen
## Gliederung
- Einleitung  (5 min)
    - Motivation,  (weite Verbreitung) ohne Ecliptic curve nur Verweis auf Vorlesung, Angriffe nur nennen und nicht erklären 
    - _ganz_ kurz, um abzuholen, anderer Vortrag direkt vor uns
- PQXDH für Secret-Erstellung (15 min)
    - CRYSTALS-Kyber KEM $\to$ sollte mit drin sein
        - eher oben einsparen
- Schlüsselübertragung (20 min)
    - Problematik
    - Herleitung ML-KEM Braid
    - Triple Ratchet
- Rollout (5 min)
- Zusammenfassung (2 min)

## Hinweise
- Ratchet: Computerphile-Videos
- Aufteilung
    - Max: Angriffsmöglichkeiten Quanten-Computing, CRYSTALS-Kyber
    - Ines: Schlüsselübertragung
    - Nico: PQXDH
- Vorgehensweise
    - Notizen in Markdown über git
    - Folien _händisch_ erstellen und ins git hochladen
    - später zusammen Folien erstellen

## Plan für Einführung von PQ-Sicherheit
`@gentyalaQuantumResistanceSignal2025`

- I. Absicherung des initialen Handshakes mittels PQXDH
    - hybrider Ansatz: Kombination von Ecliptic Curve Key Agreement und PQ KEM (CRYSTALS-Kyber)
    - beides muss gebrochen werden
- II. SPQR für PQ-sichere Forward Secrecy and Post-Compromise Security
    - Double Ratchet $\to$ Triple Ratchet
