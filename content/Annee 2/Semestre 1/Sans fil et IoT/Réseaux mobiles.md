---
title: Réseaux mobiles
draft: false
tags:
  - IoT
---

# Réseaux cellulaires

Appareil -> BS (base station) -> BSC (base station controller) -> MSC (mobile switching center) -> HLR (Home location Register), VLR (Visitor Location Register), AC (Authentication Center), EIR (Equipment Identity Register)

## Architecture des réseaux cellulaires

1. **Appareil** : Le téléphone mobile ou l'appareil IoT.
2. **BS (Base Station)** : Antenne qui communique avec les appareils dans sa cellule.
3. **BSC (Base Station Controller)** : Contrôle plusieurs stations de base et gère la distribution du trafic.
4. **MSC (Mobile Switching Center)** : Centre de commutation mobile qui gère les communications entre les réseaux mobiles et fixes, ainsi que l'itinérance.
5. **HLR (Home Location Register)** : Base de données qui contient les informations des abonnés enregistrés dans le réseau (permanent).
6. **VLR (Visitor Location Register)** : Base de données temporaire pour les abonnés en itinérance dans la zone couverte par le MSC.
7. **AC (Authentication Center)** : Responsable de l'authentification des abonnés pour garantir la sécurité des communications.
8. **EIR (Equipment Identity Register)** : Base de données qui contient les identifiants uniques des équipements pour éviter l'utilisation de téléphones volés ou non conformes.


![[Pasted image 20240930171331.png]]

----
## Multiplexage spatial

- Les fréquences étant une ressource rare, on utilise le **multiplexage spatial** pour réutiliser la même fréquence dans plusieurs cellules distantes.
- La formule pour déterminer le nombre de cellules dans un arrangement hexagonal est : 
$$
K = i^{2} + j^{2}+ ij\qquad \forall \left(i,j\right) \in \mathbb{N}^{2}
$$
		où (i,j) sont des indices qui définissent la position des cellules voisins
### Placement des antennes

- Les antennes sont souvent placées à l'intersection de trois hexagones, formant des angles de **120⁰** pour maximiser la couverture et minimiser les interférences.

![[Drawing.jpg]]

## Multiplexage par répartition dans le temps

![[Pasted image 20241006151602.png]]

- **Fréquences (axe vertical)** :
    - Le graphique montre différentes fréquences porteuses (C0, C1, C2, Cn) utilisées dans le système TDMA. Chaque fréquence porteuse représente un canal de communication distinct dans le spectre de fréquences.

- **Temps (axe horizontal)** :
    - L'axe horizontal représente le temps, et les blocs rectangulaires correspondent aux intervalles de temps (slots). Le TDMA divise le signal en plusieurs intervalles de temps, permettant à plusieurs utilisateurs de partager le même canal de fréquence en leur attribuant différents intervalles de temps.

- **Trame TDMA** :
	- L'ensemble des intervalles de temps pour toutes les fréquences porteuses pendant une période donnée forme une trame TDMA. Dans le schéma, une trame TDMA s'étend de l'intervalle de temps 0 à 7.

- **Rectangles noirs** :
    - Les rectangles noirs montrent les intervalles de temps spécifiques utilisés par un appareil ou un utilisateur sur chaque fréquence porteuse. Par exemple, les blocs noirs sur C0 indiquent que cette porteuse est active pendant les intervalles de temps 0 et 3.

- **Communication en plein débit** :
    - Ce diagramme illustre une communication en plein débit, où l'utilisateur occupe un intervalle de temps complet sur une ou plusieurs fréquences porteuses pour transmettre ses données.

---
## Principes des réseaux mobiles

- **Réutilisation des fréquences** : La même fréquence peut être utilisée dans de nombreux sites cellulaires éloignés les uns des autres.
- **Expansion cellulaire** : Facilité d'ajout de nouvelles cellules pour augmenter la capacité du réseau.
- **Transfert (handover)** : Passage transparent d'une cellule à une autre lorsque l'appareil se déplace.
- **Itinérance (roaming)** : Permet à un appareil de se connecter à des réseaux autres que son réseau d'origine.

---

## Générations des réseaux mobiles
### Génération 1 (1G)

- **Technologie** : Analogique.
- **Problèmes** :
    - Incompatibilité entre les différents systèmes.
    - Pas d'itinérance internationale.
    - Faible capacité (peu d'abonnés pris en charge).

### Génération 2 (2G)

- **Technologie** : Numérique.
- **Avantages** :
    - Capacité accrue.
    - Sécurité renforcée.
    - Compatibilité entre systèmes.
    - Utilisation de **TDMA** (Time Division Multiple Access), **FDMA** (Frequency Division Multiple Access) et **SM** (Spatial Multiplexing) pour augmenter la capacité du réseau.

![[Pasted image 20240930174948.png]]
## De la 1G au 6G

• 1G : communication analogique ;

• 2G : communication numérique sous forme circuit ;

• 3G : communication sous forme paquet, sauf pour la parole téléphonique

• 4G : communication multimédia sous forme paquet a très haut débit

• 5G : communication et services multimédias avec de très forts débits en mobilité, la
connexion de milliards d’objets et la prise en charge de missions critiques ;

• 6G : infrastructure numérique avec un service réseau et télécommunication.

----
## Bruit thermique

Le **bruit thermique** est une source de bruit dans les réseaux de télécommunication, et sa puissance peut être calculée à partir de la formule suivante :
$$
\begin{align} P = k T B_{p} \end{align}
$$
Où :

- **P** est la puissance du bruit.
- **k** est la constante de Boltzmann ($1.38 \cdot 10^{-23}$)
- **T** est la température en degrés Kelvin.
- $\mathbf{B_{p}}​$ est la largeur de bande du système.

----

**SLIDE 26**