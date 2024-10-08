---
title: Série 1
draft: false
tags:
  - IoT
  - Exercice
---

# Exercice 1

## a) Modes de transmission
Les principaux modes de transmission sont **unicast**, **multicast**, et **broadcast**.

## b) Calcul du temps de transfert d'une image
### Données :
- Taille de l'image : $1920 \times 1080 \times 3 = 6\,220\,800 \text{ octets} \approx 6.22 \text{ MB}$

### Connexion DSL :
- Débit : 16 Mbps = $16 \times 10^6$ bits/s
- Temps de transfert = $\frac{6.22 \times 8}{16 \times 10^6} \approx 3.11$ s

### Connexion Ethernet :
- Débit : 1 Gbps = $10^9$ bits/s
- Temps de transfert = $\frac{6.22 \times 8}{10^9} \approx 0.05$ s

## c) Avantages et inconvénients des réseaux filaires et sans fil

- **Avantages des réseaux filaires** : Plus rapides et stables.
- **Inconvénients des réseaux filaires** : Moins pratiques, car nécessitent des câbles.

## d) Stabilité et capacité des réseaux filaires vs sans fil

- Les réseaux **filaires** sont moins sensibles aux interférences et offrent une plus grande capacité.
- Les réseaux **sans fil** offrent une mobilité accrue mais souffrent de plus d'interférences et de capacités moindres.

---

# Exercice 2

## a) Différences entre LAN, MAN et WAN

- **LAN (Local Area Network)** : Couvre une petite zone géographique, généralement un bâtiment ou un groupe de bâtiments (ex : réseau domestique ou d’entreprise).
- **MAN (Metropolitan Area Network)** : Couvre une grande agglomération ou une ville, reliant plusieurs LANs.
- **WAN (Wide Area Network)** : Couvre de vastes zones géographiques (ex : Internet).

## b) Pertinence du terme LAN dans le futur

La distinction entre ces types de réseaux devient floue avec la **croissance des technologies sans fil** et des **infrastructures cloud**, facilitant l'extension des LANs en WANs via Internet.

Cependant, le terme **LAN** restera pertinent car il désigne une topologie spécifique avec des caractéristiques propres : proximité physique, faible latence et haute vitesse.
