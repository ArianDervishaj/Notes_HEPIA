## Types d'adresse privées 

- **Classe A** : 10.0.0.0/8
- **Classe B** : 172.16.0.0/12
- **Classe C** : 192.168.0.0/16

Ces plages d'adresses IP sont réservées pour un usage privé (réseaux internes) et ne sont pas routables sur Internet.
## Mails

- **SMTP** (Simple Mail Transfer Protocol) : Utilisé pour **envoyer des emails**.
    - **Port 25** : Utilisé pour SMTP non sécurisé ou pour des communications entre serveurs de messagerie.
    - **Port 587** : Utilisé pour SMTP avec **authentification** et sécurisé (via STARTTLS).
- **POP3** (Post Office Protocol version 3) : Utilisé pour **récupérer des emails** sur un serveur, en les téléchargeant localement.
    - **Port 110** : Utilisé pour POP3 non sécurisé.
    - **Port 995** : Utilisé pour POP3 avec **chiffrement SSL/TLS**.
- **IMAP** (Internet Message Access Protocol) : Utilisé pour **accéder et gérer des emails** directement sur le serveur, sans les télécharger.
    - **Port 143** : Utilisé pour IMAP non sécurisé.
    - **Port 993** : Utilisé pour IMAP avec **chiffrement SSL/TLS**.
## SOCKS 5

SOCKS5 est un protocole réseau qui permet de **faire transiter du trafic** (données) entre un client et un serveur à travers un **proxy**.

- **Protocole indépendant** : Fonctionne avec **n'importe quel type de trafic** (HTTP, FTP, emails, etc.).
- **Supporte TCP et UDP** :
    - **TCP** : Utilisé pour des communications fiables (par exemple, transfert de fichiers).
    - **UDP** : Utilisé pour des communications rapides (par exemple, jeux en ligne).

**Pas de chiffrement** : Contrairement à un VPN, SOCKS5 ne chiffre pas les données, il ne fait que les rediriger. (Peut être utilisé en combinaison aves des protocoles chiffrés comme HTTPs)