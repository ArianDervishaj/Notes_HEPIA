---
title: Langage assembleur
draft: false
tags:
  - Processeur
---
# Introduction

**Chaque architecture d'ordinateurs** possède son propre langage machine, et en principe **son propre langage d'assemblage.**
(Dans notre cas c'est le **assembleur ARM**)
# Différence entre processeurs RISC et CISC
## CISC (Complex Instruction Set Computer)
- Plus d'opérations élémentaires effectuées avec une seule instruction assembleur
- Instruction de taille variable
- Une instruction peut demandé plusieurs cycles d'horloge
## RISC (Reduced Instruction Set Computer)
- Opération simple
- Une instruction = un cycle d'horloge
- Chaque instruction à la même taille
# Registres

![[Pasted image 20241015193351.png]]

- **17 registres de 32 bits** interne au processeur ARM Cortex M3
- **R0 à R11 :** usage général
- **R12 :** linker les appels de fonctions particuliers
- **R13 :** pointeur de pile (**SP = Stack Pointer**)
- **R14 :** retour d'appel de fonctions (**LR = Link Register**)
- **R15 :** Adresse de l'instruction exécutée (**PC = Program Counter**)
- **xPSR :** registre d'état
## xPSR
Fanions de l'ALU :
- **N (Negative)** : Recopie le bit de poids fort du résultat
- **Z (Zero)** : Vaut 1 si l'état est nul
- **C (Carry)** : Retenue lors de calcul. (Permet indication de calcul correct ou faux pour les **nombres non signées**)
- **V (oVerflow)** : Indication de calcul correct ou faux pour les **nombres signées**
## Fanions dans une instruction

Il faut spécifier si on veut que le registre des fanions soit mis à jour ou pas (avec le **suffixe S**) lors de l'instruction
### Exemples

```assembly
ADDS R0, R1, R2 // Mets à jour les fanions
ADD  R0, R1, R2 // Le fait pas
```

| Fanions | Non signées             | Signées                 |
| ------- | ----------------------- | ----------------------- |
| N       | NON SIGNIFICATIF        | Signe du résultat       |
| Z       | Résultat est 0          | Résultat est 0          |
| C       | Dépassement de capacité | NON SIGNIFICATIF        |
| V       | NON SIGNIFICATIF        | Dépassement de capacité |
# ADD Instruction addition

```assembly
ADD  Rd, Rn, Rm                Rd = Rn + Rm
ADDS Rd, Rn, Rm                Pareil avec mise a jour des fanions
ADD  Rd, Rn, #x                Rd = Rn + x (x étant une constante)
```
# MOV Instruction de copie

```assembly
MOV  Rd, Rn
MOVS Rd, Rn
MOV  Rd, #x
MOVS Rd, #x
```
# Instruction de contrôle

Détermine instruction à exécuter par la suite.
## Sauts inconditionnels

```assembly
# Sauter à l'adresse addr (addr est copié dans PC)
b addr

# Sauter à l'adresse stocké dans R0
MOV PC, R0
```
## Sauts conditionnels

Permettent de réaliser les instruction conditionnelles (If, Then) et boucles (For, While ...) des langages de haut niveau.

```assembly
# Branch if equal, saute à l'addr si Z est 1
BEQ addr

# Branch if not equal, saute à l'addr si Z est 0
BNE addr
```

> [!info] Nommer les labels
> Il est conseillé de nommer les labels pour les sauts en commençant par **.L** pour indiquer que c'est une adresse locale et non pas d'un début de fonction.
# Instruction conditionnelle

> [!Warning] Attention
> Ce type d'instruction n'est possible que dans un bloc IT (If-Then)

```assembly
ADD{S}<c> Rd, Rn, Rm           Rd = Rn + Rm si la condition <c> est validée
ADD{S}<c> Rd, Rn, #x           Rd = Rn + x si la condition <c> est validée
MOV{S}<c> Rd, Rn
MOV{S}<c> Rd, #x
```
## Exemples

```assembly
# Cette instruction sera exécuté uniquement si Z = 1 car EQ regarde le fanion Z.
ADDSEQ R11, R4, R10

# Cette instruction sera exécuté uniquement si Z = 0 car EQ regarde le fanion Z.
MOVSNE R11, #100
```
# Instruction IT

Permet de rendre conditionnel les instructions qui suivent.
`IT{T/E}{T/E}{T/E}` permet de spécifier de 1 à 4 instructions.
## Example

```assembly
CMP    R0, R1                  # Comparer x et y
ITTEE  GE                      # 4 Instruction Then/Else. Condition de test GE

MOVEGE R0, #0                  # Then
ADDGE  R0, R0, R1              # Then
MOVLT  R0, #xFF                # Else
SUBLT  R0, R0, R1              # Else

MOV    R2, R0                  # En dehors de IT
```

Les instructions si dessus reviennent à faire :

```c
if (R0 >= R1){
	R0 = R0;
	R0 = R0 + R1;
}else{
	R0 = 0xFF;
	R0 = R0 + R1;
}

R2 = R0;
```
# If - then - else

Comment faire le code ci-dessous en assembleur ?

```c
if(x == y){
	x = 0;
}else{
	x = x + y;
}
```
## Avec sauts

```assembly
	CMP R0, R1
	
	BNE .L1
	MOV R0, #0
	B .LEXIT

.L1:
	ADD R0, R0, R1
.LEXIT:
```
## Avec instructions conditionnelles IT

```assembly
CMP R0, R1
ITE EQ
MOVEQ R0, #0
ADDNE R0, R0, R1
```
# Boucle Do-While

```c
do {
	x = x + 1;
	y = y - 1;
}while (y != 0);
```

```assembly
.L2:
	ADD  R0, R0, #1      # x = x + 1
	ADDS R1, R1, #-1     # y = y - 1 avec mis à jour de fanions

	BNE .L2              # if (y != 0) goto .L2
```
# Boucle While

```c
while (y > 0){
	y = y - 1;
	x = x + 1;
}
```

```assembly
	CMP   R1, #0
	BLE   .L8         # Branch to .L8 if R1 <= 0
.L5:
	ADDS R1, R1, #-1  # y = y - 1
	ADD  R0, R0, #1   # x = x + 1
	BGT  .L5          # Branch to .L5 if y > 0
.L8:
```
# Boucle For

```c
for(int i = 0; i < x; i++){
	y = y + 10;
}
```

```assembly
	MOV R2, #0
	CMP R2, R0
	BGE .L8           # Branch to .L8 if i >= x

.L6:
	ADD R1, R1, #10
	ADD R2, R2, #1
	CMP R2, R0
	BLT .L6           # Branch to .L6 if i < x

.L8:
```