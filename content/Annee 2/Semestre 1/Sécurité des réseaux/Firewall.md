---
title: Firewall
draft: false
tags:
  - SecuReseau
---

# Définition

Un pare-feu est un dispositif matériel ou logiciel qui contrôle et gère le trafic réseau entre une zone interne (à protéger) et une zone externe (comme Internet), en suivant une politique de sécurité définie.
## Tâches principales

- **Contrôle d'accès centralisé** : Gérer les permissions d'accès au réseau en fonction des règles de sécurité.
- **Enregistrement du trafic** : Conserver des journaux des connexions pour l'analyse et la surveillance.
- **Défense en profondeur** : **Défense en profondeur** : Protéger plusieurs couches du réseau (serveurs, hôtes, etc.) en appliquant des mesures de sécurité à chaque niveau.
## Localisation

- Positionné entre le réseau interne (privé) et le réseau externe (public, comme Internet).
- Souvent sous la forme d'un seul composant logiciel, mais peut inclure des configurations plus complexes avec plusieurs filtres, passerelles et zones comme une **DMZ** (zone démilitarisée).

---
# Techniques de sécurité
## Passage au guichet

Toutes les requêtes doivent passer par un point de contrôle unique, où elles sont filtrées selon des règles précises.
## Éloignement

Met en place des barrières pour décourager les attaques externes ou les empêcher de pénétrer dans le réseau interne.
## Confinement

Limite les mouvements des attaquants à l'intérieur du réseau, réduisant l'impact en cas d'intrusion.

---
# Types de pare-feu

## Filtrage réseau

- **Filtrage de paquets IP** : Filtre les données en se basant sur des critères comme l'adresse IP source ou destination, et le protocole.
- **Pare-feu avec état** : Suit l'état des connexions (établi, en attente, etc.) pour prendre des décisions de filtrage plus intelligentes.
## Filtrage transport et service

- Analyse le trafic au niveau des protocoles de transport (TCP, UDP) et des services.
- Peut agir comme un **proxy**, qui relaie le trafic entre un client et un serveur, ajoutant ainsi une couche de contrôle supplémentaire.
- **Exemple** : Le **pare-feu Firewalld** sous CentOS, qui gère les services accessibles par des hôtes spécifiques via TCP.
## Filtrage applicatif

- Contrôle les applications qui peuvent s'exécuter ou accéder au réseau. Par exemple, bloquer un transfert de fichiers ou restreindre une application via le pare-feu Windows.
## Contrôle d'accès statique

- Chaque paquet est traité indépendamment, sans tenir compte de l'état des connexions. C’est ce qu'on appelle un pare-feu **stateless**.
## Contrôle d'accès dynamique

- Gère les paquets en fonction du flux global de communication (état des connexions), ce qui permet de suivre l'état des connexions. C’est un pare-feu **stateful**.

---
# Matériel vs Logiciel
## Routeur filtrant

- **Avantages** : Crée une barrière périmétrique solide pour éviter que des utilisateurs non autorisés n'observent le réseau.
- **Inconvénients** : Plus cher, demande davantage de ressources (CPU et mémoire).
## Pare-feu logiciel sur serveur dédié

- **Avantages** : Dépend du système d'exploitation (OS) pour les règles de filtrage, et est flexible avec une interface dédiée pour sa gestion.
- **Inconvénients** : Moins performant que les pare-feu matériels.
## Pare-feu logiciel sur machine personnelle

- Protège un seul ordinateur. Son efficacité dépend du système d'exploitation utilisé.
## Pare-feu mixte

- Combinaison d'un pare-feu matériel et d'un logiciel avec un OS dédié, conçu pour être minimal et sécurisé.

---
# Limitations des pare-feu

- **Protection interne** : Un pare-feu ne peut pas protéger contre des menaces internes ou des systèmes déjà compromis.
- **Trafic non surveillé** : Certains types de trafic peuvent contourner le pare-feu (par exemple, les tunnels ou le trafic chiffré).
- **Détection des menaces émergentes** : Un pare-feu ne peut pas détecter automatiquement de nouvelles menaces s'il n'a pas de mécanisme de mise à jour pour ses règles.
- **Configuration complexe** : Nécessite une expertise pour configurer correctement les règles de filtrage et éviter les erreurs qui pourraient créer des failles.

