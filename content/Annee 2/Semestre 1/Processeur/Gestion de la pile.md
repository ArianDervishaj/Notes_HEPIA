---
title: Gestion de la pile
draft: false
tags:
  - Processeur
---
# Utilisation de la pile

La **pile (stack)** est une zone de mémoire temporaire utilisée pour :

- Sauvegarder l’adresse de retour après un appel de fonction.
- Passer des paramètres lorsque ceux-ci dépassent 4 registres.
- Sauvegarder des registres et des variables locales.

Le **registre R13 (SP)** est utilisé comme pointeur de pile. La pile fonctionne en mode LIFO (last-in, first-out), et son adresse **décroît** à mesure que des éléments sont ajoutés.
# Différence entre la pile et le tas

- **La pile** : Utilisée pour des variables locales et temporaires.
- **Le tas** : Utilisé pour des allocations mémoire dynamiques (par exemple, via `malloc`).

![[Pasted image 20241015232547.png]]
# Utilisation d'une pile

On peut y placer (**push**) et récupérer (**pop**) des données à l'aide du registre **SP** qui pointe toujours sur l'élément le plus bas de la pile.

![[Pasted image 20241015232736.png]]
## En C
```c
int leaf_example(int g, int h, int i, int j) {
    int f, k;
    k = g + h;
    f = i + j;
    return k - f;
}
```
## En assembleur ARM "automatique"

```assembly
leaf_example:
PUSH {R4,R5} // sauvegarde des registres
ADD R5,R0,R1 // k = g + h
ADD R4,R2,R3 // f = i + j
SUB R0,R5,R4 // retour = k - f
POP {R4,R5} // restauration des registres
BX LR // retour de procédure
```
# Passage de paramètre avec la pile

Quand plus de 4 paramètres doivent être passés à une fonction (R0 à R3 étant les seuls registres disponibles pour cela), les paramètres supplémentaires sont placés sur la pile.
## En C

```c
int addfonct(int a, int b, int c, int d, int e) {
    return a + b + c + d + e;
}
```
## En assembleur

```assembly
// Appel de la fonction depuis le code principal
MOV R0, R4
MOV R1, R5
MOV R2, R6
MOV R3, R7
PUSH {R8}        // Placer ‘e’ sur la pile
BL addfonct      // Appeler la fonction
POP {R8}         // Remettre SP à sa place après l’appel
...
addfonct:
  ADD R0, R0, R1
  ADD R0, R0, R2
  ADD R0, R0, R3
  LDR R1, [SP]   // Récupérer ‘e’ de la pile
  ADD R0, R0, R1
  BX LR          // Retour à l'appelant

```
# Gestion des fonctions imbriquées

Quand une fonction appelle une autre fonction, il faut sauvegarder le contexte (les registres utilisés) pour ne pas perdre leur état lors du retour. Cela se fait en poussant les registres importants sur la pile avant l’appel, et en les restaurant après.

```c
int f1() {
    f2();
}
int f2() {
    // Code de f2
}
```

```assembly
f1:
  PUSH {LR}        // Sauvegarder l'adresse de retour
  BL f2            // Appeler f2
  POP {LR}         // Restaurer l'adresse de retour
  BX LR            // Retour à l'appelant
```
# Fonction récursive

Les fonctions récursives nécessitent aussi de sauvegarder le contexte à chaque appel pour pouvoir le restaurer lors du retour.

```c
int fact(int n) {
    if (n > 1) {
        return n * fact(n - 1);
    } else {
        return 1;
    }
}
```

```assembly
fact:
  PUSH {R0, LR}     // Sauvegarder l'argument et l'adresse de retour
  SUBS R0, #1       // n = n - 1
  BGT .Lrcall       // Si n > 1, continuer
  MOV R0, #1        // Si n <= 1, retourner 1
  POP {R0, LR}        // Libérer la pile
  BX LR             // Retourner
.Lrcall:
  BL fact           // Appeler fact(n - 1)
  POP {R2, LR}      // Restaurer n et l'adresse de retour
  MUL R0, R2, R0    // R0 = n * fact(n - 1)
  BX LR             // Retourner à l'appelant
```