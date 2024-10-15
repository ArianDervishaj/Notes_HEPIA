---
title: Gestion de la mémoire
draft: false
tags:
  - Processeur
---
Un programme est fait de section que le programmeur peut placer à différents endroits de l'espace d'adressage.
La mémoire elle-même peut être localisée à différents endroits et être de différent type (ROM, SRAM, DDR, ...).
Un plan d'organisation des sections de code et données doit être fourni au compilateur.
**Ce plan correspond à des adresses physiques et dépend de chauqe plateforme.**

![[Pasted image 20241015204216.png]]
# Accès à la mémoire (load/store)

- Load (**LDR**) charge un mot de 32 bits de la mémoire vers un registre
- Store (**STR**) charge un mot de 32 bits d'un registre vers la mémoire
## Example
```assembly
MOV R4,#0x100    // adresse arbitraire
MOV R5,#0x200    // adresse arbitraire
LDR R0,[R4]      // R0 = *(R4), R4 employé comme pointeur
STR R0,[R5]      // *(R5) = R0
LDR R1,[R4,#4]   // lecture avec offset: R1=*(R4+4)
```
## Syntaxe générale
```assmebly
// Rx = *(Ry+offset) où Ry peut aussi être le PC

LDR<t><c> Rx,[Ry<,#offset_bytes>]<!> // pré-indexage
LDR<t><c> Rx,[Ry]<,#offset_bytes> // post-indexage

// *(Ry) = Rx où Ry ne peut pas être le PC

STR<t><c> Rx,[Ry<,#offset_bytes >]<!> // pré-indexage
STR<t><c> Rx,[Ry]<,#offset_bytes> // post-indexage
```

