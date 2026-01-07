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

- Effizienz
    - Warten auf EK/CT verhindert senden von Sachen bei Kapazität
    - Durch Betrachtung von ML-KEM auf Optimierung kommen
- ML-KEM
    - Grob
        - A -> B: EK
        - B -> A: CT (Secret mit EK verschlüsselt)
        - A entschlüsselt mit DK und erhält das Secret
    - CT Erstellung genauer:
        - S und R erstellen und mit EK-Hash kombinieren
        - EK hashen
        - Seed sind 32 bit vom EK
        - Seed + R -> Großteil von CT
        - S + EK -> Letzter Teil von CT
        - ==> Senden von EK Hash und Seed
    - Neuer Ablauf:
        - EK + DK + Seed
        - A -> B: EK1 (Seed + Hash)
        - B Erstellung von CT1 (Großteil von CT)
        - A -> B: EK2 (EK - Seed) und B -> A: CT1
        - B erstellt CT2 mit EK2
        - Wenn CT1 ack: B -> A: CT2
        - --> Epoche erhöhen

- Triple Ratchet
    - Double + dazu SPQR und Keys mixen
    - Senden einer Nachricht:
        - Beide Protokolle nach Verschlüsselungs-Key + Header Info
        - Key Derivation Function ("random input" -> Kryptografischer Key beliebiger Länge)
        - Angreifer müssen EC + ML-KEM brechen und dann noch Key von Random Bits unterscheiden
        - Nutzen von Mixed Key zum Verschlüsseln
    - Entschlüsseln einer Nachricht:
        - Header nutzen und Protokolle nach Entschlüsselungs-Key fragen
        - Beide Schlüssel in die Key Derivation Function geben
        - Nutzen des Entschlüsselungs-Keys


- Rollout
    - Downgrade möglich
        - A -> B: Message + SPQR Data ohne Key
        - B kann entschlüsseln auch wenn die Daten nicht verstanden werden
        - B -> A: Antwort ohne SPQR Data
        - A weiß jetzt das B kein SPQR versteht
    - SPQR Data so mit der Nachricht verbinden, dass es nicht einfach entfernt werden kann
        - SPQR data attached to the message be MAC’d by the message-wide authentication code
    - Downgrade nur am Start möglich (Erhalt der ersten Nachricht/Antwort)
    - Zukünftig Code Änderung sodass nur mit SPQR kommuniziert wird -> Archivieren der vorherigen Verläufe
    - Mechanismus funktioniert auch für zukünftigere Versionen von SPQR

## Problematik


## Herleitung ML-KEM Braid



## Triple Ratchet


