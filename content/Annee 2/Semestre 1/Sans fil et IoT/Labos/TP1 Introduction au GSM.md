---
title: TP1 Introduction au GSM
draft: false
tags:
  - IoT
  - Labo
---

# Authentification au réseau 2G

s'articule autour de plusieurs entités clés et d'un ensemble de procédures qui servent à authentifier les utilisateurs de manières sécurisées.
## Les entités impliquées

- HLR (Home location register) : base de données centrale ayant les infos d'abonnement des utilisateurs y compris leurs clés.
- AuC (Auth Center) : Partie du HLR. Genère et vérifies les paramètres d'auth
- MS (Mobile Station) : dispositif mobile de l'utilisateur
- BTS (Base Transceiver Station): Station facilitant la communications avec les MS


> [!info] Pour plus de détails
> Voir [[Réseaux mobiles#Architecture des réseaux cellulaires| architecture des réseaux]]

## Processus d'authentification

1. **Génération du défi** :  
    La **Mobile Station (MS)** tente de se connecter au réseau via la **Base Transceiver Station (BTS)**. La **BTS** transmet cette demande au **VLR/MSC** (Visitor Location Register / Mobile Switching Center).  
    
    Le **VLR/MSC**, n'ayant pas les informations nécessaires pour l'authentification, fait une demande au **HLR** (Home Location Register) pour obtenir ces informations. 
     
    L'**AuC** (Authentication Center), qui fait partie du **HLR**, génère un triplet d'authentification comprenant :
    
    - Un nombre aléatoire **RAND** (128 bits),
    - Un code de réponse **SRES** (généré avec l'algorithme A3),
    - Une clé de chiffrement temporaire **Kc** (générée avec l'algorithme A8).
    
	Ce triplet est envoyé au **VLR/MSC**, qui transmet ensuite le **RAND** à la **MS**.
    
2. **Réponse à l'authentification** :  
	La **MS** utilise l'algorithme **COMP128** pour appliquer la fonction **A3** au **RAND** reçu, en utilisant sa clé secrète **$K_i$** (stockée sur la carte SIM). Cela génère un code de réponse **SRES** propre à la **MS**, qui est ensuite envoyé au **VLR/MSC**.
    
3. **Vérification** :  
	Le **VLR/MSC** compare le **SRES** reçu de la **MS** avec le **SRES** qu'il a reçu précédemment du **HLR**.  
    Si les deux **SRES** correspondent (**$SRES_{VLR} = SRES_{MS}$**), l'authentification est réussie.
    
4. **Génération de la clé de chiffrement** :  
    Une fois l'authentification réussie, la clé de chiffrement temporaire **Kc** (générée par l'algorithme **A8** et transmise dans le triplet initial) est utilisée pour sécuriser les communications ultérieures entre la **MS** et le réseau.

![[Pasted image 20241006143345.png]]

## Processus de chiffrement

#### Dans le GSM
Utilise l'ago de chiffrement par bloc A5 avec des variantes  (A5/1, A5/2, A5/3, et A5/4),

#### Dans le GPRS
Algo GEA. 

![[Pasted image 20241006144027.png]]