---
title: MacFlooding
draft: false
tags:
  - RPI
---

# Pourquoi la sécurité d'un LAN est importante ? 

> [!WARNING] Attention
> Si une couche inférieure est compromise, alors toutes les couches au-dessus le sont également. 
# Sécurité du LAN 

Historiquement, on considérait que l'on pouvait faire confiance aux machines d'un même LAN : 
- Peu de machines fixes et statiques. 
- Machines enregistrées manuellement sur le réseau. 

**Aujourd'hui, cela n'est plus pertinent**, en raison de la mobilité des terminaux et du concept «Bring Your Own Device» (BYOD).

> [!Warning] Attention
> Le LAN doit être protégé.

---
# MAC Flooding
## Mémoire CAM (Content Addressable Memory)

- Une table associative, implémentée en matériel.
- Permet un accès clé-valeur ultra rapide, très utilisée dans les switches industriels.
    - Un lookup par trame, avec des vitesses de 10-100 Gb/s par port.
- Taille fixe :
    - Taille typique : 8k, 16k ou 32k adresses MAC sur un switch de taille moyenne.
    - 130k adresses MAC sur un plus gros switch.
### Switch avec CAM pleine

Quand la table CAM d'un switch est saturée, le switch entre en mode «fail open» :
- Tout trafic sans entrée dans la CAM est **floodé** sur tous les ports associés au VLAN du port d'entrée.
- Si un autre switch est connecté au même VLAN, il peut également être attaqué.
- L'attaque touche **tout le domaine de diffusion**, pas seulement un switch.
- Il devient possible d'écouter le trafic qui circule à travers le switch.
    - Sans VLAN, il est possible d'écouter **tout** le trafic.

---
## MACof

MACof est un outil permettant de flooder un switch avec des adresses MAC différentes pour saturer une CAM de 16k entrées en quelques minutes.

---
# Comment mitiger le MAC flooding ?
## Port Security + Logs

Sur les ports critiques, on configure la **port security** :
- Limite le nombre d'adresses MAC autorisées.
- Le switch peut être mis en mode apprentissage : les premières adresses MAC trouvées sont enregistrées dans la CAM.
- Si le nombre d'adresses MAC dépasse la limite :
    - Le port peut être automatiquement désactivé.
    - Une alerte peut être générée pour signaler un dépassement.
## Et les switches software ?

- **Pas de CAM** : la table MAC est implémentée en software.
    - Taille limitée par la mémoire de l'hôte.
    - Plus lent qu'un switch hardware (c'est le CPU qui gère dans la RAM).
- Attention à l'implémentation :
    - Le bridge Linux utilise une **hashmap** basée sur les 32 derniers bits des adresses MAC.

---
# Et en wifi ?

En principe, un réseau WiFi n'est **pas vulnérable** à cette attaque :
- L'association d'une station à un point d'accès est basée sur l'adresse MAC de la station.
    - Si une adresse change, la connexion WiFi est perdue.
    - À tester avec une machine virtuelle bridgée sur l'interface WiFi (à faire en exercice pratique).


> [!Warning] Ne pas activer port-security sur un réseau sans fil
> - Un point d'accès est vu comme un port réseau.
> - De nombreuses stations peuvent être connectées, ce qui pose problème.

---
# Que se passe-t-il si on envoie un message à une adresse multicast ?
### Multicast en destination :
- On communique avec un **groupe de machines**, et non une seule.
- Les switches vont **flooder** le message à travers tous les ports sauf celui d'entrée.
### Multicast en source :
- Si la source utilise le multicast, les tables de routage doivent constamment être mises à jour.
    - Les membres du groupe peuvent changer fréquemment.
    - Les adresses multicast ne sont pas conservées dans les tables des switches, car elles ne sont pas associées à un emplacement unique et fixe.