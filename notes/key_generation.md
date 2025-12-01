# Key Generation
## Quantum Resistance and the Signal Protocol
`@kretQuantumResistanceSignal`

- erster Schritt in Richtung Quantum-Resistenz
    - Upgrade von [X3DH](https://signal.org/docs/specifications/x3dh/) zu PQXDH
- zusätzliche Schicht
- Entgegenwirken von _Harvest Now, Decrypt Later_ Attacken
- [Whitepaper](#the-pqxdh-key-agreement-protocol)

<br>

- neue _One-Way Functions_, welche nicht einfach von QC invertierbar sind
    - [NIST Standardization Process for Post-Quantum Cryptography](https://csrc.nist.gov/projects/post-quantum-cryptography)
    - Kandidaten teilweise angreifbar durch klassische Computer $\to$ Vorsicht bei Integration
- Entscheidung für [**CRYSTALS-Kyber**](https://pq-crystals.org/kyber/) _Key-Encapsulation Mechanismus_
- Erweiterung von Elliptic-Curve (X25519) $\to$ beide Systeme müssen geknackt werden
- Kombination der _Shared Secrets_ beider Algorithmen


## The PQXDH Key Agreement Protocol
`@kretPQXDHKeyAgreement2024`

- PQXDH etabliert _Shared Secret_ zwischen zwei Partien, welche sich gegenseitig durch Public Keys authentifizieren
- Quantum-sichere _Forward Secrecy_, kryptografische _Deniability_
- immer noch basierend auf Schwierigkeit des Diskreten-Logarithmen-Problems für gegenseitige Authentifizierung
- asynchron, Offline-Verfügbarkeit

### Parameter

| Parameter | Definition |
|-----------|------------|
| `curve` | Art der Elliptic Curve, bspw. `curve25519` |
| `hash` | Hash-Funktion, bspw. `SHA-512` |
| `info` | ASCII-String zur Identifikation der Anwendung, bspw. `MyProtocol` |
| `pqkem` | Post-Quantum Key Encapsulation Mechanism, bspw. `CRYSTALS-Kyber-1024` |
| `aed` | Schema für _Authenticated Encryption with Associated Data_ |
| `EncodeEC` | Funktion, die `curve`-Public-Key in Byte-Sequenz verschlüsselt |
| `DecodeEC` | Funktion, die Byte-Sequenz in `curve`-Public-Key entschlüsselt, Inverses von `EncodeEC` |
| `EncodeKEM` | Funktion, die `pqkem`-PK in Byte-Sequenz verschlüsselt |
| `DecodeKEM` | Funktion, die Byte-Sequenz in `pqkem`-PK entschlüsselt, Inverses von `EncodeKEM` |

### Rollen

- Parteien: Alice, Bob und Server
- Alice
    - Senden von initialen, verschlüsselten Daten an Bob
    - Etablierung eines Shared Key $\to$ Verwendung für bidirektionale Kommunikation
- Bob
    - möchte es Parteien wie Alice erlauben, dass SK etabliert und verschlüsselte Daten gesendet werden
    - ggf. offline, wenn Alice dies versucht $\to$ Beziehung zu einem Server
- Server
    - Speicherung von Nachrichten von Alice an Bob
        - späterer Abruf durch Bob
    - Veröffentlichung von Daten von Bob, welche Alice zur Verfügung gestellt werden
    - ggf. Aufteilung in mehrere Komponenten

### Notation

- Verkettung von Byte-Sequenzen $X, Y$ ist $X || Y$
    - hier: $X \circ Y$
- $DH(PK_1,PK_2)$: Shared Secret, Output von Diffie-Hellman-Funktion (je nach `curve`-Parameter)
- $Sig(PK,M,Z)$: `curve` XEdDSA-Signatur der Byte-Sequenz $M$, welche mit $PK$'s Private Key erstellt wurde unter Verwendung von 64B von $Z$-_Randomness_
- $KDF(KM)$: 32B Output vom HKDF-Algorithmus (_Key Derivation Function_)
    - verschiedene Parameter von HKDF erklärt
- $(CT,SS) = PQKEM-ENC(PK)$: KEM-Ciphertext aus `pqkem`-Algorithmus und Shared Secret, welche im CT durch Public Key eingebettet ist
- $PQKEM-DEC(PK,CT)$: dekodiertes Shared Secret $SS$
    - Verwendung vom Private Key $PK$, welcher zum Public Key passt, mit welchem $CT$ verschlüsselt wurde

#### Elliptic Curve Keys

- $IK_A$, $IK_B$: Identity Key von Alice / Bob
    - Long-Term Identity Elliptic Curve Public Key
- $EK_A$: _Ephemeral Key_ (public) von Alice
    - neue Generierung für jeden _Protocol Run_
- $SPK_B$: _Signed Prekey_ von Bob
    - regelmäßiger Wechsel
    - Signieren mit $IK_B$
    - Veröffentlichung auf Server zusammen mit Identifier $IdEC$
- $(OPK_B^1, OPK_B^2, \ldots)$: Menge an _One-Time Prekeys_ von Bob
    - Verwendung in jeder einzelnen PQXDH-Protokoll-Runde
- $IdEC(K)$: eindeutiger Identifier für Keys ($SPK$ oder $OPK$)
    - Speicherung auf Bobs Gerät

#### Post-Quantum Key Encapsulation Keys

- $(PQOPK_B^1, PQOPK_B^2, \ldots)$: Bobs Menge an signierten One-Time `pqkem` Pre-Keys
    - signiert mit $IK_B$
    - Verwendung in jeder Protokoll-Runde
- $PQSPK_B$: Bobs signierter _last-resort_ `pqkem` Pre-Key
    - periodische Änderung
    - signiert mit $IK_B$
    - wird nur verwendet, wenn $PQOPK_B$ nicht verfügbar
        - bspw. wenn mehr Keys als hochgeladen abgefragt werden
- $IdKEM(K)$: Identifier für jeden $PQSPK$ bzw. Ephemeral KEM-Key
    - zur eindeutigen Identifikation des jeweiligen Keys auf Bobs Gerät
- Speicherung der Keys + Identifier auf Server

### PQXDH-Protokoll

**3 Phasen**
- Bob veröffentlicht EC Identity-Key, EC Prekeys & `pqkem`-Prekeys auf Server
- Alice ruft _Prekey Bundle_ vom Server ab; verwendet diese für initiale Nachricht an Bob
- Bob empfängt und verarbeitet initiale Nachricht von Alice

#### I. Veröffentlichung der Keys

- Bob generiert 64 Byte zufällige Werte $Z_{\text{SPK}}, Z_{\text{PQSPK}}, Z_1, Z_2, \ldots$
- Veröffentlichung von verschiedenen Keys
    - siehe Paper
- Identity Key $IK_B$ wird nur einmal hochgeladen
- neue One-Time Prekeys jederzeit möglich
    - bspw. wenn Bob vom Server informiert wird, dass nur noch wenige vorliegen
- neuer signierter `curve`-Prekey bzw. signierter Last-Resort-`pqkem`-Prekey in bestimmten Intervall
    - wieder mit $IK_B$ signiert
    - neue Prekeys ersetzen vorherige Werte
    - entsprechender Private Key für bestimmte Zeit behalten zur Entschlüsselung von ggf. verspäteten Nachrichten; schließlich Löschen für _Forward Secrecy_
- Private Keys für One-Time-Prekey werden gelöscht, sobald Nachricht empfangen wurde, welche diesen benutzt

#### II: Initiale Nachricht Senden

- für _PQXDH Key Agreement_ mit Bob sind Prekeys notwendig
- Alice kontaktiert Server für _Prekey Bundle_
    - siehe Paper
    - bei Bereitstellung von `curve` One-Time Prekey löscht Server diesen anschließend
    - wenn kein `curve` OPK auf Server mehr vorhanden, dann enthält Prekey Bundle keinen
    - Bereitstellung von `pqkem` One-Time Signed Prekeys $PKOPK_B^n$, falls vorhanden
        - anschließend löschen
        - falls nicht vorhanden, dann `pqkem` Last-Resort Signed Prekey $PQSPK_B$
- Alice verifiziert Signaturen der Prekeys
    - Abbruch des Protokolls falls eine Prüfung fehlschlägt
- Generierung eines Ephemeral `curve` Schlüsselpaares mit Public Key $EK_A$
- Generierung eines `pqkem` _Encapsulated Shared Secret_
    - $(CT,SS) = PQKEM-ENC(PQPK_B)$
        - Shared Secret $SS$
        - Ciphertext $CT$
- falls Bundle keinen `curve` OPK enthält, dann
    - $DH_1 = DH(IK_A, SPK_B)$
    - $DH_1 = DH(EK_A, IK_B)$
    - $DH_1 = DH(EK_A, SPK_B)$
    - $SK = KDF(DH_1 \circ DH_2 \circ DH_3 \circ SS)$
- andernfalls:
    - $DH_4 = DH(EK_A, OPK_B)$
    - $SK = KDF(DH_1 \circ DH_2 \circ DH_3 \circ \mathbf{DH_4} \circ SS)$
- nach Berechnung von $SK$: Löschung des Ephemeral `curve` Private Keys, die $DH$-Ergebnisse und das Shared Secret $SS$
- Berechnung einer _Associated Data_ Bytesequenz $AD$
    - enthält Identitätsinformationen beider Partien
    - $AD = EncodeEC(IK_A) \circ EncodeEC(IK_B)$
    - falls `pqkem` $PKPK_B$ _nicht_ in Ciphertext verarbeitet, muss Alice $EncodeKEM(PQPK_B)$ an $AD$ dranhängen
    - optional: weitere Informationen anhängen, bspw. Username, Zertifikate oder andere Identifikationsmittel
- Alice sende initiale Nachricht an Bob inkl. verschiedener Keys
    - siehe Paper
- anschließend Löschen vom Ciphertext $CT$
- ggf. Weiterverwendung von $SK$ oder davon abgeleiteten Keys

### III. Empfang der initialen Nachricht

- Erhalten von Alices initialer Nachricht
- Bob ruft Alices Identity Key und Ephemeral Key aus Nachricht ab
- Laden des eigenen Identity Private Keys
- Verwendung der Key-Identifier zum Laden der Private Keys, welche zu den signierten Prekeys, One-Time Prekeys und KEM-Key passen, welche von Alice verwendet wurden
- Berechnung des Shared Secrets $SS = PQKEM-DEC(PQPK_B, CT)$
- Wiederholung der $DH$- und $KDF$-Berechnungen aus II zur Ableitung von $SK$ und $AD$
- Abbruch falls Entschlüsselung von $CT$ fehlschlägt, Löschcen von $SK$
- erfolgreiche Entschlüsselung: Abschluss des Protokolls
    - Löschen von $CT$ und allen One-Time Private Prekeys, welche verwendet wurden $\to$ Forward Secrecy
    - ggf. Weiterverwendung von $SK$ oder davon abgeleiteten Keys
