---
title: Appels de fonctions, gestion de la pile
draft: false
tags:
  - Processeur
---

#### **Appels de fonctions et gestion de la pile**

- Une pile (stack) est un espace mémoire temporaire pour :
    - Stocker l'adresse de retour
    - Passer des paramètres
    - Sauvegarder les registres
    - Stocker des variables locales
- En ARM, le registre R13 est utilisé comme **stack pointer** (**SP**).
- La pile est une zone LIFO (Last-In First-Out), où les éléments sont retirés dans l'ordre inverse de leur insertion.
- L'adresse de la pile décroit au fur et à mesure que des éléments y sont placés

#### **Utilisation de la pile**

- La pile se trouve en général à la fin de la mémoire et croît vers le début (adresses décroissantes).
- **Différence pile/heap** :
    - La pile : mémoire temporaire (variables locales).
    - Le tas (heap) : stockage de variables globales ou allocations dynamiques. (adresse croissante)

![[Pasted image 20241002141849.png]]

Le **SP** pointe sur l'élément le plus bas de la pile (plus petite adresse)
#### **Exemple d'utilisation de la pile**

###### Exemple en C :
```c
int leaf_example(int g, int h, int i, int j) {
  int f, k;
  k = g + h;
  f = i + j;
  return k - f;
}

```

###### En assembleur 
on utilise la pile pour sauvegarder et restaurer les registres
```assembly
SUB SP, #8         // Réservation de place
STR R5, [SP, #4]   // Sauvegarde R5
STR R4, [SP, #0]   // Sauvegarde R4
ADD R5, R0, R1     // k = g + h
ADD R4, R2, R3     // f = i + j
SUB R0, R5, R4     // Retour = k - f
LDR R4, [SP, #0]   // Restauration R4
LDR R5, [SP, #4]   // Restauration R5
ADD SP, #8         // Mise à jour pile
BX LR              // Retour

```

#### **Passage de paramètres par la pile**

- En ARM, seuls 4 paramètres peuvent être passés par des registres (R0 à R3). Les paramètres supplémentaires sont placés sur la pile :
```assembly
MOV R0, R4
MOV R1, R5
MOV R2, R6
MOV R3, R7
PUSH {R8}           // 'e' est placé sur la pile
BL addfonct         // Appel fonction
ADD SP, #4          // Repositionnement du SP (jette 'e')
```

#### **Fonctions imbriquées**

- Les fonctions imbriquées doivent sauvegarder leur contexte pour ne pas affecter l'état des fonctions appelantes.
```assembly
f1: PUSH {LR}       // Sauvegarde LR
BL f2               // Appel f2
POP {LR}            // Restauration LR
BX LR               // Retour à f1
```

#### **Fonctions récursives**
```c
int fact(int n) {
  if (n > 1)
    return n * fact(n - 1);
  else
    return 1;
}

```

```assembly
fact:
PUSH {R0, LR}     // Sauvegarde n (R0) et LR
SUBS R0, #1       // R0 = n - 1
BGT .Lrcall       // Si n - 1 > 0, appel récursif
MOV R0, #1        // Si n <= 1, retour 1
ADD SP, #8        // Libération de la pile
BX LR             // Retour
.Lrcall:
BL fact           // Appel récursif
POP {R2, LR}      // Récupère n et LR
MUL R0, R2, R0    // n * fact(n-1)
BX LR             // Retour
```