> [!warning] Important
> Un pare-feu ne doit pas être confondu avec un **IDS** (système de détection d'intrusion) ou un **IRS** (système de réponse aux intrusions).

---
# Politiques de sécurité
## Méthode permissive

- **Autorise tout par défaut** : Le trafic est permis à moins qu'une règle ne l'interdise.
- Cette approche facilite l'accès, mais demande une administration plus stricte.
- Moins souvent utilisée car moins sécurisée.
## Méthode prohibitive (souvent par défaut)

- **Interdit tout par défaut** : Le trafic est bloqué sauf si une règle explicite l'autorise.
- Plus sécurisé et plus facile à administrer.
- Méthode couramment utilisée.
### Exemples
#### Règle pare-feu standard

- **Source** : Adresse IP externe.
- **Destination** : Adresse IP interne.
- **Protocole** : TCP ou UDP.
- **Action** : Autoriser ou bloquer le trafic entrant ou sortant.
#### Règle prohibitive

- **Source** : Toute adresse IP.
- **Destination** : Toute adresse IP.
- **Protocole** : Tout protocole.
- **Action** : Bloquer et journaliser le trafic.

> [!WARNING] Ne Pas Oublier
> Toujours inclure cette règle à la fin de la configuration pour éviter de bloquer les autres règles.

---
# Filtrage de paquets et de segments

- **Niveau 3 (réseau)** : Filtrage des **paquets IP**.
- **Niveau 4 (transport)** : Filtrage des **segments TCP/UDP**.
## Informations disponibles dans les messages

- **Protocoles** : IP, ICMP (niveau 3), TCP, UDP (niveau 4).
- **Interface** : Trafic entrant ou sortant.
- **Adresses** : Source et destination (hôte ou sous-réseau).
- **En-têtes** : Informations comme le TTL (temps de vie) et le TOS (type de service).
- **Flags TCP** : Utilisés pour le contrôle de la communication (ex : SYN, ACK).

![[Pasted image 20240930191045.png]]
### Explication :
1. **Interfaces du routeur** :
    
    - **Interface externe** (facing the Internet) : 192.168.1.0/24.
        - C'est l'interface qui est connectée à Internet ou à un réseau externe.
    - **Interface interne** (facing the local network) : 192.168.2.1/24.
        - Cette interface est connectée au réseau interne, avec plusieurs ordinateurs ayant des adresses dans la plage 192.168.2.x.

2. **Filtrage du trafic** :
    
    - Le routeur est configuré pour ne router le trafic entrant depuis l'extérieur qu'à une plage d'adresses IP spécifique dans le réseau interne.
    - Dans cet exemple, **le routeur permet uniquement le trafic vers 192.168.2.2 à 192.168.2.5**. Le trafic Internet ne peut pas atteindre les autres adresses IP du réseau interne (comme 192.168.2.6).

3. **Fonction de filtrage des paquets** :
    
    - Le routeur joue le rôle de **filtre de paquets**. Il examine les paquets en fonction de règles définies (ici, probablement des règles basées sur les adresses IP).
    - Cela permet de **restreindre l'accès** à certaines machines sur le réseau interne, tout en bloquant les autres.

---
# Filtrage par proxy (ou relai)
## Filtrage applicatif (gateway ou passerelle)

- Le proxy agit comme **un intermédiaire obligatoire** entre le client et le serveur.
	- Tout le trafic doit passer par le proxy, où il est contrôlé avant d'atteindre le serveur ou les systèmes internes.
	- Cela peut nécessiter une **configuration manuelle du client**, ou être transparent.
## Limitation du trafic par configuration

- Le proxy peut restreindre le trafic selon le **type d'application** (par exemple, Telnet, HTTP) ou **les fonctionnalités** (ex : bloquer les scripts, limiter les téléchargements).
## Audit et journalisation

- Le proxy conserve des **journaux des connexions**, permettant une analyse détaillée des requêtes et du trafic.
## Connexion maintenue

- Le proxy doit **maintenir la connexion** de bout en bout, car il interprète et contrôle le trafic, agissant ainsi comme un intermédiaire permanent entre le client et le serveur.

---
# Zone démilitarisée et Bastion
## Zone démilitarisée

La **zone démilitarisée** (DMZ) est un **sous réseau isolé** comme passerelle entre le réseau local à protéger et extérieur (intranet et extranet). La DMZ **fait partie du pare-feu**.
Cette zone intermédiaire permet d’exposer certains services aux utilisateurs externes sans pour autant donner un accès direct au réseau interne.
### Exemple de services exposés dans une DMZ:
- Serveur web
- Serveur DNS
- Serveurs de messagerie
## Bastion

Chaque hôte exposé dans la DMZ est un **bastion**.
- Il permet toutes les connexions entrantes. 
- Les connexions depuis la DMZ sont **possibles que vers l'extérieur**

> [!Warning] Confinement
> Interdiction d'utiliser la DMZ comme point d'entrée au réseau interne

![[Pasted image 20241007143415.png]]
## Comment les services exposés gèrent les requêtes

Les services exposés dans la DMZ peuvent soit :
- **traiter directement les requêtes** venant de l'extérieur sans passer par un système intermédiaire
- **rediriger ou reléguer les requêtes** à des serveurs situés dans le réseau interne à travers des proxy ou des relais
## Rôle de l'IDS

Surveille tout le traffic entrant et sortant des services exposés.
Si il détecte une activité suspecte, il envoie des alertes aux admins réseaux.
## Rôle de Honey Pot

Un Honey Pot est un système intentionnellement vulnérable placé dans la DMZ pour **attirer et piéger les attaquants** avant qu'ils n'atteignent les véritables serveurs ou services.

---
# Translation d'adresses / ports
## NAT (Network address translation)

Mécanisme permettant de traduire les IP privées d'un réseau interne en IP publiques pour accéder à Internet.
### Deux types
- **Statique** : chaque adresse interne a une correspondance fixe avec une adresse externe.
- **Dynamique** : l'adresse interne est associée dynamiquement à une adresse externe disponible.
## PAT (Port address translation)

Permet à plusieurs appareils du réseau interne de partager une même adresse IP publique en différenciant les connexions par le numéro de port.
## NAPT (Network address and port translation)

**Masque à la fois les adresses IP et les ports des appareils interne**, en les traduisant en adresses et ports externes visibles depuis internet.
### Fonctionnement général de NAT/NAPT

- **Masquage des adresses internes** : Les adresses IP privées et les ports internes sont cachés et traduits en adresses et ports publics lorsqu'ils accèdent à des ressources externes.
- **Modification de la destination des paquets** : Lorsqu'un paquet est reçu de l'extérieur, la destination est modifiée au niveau NAT pour être redirigée vers une adresse interne appropriée avant son routage.
- **Modification de la source des paquets** : Lorsqu'un paquet est émis depuis le réseau interne vers l'extérieur, l'adresse et le port source sont modifiés pour correspondre à l'interface publique du routeur.
- **Association adresses/ports internes et externes** : Le NAT/NAPT associe les adresses IP privées internes à une seule interface publique (par exemple, eth0, eth1).
- **Transfert de connexions (port forwarding)** : Permet de rediriger les connexions depuis un **port d'une adresse publique** vers un **port d'une adresse privée**.
    - Exemple : Transfert d'une connexion sur le port 80 (HTTP) vers le port 8080 d'un serveur interne.

![[Pasted image 20241007145737.png]]


---
TODO
- [x] Sécu : retrouver tous les types d'IPs privées 📅2024-10-07 [link](https://todoist.com/app/task/8443181623) #todoist %%[todoist_id:: 8443181623]%%
- [x] Sécu : regarder si mail = TCP ou UDP 📅2024-10-07 [link](https://todoist.com/app/task/8443181652) #todoist %%[todoist_id:: 8443181652]%%
- [x] Sécu : SOCKS 5 📅2024-10-07 [link](https://todoist.com/app/task/8443181684) #todoist %%[todoist_id:: 8443181684]%%
- [x] Sécu : Trouver un logiciel de honey pot sous linux 📆2024-10-14 [link](https://todoist.com/app/task/8465696102) #todoist  %%[todoist_id:: 8465696102]%%
- [x] Sécu : Toutes les adresses possibles des hôtes pour 172.16.0.0/12 avec adresse réseaux ett adresse 📆2024-10-14 [link](https://todoist.com/app/task/8465724536) #todoist %%[todoist_id:: 8465724536]%%
- [ ] Sécu : Faire TP 1 Proxy ex1 a,b,c, ex2,  finir 4 📆2024-10-14 [link](https://todoist.com/app/task/8465863626) #todoist %%[todoist_id:: 8465863626]%%
