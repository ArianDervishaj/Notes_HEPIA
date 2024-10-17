---
title: Les bases de Java
draft: false
tags:
  - POO
---
# Les types primitifs

- Commence par une minuscule
- à son équivalent en classe qui commence par une majuscule (Byte, Integer, Double, ...)
- est stocké sur la pile (stack)

> [!Important] 
> Préférez toujours un type primitif !

| Category        | Types   | Size (bits) | Minimum Value                                      | Maximum Value                                      | Precision                                           | Example                    |
|-----------------|---------|-------------|----------------------------------------------------|----------------------------------------------------|-----------------------------------------------------|----------------------------|
| Integer         | `byte`  | 8           | $\Large{-128}$                                     | $\Large{127}$                                      | From $\Large{+127}$ to $\Large{-128}$               | `byte b = 65;`             |
|                 | `char`  | 16          | $\Large{0}$                                        | $\Large{2^{16} - 1}$                               | All Unicode characters                              | `char c = 'A';`            |
|                 | `short` | 16          | $\Large{-2^{15}}$                                  | $\Large{2^{15} - 1}$                               | From $\Large{-32,768}$ to $\Large{+32,767}$         | `short s = 65;`            |
|                 | `int`   | 32          | $\Large{-2^{31}}$                                  | $\Large{2^{31} - 1}$                               | From $\Large{-2,147,483,648}$ to $\Large{+2,147,483,647}$ | `int i = 65;`              |
|                 | `long`  | 64          | $\Large{-2^{63}}$                                  | $\Large{2^{63} - 1}$                               | From $\Large{-9,223,372,036,854,775,808}$ to $\Large{+9,223,372,036,854,775,807}$ | `long l = 65L;`            |
| Floating-point  | `float` | 32          | $\Large{2^{-149}}$                                 | $\Large{(2 - 2^{-23}) \cdot 2^{127}}$              | From $\Large{3.4028235 \times 10^{38}}$ to $\Large{1.4 \times 10^{-45}}$ | `float f = 65f;`           |
|                 | `double`| 64          | $\Large{2^{-1074}}$                                | $\Large{(2 - 2^{-52}) \cdot 2^{1023}}$             | From $\Large{1.7976931348623157 \times 10^{308}}$ to $\Large{4.9 \times 10^{-324}}$ | `double d = 65.55;`        |
| Other           | `boolean`| --         | --                                                 | --                                                 | `false`, `true`                                     | `boolean b = true;`        |
# Déclaration de variables

Seules deux façons de déclarer des variables sont recommandées : 
```java
int i = 6;
int j = 3 * 6;
```
## Valeur attribué par défaut

| Valeur | Type          |
| ------ | ------------- |
| 0      | int           |
| 0.0    | float, double |
| false  | bool          |
| ''     | char          |
| null   | object        |
# Type entiers

| type  | octets |
| ----- | ------ |
| byte  | 1      |
| short | 2      |
| int   | 4      |
| long  | 8      |
## Littéral

Valeur donnée explicitement dans le code

```java
int i = 14; // 14 est un littéral
```
## Conversion en long

Il faut post-fixer le littéral avec un L 

```java
long l1 = 3000000000; //erreur car pas un int
long l1 = 3000000000L; //OK
```
## Promotion numérique

Le résultat d'une expression entière est de type int.

```java
short a = 10;
short b = 20;
short c = a + b; // Erreur car a+b retourne un int.
```
# Type réels

| type   | octets |
| ------ | ------ |
| float  | 4      |
| double | 8      |
Littéral de type double.
Pour avoir un float, il faut le postfixer avec un f.

```java
float a = 3.2f;
```

Pas de promotion numérique, une expression sur deux float retourne un float.
# Type caractère

| type | octets |
| ---- | ------ |
| char | 2      |
- Représentation Unicode
- interprété comme un entier non signé
- respecte la promotion numérique

```java
char a = 'a';
```
# Type booléen

| type    | octets |
| ------- | ------ |
| boolean | 1      |
# Compatibilité et équivalence
## Types non équivalents entre eux

- Si l'espace mémoire diffère
- Si l'un est signé l'autre est non signé
## Types comparables 

- Si une comparaison peut être faite directement entre les deux.
- Par exemple, les int et les float sont des types comparables :

```java
int i = 10;
float j = 3.0f;

if ( i < j ){...}
```
## Types compatibles

- Si une conversion entre ces types sont possibles.
- [[Les bases de Java#Conversions implicites|Conversion implicites]] possible si aucune perte de précision.
- Si la conversion implique une perte de précision alors conversion explicite (*cast*) au risque de perdre de l'information.
# Conversions implicites
## Possible selon la hiérarchie suivante :

1. `byte -> short -> int -> long -> float -> double`
2. `char -> int -> long -> float -> double`
## Conversion impossibles :

1. `byte -> char`
2. `short -> char`
## Deux types de conversions implicites dans les expressions :
### Promotion numérique sur les opérateurs

Byte, short, char -> int 
### Ajustement de types

Si $T_{1} \subset T_{2}$, alors $T_{1}$ peut être converti en $T_{2}$
# Conversions explicites

```java
short s = 32768; // Erreur car 32768 > 32767 = val max d'un short.
short s1 = (short) 32768; // OK mais overflow donc s1 = -32768
```