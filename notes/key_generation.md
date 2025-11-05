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
