---
title: Appels de fonctions
draft: false
tags:
  - Processeur
---
# Convention des registres ARM pour les appels de fonctions en assembleur (depuis le C*)

- **R0-R3** : Utilisés pour les arguments et comme registres temporaires.
- **R0-R1** : Utilisés pour retourner le résultat de la fonction.
- **R4-R11** : Doivent être conservés après l'appel de fonction.
- **R12-R15** : Registres spéciaux :
    - **R12 (FP)** : Pointeur de cadre (Frame Pointer), utilisé lors de l'appel de fonction (mais non utilisé ici).
    - **R13 (SP)** : Pointeur de pile (Stack Pointer).
    - **R14 (LR)** : Garde l'adresse de retour après un appel de fonction.
    - **R15 (PC)** : Compteur de programme, pointe vers l'instruction suivante à exécuter.
# Utilisation du registre LR (R14) pour stocker l'adresse de retour

L'instruction `BL` (Branch and Link) effectue un saut vers une fonction tout en sauvegardant l'adresse de retour dans le registre **LR**.

``` assembly
	BL ma_fonc   // Sauter à la fonction ma_fonc, et sauvegarder l'adresse de retour dans LR
...
ma_fonc:
  ...        // Code de la fonction
  BX LR // Retourner à l'instruction après l'appel (ou utiliser MOV PC, LR)
```
# Passage de paramètres

Le passage de paramètres est le mécanisme qui permet de **communiquer des informations** entre un programme appelant et une routine (fonction ou procédure).
## Règles principales :

1. **Les valeurs** sont toujours passées en tant que paramètres à une fonction.
2. **Ces valeurs sont effacées** à la fin de l'exécution de la fonction.
## En C :

- On peut passer des **nombres** ou des **pointeurs** en paramètre. Dans tous les cas, il s'agit de valeurs (que ce soit un nombre ou une adresse).
## En assembleur :

- Il n'y a pas de notion de "type". C'est au programmeur de différencier un nombre d'une adresse lors du passage de paramètres.
## Exemple de passage de paramètre et retour de résultat
### En C :

```c
int fonct(int a, int b){ return 2*(a+b); }

main(){
	int x, y, z;
	z = fonct(x, y);
	}
```
### En assembleur :

R4 = x, R5 = y, R6 = z

```assembly
main:
  MOV R0, R4        // Charger x dans R0
  MOV R1, R5        // Charger y dans R1
  BL fonct          // Appel de la fonction
  MOV R6, R0        // Charger le résultat dans z (R6)
...

fonct:
  ADD R2, R0, R1    // Additionner a et b
  ADD R0, R2, R2    // Renvoyer 2 * (a + b)
  BX LR             // Retourner à l'appelant
```
## Passage par valeur 

```c
x = addfonct(a, b);
```

```assembly
MOV R0, a          // Charger a dans R0
MOV R1, b          // Charger b dans R1
BL addfonct        // Appeler la fonction addfonct
```
## Passage par référence

```c
x = addfonct(&a, &b);
```

```assembly
MOV R0, adresse_a  // Charger l'adresse de a dans R0
MOV R1, adresse_b  // Charger l'adresse de b dans R1
BL addfonct        // Appeler la fonction addfonct
...
addfonct:
  LDR R2, [R0]     // Charger la valeur de a
  LDR R3, [R1]     // Charger la valeur de b
  ADD R0, R2, R3   // Additionner a et b et renvoyer le résultat
```

