---
title: Vulnérabilités STP
draft: false
tags:
  - RPI
---

# Spanning Tree Protocol

L'objectif du STP est d'assurer en permanence une topologie de niveau 2 sans boucle dans un réseau avec des connexions redondantes entre les switches.

![[Pasted image 20241008153435.png]]

1. Election d'un switch racine
2. Calcul du plus court chemin
## Message du protocole STP

Les messages sont envoyés dans des **BPDU** (Bridge Protocol Data Unit)
### 2 Types de messages

- Conf (**CNF**), contient les informations minimale pour établie l'arbre après convergence
- Topology Change Notification (**TCN**), indique un changement de topologie.

> [!important] Convergence
> Tant que STP n'a pas convergé, les ports du switch ne forwardent aucune trame
## Etats des ports STP

- **Blocking** : le switch écoute uniquement les BPDU et ne forwarde rien. C’est l’état initial, et peut-être final du switch si il ne passe pas en Listening.
- **Listening** : Le switch envoie des BPDU, car il a des ports dans l’arbre couvrant
- **Learning** : la racine est élue, si le switch n’est pas racine, ils arrête de générer des BPDU et ne fait que retransmettre ceux de la racine.
- **Forwarding** : Le port est bien sur l’arbre couvrant vers la racine.

![[Pasted image 20241008153906.png]]

---
# Exemple d'attaque STP
## L’attaquant a physiquement accès à un port du switch

Comme **les messages STP ne sont pas authentifiés** rien n’empêche d’y **injecter des BPDUs**.
L'attaquant peut donc prendre le rôle de la racine sur le réseau puis :
- Partir, le réseau sera en panne le temps que STP converge à nouveau.
- Capturer le trafic envoyé vers lui et effectuer un Man in the Middle.

![[Pasted image 20241008154103.png]]
## Yersinia

Boite à outils d'attaques sur un LAN.
- Objectif : audit efficace car les mêmes problèmes de sécurité reviennent tout le temps.
- Attaques rapides et simples, tout est déjà implémenté, il ne reste plus qu’à taper sur les bonnes touches et comprendre ce qu’on fait.
- Interface graphique ou ncurses