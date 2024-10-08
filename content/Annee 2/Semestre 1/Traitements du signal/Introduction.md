---
title: Introduction
draft: false
tags:
  - Signal
---

# Qu'est ce qu'un signal ?

Un signal est une grandeur représentative d'un phénomène physique.

![[Pasted image 20240922225828.png]]

Les signaux sont porteurs d'informations. Il faut ensuite traiter ces informations et prendre des décisions.
L'objectif du traitement du signal et d'images est de développer des systèmes permettant **d'assister** l'observateur humain dans le traitement et la prise de décision.
# Que contient le signal

Un signal contient de **l'information**

$\large{Information} \subset \large{Signal}$
## Deux familles de signaux

1. Les signaux **déterministes**
2. Les signaux **aléatoires**
### Signaux Déterministes

Les informations contenues dans un signal déterministes sont **parfaitement** prédictibles par une fonction mathématique.

Exemple : un signal sinusoïdal
### Signaux aléatoires

Les informations contenues dans un signal **aléatoires** sont imprévisibles ou **non parfaitement** prédictibles.
Elles sont par contre **descriptibles** par des observations statistiques (moyenne, variance, distribution,...)

Exemple : une photo, du bruit.
# Dimensionnalité du signal

Le signal est une grandeur modélisée par une fonction **mathématique**.  
La dimensionnalité du signal est le nombre de **variables** (et non de paramètres) dont dépend cette fonction.

- Signal **1D**: u(t). Exemple: un signal audio issu d'un microphone.
- Signal **2D**: u(x,y). Exemple: une image en niveaux de gris issue d'un capteur CCD.
- Signal **3D**: u(x,y,z). Exemple: une image couleur issue d'un capteur CCD, une image tridimensionnelle issue d'un scanner.
- Signal **4D**: u(x,y,z,t). Exemple: un film en images couleur issu d'une caméra.

## Que représente la fonction mathématique du signal

Un signal **1D** est considéré comme un **vecteur**.
Un signal **2D, 3D ou 4D** (image /vidéo) est considéré comme une **matrice à 2, 3 ou 4 dimensions**.

![[Pasted image 20240922230439.png]]

$$
\large{Signal\ 1D:} x_1 = \begin{bmatrix} 1 & 2 & 0 & 4 \end{bmatrix}
$$$$
\large{Taille de x_1:} (1, 4) \\
\large{Signal 1D:} x_2 = \begin{bmatrix} 1 & 2 & 0 & 4 \end{bmatrix}
$$$$
\large{Taille\ de\ x_2:} (4,)
$$$$
\large{Signal 2D:} A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \\ 0 & 1 & 2 \end{bmatrix} \large{Taille de A:} (3, 3)
$$
