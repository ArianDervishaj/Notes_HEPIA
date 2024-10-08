# Installation des machines

#### H4
```bash
dhclient -v mgmt0
apt update
apt -y --no-install-recommends install git python3-pip python-is-python3 tcpreplay
pip3 install scapy
git clone https://gitedu.hesge.ch/hoerdt-public/sendether
cd sendether && chmod 755 sendether.py && cp sendether.py /usr/local/bin
hostnamectl set-hostname H1
ip addr add 10.0.0.1/24 dev eth0
ip link set up dev eth0
ip link set dev eth0 addr 00:00:00:00:00:01
exit
```

#### H2
```bash
hostnamectl set-hostname H2
ip addr add 10.0.0.2/24 dev eth0
ip link set up dev eth0
ip link set dev eth0 addr 00:00:00:00:00:02
exit
```

#### H3
```bash
hostnamectl set-hostname H3
ip addr add 10.0.0.3/24 dev eth0
ip link set up dev eth0
ip link set dev eth0 addr 00:00:00:00:00:03
exit
```

---
## A Fonctionnement normal d'un switch
### 1. Expliquez la sortie de la commande `show mac-address-table aging-time` exécutée sur le switch S1 ?

> La commande permet de voir le temps d'expiration des adresses MAC dans la table d'adresses du switch en seconde.

### 2. Expliquez la sortie de la commande `show mac-address-table count` exécutée sur S1 ?

![[Pasted image 20241001135318.png]]
>La commande permet d'afficher le nombre d'entrées dans la table d'adresse de S1.
>0 adresse dynamique, 0 adresse MAC securisé, 0 statique. Le système possède une adresse MAC propre.
>Le Max de la table est de 8192.
>Le switch n'a pas encore appris d'adresses MAC sur le réseau => pas encore eu de traffic passant par S1.

### 3. Donner, en théorie, la valeur de la table d’adresse mac des switches S1, S2 et S3 après un ping exécuté depuis H1 vers H2 et H3 il y a moins de 5 min ?

| S1  | Name | Port |
| --- | ---- | ---- |
|     | H1   | f1/4 |
|     | H2   | f1/1 |
|     | H3   | f1/2 |

| S2  | Name | Port |
| --- | ---- | ---- |
|     | H1   | f1/1 |
|     | H2   | f1/4 |

| S3  | Name | Port |
| --- | ---- | ---- |
|     | H1   | f1/2 |
|     | H3   | f1/4 |

### 4. Donner la commande, en une ligne, à exécuter sur H1 pour pinger H2 et H3 ? Exécuter cette commande et vérifier la valeur des tables d’adresse mac de S1, S2 et S3 avec celle que vous avez donné dans la question précédente. Expliquez comment cette table s’est construite ?

```bash
ping -c 5 10.0.0.2 & ping -c 5 10.0.0.3
```

### 5. Depuis H1, pinger H2 (`ping 10.0.0.2`). Observer le trafic sur le lien entre S1 et S2, puis entre S1 et S3. Expliquez pourquoi on ne voit pas les messages ICMP sur le lien entre S1 et S3 ?

![[Pasted image 20241001141843.png]]
![[Pasted image 20241001141902.png]]
ICMP entre 1 -> 2 car S1 a déjà dans sa table l'adresse de 2 donc va pas passer par S3.
## B Injection de trames sur le réseau
### 1. Expliquez les valeurs dans les tables d’adresses MAC de S1, S2 et S3 suite à l’exécution de la commande `sendether.py -i eth0` sur H1 ?
### 2. Que se passe-t-il sur les tables des switches S1, S2 et S3 après exécution de la commande `sendether.py -i eth0 -c 5` ? Expliquez et détaillez votre réponse à partir des sources de l’utilitaire `sendether.py` disponibles sur [https://gitedu.hesge.ch/hoerdt-public/sendether/-/blob/master/sendether.py](https://gitedu.hesge.ch/hoerdt-public/sendether/-/blob/master/sendether.py)
### 3. Sur le programme `/usr/local/bin/sendether.py` installé sur H1, Inversez l’adresse mac source et l’adresse mac destination de la trame à la ligne 23 de sendether ([https://gitedu.hesge.ch/hoerdt-public/sendether/-/blob/master/sendether.py#L23](https://gitedu.hesge.ch/hoerdt-public/sendether/-/blob/master/sendether.py#L23)). Re-exécutez sendether.py 2-3 fois. Que remarquez-vous sur les tables d’adresses MAC des switches suite à cette inversion ? Expliquez pourquoi ?

