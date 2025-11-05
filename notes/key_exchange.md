# Schlüsselübertragung

## Herleitung im Blogbeitrag [connellSignalProtocolPostQuantum2025]

- Aktuell regelmäßig neue ECDH Secrets (32 bytes) für Ratchet
- Durch mixen (einkapseln) von PQXDH mit KEM jede Nachricht über 1000 bytes


* Naiver Ansatz
    - A jedes Senden + EK
    - B jedes Erhalten CT + Secret
    - A jedes Erhalten extrahieren + mixen
* Problematik bei Offline/Verloren/Neuangefordert/nicht 1:1 von Senden und Empfangen

- State Machine
    - ...
- Mehrfaches senden von großen Nachrichten (EK und CT)


* Bandbreiten-Kosten reduzieren
    - Nicht bei jeder Nachricht ist problematisch mit Offline/Verlust
    - Angriffsmölichkeit wenn nur ab und zu eine große Nachricht kommt
* Chuck Up
    - Durchrotierend senden (Wartezeit bei Verlust ist unschön)
    - Angriffsmöglichkeit bei gezieltem Verlust
* Erasure Code umgeht Problematik
    - Nach erstmaligem durchrotieren neue Chucks erstellen
    - Nach erhalt bestimmter Anzahl an Chucks ist die Nachricht beim Empfänger zusammenstellbar
    - Nur DOS hält vollständiges Senden auf (bemerkbar)


- Epochen handling/Nutzen von freier Kapazität
    - Bei zu schnellem Senden müssen mehrere Schlüssel aufbewart werden (Angriffsmöglichkeit)
    - Mehrere Simulationen bei Signal durchgeführt (Vergleich von der Anzahl anfälliger Nachrichten)
        - Ergebnis: Kein EK#2 senden vor Erhalt von CT#1


## Problematik


## Herleitung ML-KEM Braid



## Triple Ratchet


