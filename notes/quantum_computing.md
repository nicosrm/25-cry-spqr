https://arxiv.org/pdf/2403.11741

Probleme die schwer zu lösen sind, ihn fragen was es noch für probleme gibt
- integer factorization
- discrete logarithms
eliptische kurven?


Shor's Algorithm löst beides

RSA, ECC -> integer factorization, discrete logarithms -> Shor's Algorithm löst beides

RSA: factoring large composite
large integers exponentially faster with shor's

ECC Elliptic Curve Cryptography nutzt: elliptic curve discrete logarithm
problem
Shor's Algorithm kann diskrete log berechnen und damit aus public key private key berechnen


Quantum computer sind:
- Qbits 0,1 und super position 0 udn 1
- können entangled/ verschränkt sein damit voneinander abhänig
- transformation über mehrer qbits auf einmal
- konstructiv interverenz und destruktive die wahrscheinlichkeit auf richtige ergebniss erhöhen

- bestehende kryptografische systeme wie RSA und eliptische kurven basieren darauf das die dahinterliegenden Probleme (...) schwer zu lösen sind

shor faktorisierung:
- nicht jeden faktor einzeln testen
- mehrere gleichzeitig
- richtiges ergebnis durch interverenz anzeigen



harvest now, decrypt later Problem selbst wenn Quantum computer nicht in realtime lösen kann trotzdem eine gefahr


Vortrag Inhalte:
- Qbits 0,1 und super position 0 udn 1
- können entangled/ verschränkt sein damit voneinander abhänig
- transformation über mehrer qbits auf einmal
- konstructiv interverenz und destruktive die wahrscheinlichkeit auf richtige ergebniss erhöhen

- bestehende kryptografische systeme wie RSA und eliptische kurven basieren darauf das die dahinterliegenden Probleme schwer zu lösen sind
- Es gibt algorythmen die schneller auf Quantencomputern laufen als auf herkömmlichen
- Damit sind die Probleme nicht mehr schwer zu lösen, solange ein hinreichend großer Quanten Computer zur Verfügung steht
- 22.10.2025 erstes Program nachgewiesen 13000 mal schneller als auf supercomputer. \cite{google2025observation}
- Dennoch weit entfernt für RSA-2048 braucht mehrere 1000 Logische Qbits und 20 Mio Physische für 8 Stunden\cite{gidney2021factor} 
- ECC-256 2330 Logical qibits 317 × 10^6 für 1 Stunde und 13 x 10^6 für 1 Tag\cite{roetteler2017quantum, webber2022impact}


- Ansätze basierne auf Shors algortihm
<!-- - ohne in die Tiefe zu gehen -->
- Quanten-Parallelität um die "Perioden" zu finden https://en.wikipedia.org/wiki/Hidden_subgroup_problem
- ECC benötigt sogar weniger Q-Bits





Folien:

Folie 1 – Quantencomputing: Grundidee (Einleitung)
- Qubits statt Bits
- Superposition: 0 und 1
- Verschränkung zwischen Qubits
- Gemeinsame Operationen
- Interferenz verstärkt Ergebnisse

Folie 2 – Bedeutung für Kryptografie
- RSA und ECC heute sicher
- Sicherheit durch Rechenaufwand
- Quantenalgorithmen schneller
- Probleme werden lösbar
- Abhängig von Rechnergröße

Folie 3 – Aktueller Stand der Technik
- Spezielle Quantenprogramme schneller
- 13.000× gegenüber Supercomputern
- Experimentelle Demonstration
- Noch keine Praxisangriffe
- Hardware stark limitiert

Folie 4 – Algorithmischer Kern & Ausblick
- Shors Algorithmus
- Periodenfindung
- Quanten-Parallelität
- Hoher Qubit-Bedarf
- Post-Quantum-Krypto nötig