- **t** : type de transfert
- **c** : condition (utile uniquement si [[Langage assembleur#Instruction IT|instruction IT]]). LDRNE, LDRCC, STRGT, ...
- **!** : permet de réécrire dans le registre de base addressee calculée. (**write-back**)
### Write-back

```assembly
LDR R1, [R0, #4]!
```

- Load la valeur $R0 + 4$ dans R1.
- Après le load, $R0 + 4$ est écrit (*written back*) dans R0. $R0 = R0 + 4$.
## Examples de load/store

```assembly
LDR R0,[R1,#4]  // R0 = *(R1+4), en C: R0=*(R1+1);
LDR R0,[R1,#4]! // R1 += 4; R0 = *(R1), en C: R0=*(++R1);
LDR R0,[R1],#4  // R0 = *(R1); R1 += 4, en C: R0=*(R1++);
STR R0,[R1,#4]  // *(R1+4) = R0, en C: *(R1+1)= R0;
STR R0,[R1,#4]! // R1 += 4; *(R1)= R0, en C: *(++R1) = R0;
STR R0,[R1],#4  // *(R1)= R0; R1 += 4, en C: *(R1++) = R0
```
# Chargement de mots de 32 bits

ARM étant de type [[Langage assembleur#RISC (Reduced Instruction Set Computer)|RISC]] chaque instruction doit avoir la même taille.
Il est donc **impossible** de coder une instruction qui charge une valeur de 32 bits dans un registre à cause de l'OPCODE.
## Macro '.equ'

La macro `.equ` permet de réserver et initialiser une constante unique (dans la ROM) et on pourra y accéder avec un pointeur.
```assembly
.equ var2, 0x12345678
	LDR R0,=var2
```
# Déclaration d'une section et d'un tableau de valeurs
La macro `=nom` permet de charger l'adresse d'une variable en utilisant son adresse relative au PC (compteur de programme). Cela équivaut à l'expression suivante :

$$
\left[ \text{PC} + \left( \text{adresse\_de\_nom} - \text{adresse\_instruction\_courante} \right) \right]
$$
## Example
```assembly
// Section pour les données (stockées en RAM)
.data  

// Étiquette pour une variable, pointant vers les adresses suivantes
var1:

// Réserve 3 emplacements de 32 bits pour les variables
.long 0x10, 0x20, 0xFF   
```

```assembly
LDR R4, =var1      // Charge l'adresse de "var1" dans le registre R4
                   // Équivalent à: LDR R4, [PC + (adresse_var1 - adresse_actuelle)]

LDR R3, [R4]       // Charge la valeur à laquelle R4 pointe dans R3
                   // R3 = *(R4), ici R4 agit comme un pointeur

ADD R3, R3, #1     // Incrémente la valeur dans R3 de 1
                   // R3 = R3 + 1

STR R3, [R4]       // Stocke la valeur de R3 dans l'adresse pointée par R4
                   // *(R4) = R3
```

> [!Warning] Importants
> - La macro `=nom` fonctionne uniquement avec l'instruction `LDR` (pour charger une adresse), et **pas** avec l'instruction `STR` (pour stocker une valeur).
> - L'instruction `STR` ne fonctionnera que si `R4` pointe vers une zone en RAM (ou une zone marquée en 'W' pour "Writable", c'est-à-dire modifiable).
# Accès à la mémoire avec une taille de mot variable

Les instructions `LDR` et `STR` permettent de charger et stocker des données de différentes tailles grâce à des extensions. Voici les extensions disponibles :

- **B** : un octet (8 bits). Avec `LDR`, l’octet est considéré comme non-signé et étendu à 32 bits en remplissant les bits restants avec des zéros.
- **SB** :un octet signé (8 bits). Avec `LDR`, l’octet est signé et étendu à 32 bits en préservant le signe.
- **H** : mot de 16 bits. Avec `LDR`, le mot est considéré comme non-signé et étendu à 32 bits avec des zéros.
- **SH** : mot de 16 bits signé. Avec `LDR`, le mot est signé et étendu à 32 bits en préservant le signe.
## Example

```assembly
var:                // Adresse 32 bits des données suivantes
.long 0x12345678    // Variable de 32 bits pré-initialisée avec 0x12345678

LDR R0, =var        // Charge l’adresse de "var" dans R0
LDRB R3, [R0]       // Charge l’octet à l'adresse R0 dans R3 (ici R3 = 0x78)

MOV R4, #0x1000     // Charge la valeur 0x1000 dans R4
STRH R4, [R0]       // Stocke les 16 bits de R4 à l'adresse R0 (écrase les deux premiers octets de "var")
```

- Lors du chargement avec `LDRB`, seul l'octet de poids faible de `var` (0x78) est chargé dans R3.
- Après `STRH`, l’adresse `var` contient `0x12341000` (les 16 bits inférieurs sont remplacés par la valeur `0x1000`).

> [!Warning] Remarque
> L’extension des bits (remplissage par des zéros ou préservation du signe) se fait uniquement lors des chargements (`LDR`).
# Accès à la mémoire avec registres multiples
Il est possible de charger ou de stocker plusieurs registres en une seule instruction avec les commandes suivantes :

```assembly
- LDMd Rx<!>, {Ri, Rj, Rk, …} : Charge plusieurs registres à partir de la mémoire (Load Multiple)
- STMd Rx<!>, {Ri, Rj, Rk, …} : Stocke plusieurs registres dans la mémoire (Store Multiple)
```
## Les suffixes d:

- **IA** : Incrémente l'adresse dans Rx **après chaque accès** à la mémoire (Increment After).
- **DB** : Décrémente l'adresse dans Rx **avant chaque accès** à la mémoire (Decrement Before).
## Le symbole `!` :

- Si le `!` est présent, Rx sera mis à jour avec la dernière adresse atteinte après le chargement ou le stockage.
## Example
```assembly
LDMIA R0, {R1, R5, R6, R9}
```

- Charge les valeurs à partir de la mémoire, en commençant à l'adresse pointée par `R0`, dans les registres `R1`, `R5`, `R6`, et `R9` dans cet ordre.
- **R0 n'est pas modifié** dans cet exemple car il n'y a pas de `!`.

![[Pasted image 20241015221722.png]]

```assembly
STMDB R0!,{R1,R5,R6,R9}
```
- Charge les valeurs à partir de la mémoire, en commençant à l'adresse pointée par `R0`, dans les registres `R1`, `R5`, `R6`, et `R9` dans cet ordre.
- **R0 est modifié**.

![[Pasted image 20241015221854.png]]

> [!Info] Notes
> l’ensemble de registres entre { } doit être écrit dans l’ordre croissant de leurs indices. Cette instruction prendra plusieurs cycles d'horloge.
# Accès à un tableau avec indice.
#### Supposons :
- `A` et `B` sont des tableaux de caractères ASCII (8 bits).
- On veut copier un caractère de `A[i]` vers `B[i]`.
#### Affectation des registres :
- `R0` pointe vers `A`
- `R2` pointe vers `B`
- `R1` contient l'indice du tableau `i`

```assembly
.Lbcl:
  LDRB R3, [R0, R1]  // R3 = *(A + i) = A[i] (copie)
  STRB R3, [R2, R1]  // *(B + i) = R3 (coller)
  ADD R1, #1         // i = i + 1
  CMP R1, #n         // Comparer i avec n
  BNE .Lbcl          // Si i != n, revenir à .Lbcl
```
# Accès à un tableau optimisé par adresse
#### Supposons :
- `A` est un tableau d'entiers (8 bits).
- On effectue une copie de type `*B++ = *A++` (A et B sont des pointeurs).
#### Affectation des registres :
- `R0` pointe vers `A`
- `R1` contient la taille du tableau
- `R2` pointe vers `B`

```assembly
.Lbcl:
  LDRB R3, [R0], #1  // R3 = *(A++) (copie)
  STRB R3, [R2], #1  // *(B++) = R3 (coller)
  SUBS R1, #1        // décrémenter i (si i == 0, terminé)
  BNE .Lbcl          // Si i != 0, revenir à .Lbcl
```
# Accès à un tableau avec des éléments de 16 bits
#### Supposons :
- `A` est un tableau d'entiers, et `b` est un entier (16 bits).
- On effectue une opération de type `*A = *A++ + b`.
#### Affectation des registres :
- `R0` pointe vers `A`
- `R1` contient la taille du tableau
- `R2` contient `b`

```assembly
.Lbcl:
  LDRH R3, [R0]      // R3 = *(A)
  ADD R3, R2         // R3 = A[i] + b
  STRH R3, [R0], #2  // *(A++) = R3 (mettre à jour A)
  SUBS R1, #1        // décrémenter i
  BNE .Lbcl          // Si i != 0, revenir à .Lbcl
```