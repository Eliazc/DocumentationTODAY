# Configuration d'un Routeur Cisco – Projet Réseau

## Présentation

Ce projet consiste à configurer un routeur Cisco IOS afin de permettre :

* le routage inter-VLAN
* le relais DHCP vers un serveur distant
* l’administration sécurisée en SSH
* le contrôle d’accès via ACL

Le routeur est configuré selon une architecture **Router-on-a-Stick**, permettant de gérer plusieurs VLANs via une seule interface physique.

---

# Architecture réseau

Le réseau est composé :

* d’un routeur Cisco
* d’un switch configuré en trunk
* de trois VLANs
* d’un serveur DHCP

```
                DHCP Server
                 10.0.0.1
                     │
               10.0.0.254
                 Router0
                     │
               Trunk 802.1Q
                     │
                   Switch
         ┌───────────┼───────────┐
         │           │           │
       VLAN10      VLAN20      VLAN30
   192.168.10.0 192.168.20.0 192.168.30.0
   GW .254       GW .254       GW .254
```

---

# Informations système

```cisco
version 12.4
hostname Router0
```

### Explication

* `version 12.4` : version du système Cisco IOS
* `hostname Router0` : nom attribué au routeur pour l’identification dans le réseau

---

# Sécurité des mots de passe

```cisco
service password-encryption
enable secret 5 <hash>
```

### Explication

* `service password-encryption` : chiffre les mots de passe dans la configuration
* `enable secret` : mot de passe utilisé pour accéder au mode privilégié

Le type **5** correspond à un **hash MD5**.

---

# Gestion des utilisateurs

```cisco
username admin privilege 15 secret 5 <hash>
username stagiaire secret 5 <hash>
```

### Explication

Deux comptes locaux sont configurés :

| Utilisateur | Niveau | Description                  |
| ----------- | ------ | ---------------------------- |
| admin       | 15     | accès administrateur complet |
| stagiaire   | 1      | accès utilisateur limité     |

Les mots de passe sont stockés sous forme chiffrée.

---

# Accès distant sécurisé (SSH)

```cisco
ip ssh version 2
ip domain-name MediCare.intra
```

### Explication

* `ip ssh version 2` : activation du protocole SSH version 2
* `ip domain-name` : nécessaire pour générer les clés RSA

SSH permet une administration sécurisée du routeur à distance contrairement à Telnet.

---

# Routage inter-VLAN (Router-on-a-Stick)

Interface physique utilisée comme trunk :

```cisco
interface FastEthernet0/0
 no ip address
 duplex auto
 speed auto
```

Les VLANs sont configurés sous forme de sous-interfaces.

---

# VLAN 10

```cisco
interface FastEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.254 255.255.255.0
 ip helper-address 10.0.0.1
```

### Rôle

* passerelle du réseau **192.168.10.0/24**
* relais DHCP vers **10.0.0.1**

---

# VLAN 20

```cisco
interface FastEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.254 255.255.255.0
 ip helper-address 10.0.0.1
```

### Rôle

* passerelle du réseau **192.168.20.0/24**
* relais DHCP vers le serveur distant

---

# VLAN 30

```cisco
interface FastEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.254 255.255.255.0
 ip helper-address 10.0.0.1
```

### Rôle

* passerelle du réseau **192.168.30.0/24**
* relais DHCP vers le serveur

---

# Réseau du serveur DHCP

```cisco
interface FastEthernet0/1
 ip address 10.0.0.254 255.255.255.0
 duplex auto
 speed auto
```

### Explication

Cette interface connecte le routeur au réseau du serveur DHCP :

```
10.0.0.0 /24
```

Serveur DHCP :

```
10.0.0.1
```

---

# Relais DHCP

Dans chaque VLAN :

```cisco
ip helper-address 10.0.0.1
```

### Fonction

Cette commande permet de :

* transmettre les requêtes DHCP broadcast
* vers un serveur DHCP distant

Sans cela, les clients des VLANs ne recevraient pas d’adresse IP.

---

# Liste de contrôle d’accès (ACL)

```cisco
ip access-list standard ACCES_SSH
 permit 192.168.30.0 0.0.0.255
```

### Explication

Cette ACL autorise uniquement le réseau :

```
192.168.30.0/24
```

à accéder au routeur en SSH.

Cela signifie que **seul le VLAN 30 peut administrer le routeur**.

---

# Configuration des lignes VTY

```cisco
line vty 0 4
 access-class ACCES_SSH in
 login local
 transport input ssh
 transport output ssh
```

### Explication

* accès distant limité aux **5 premières sessions**
* authentification via **base locale**
* connexion possible uniquement en **SSH**

---

# Désactivation des lignes VTY supplémentaires

```cisco
line vty 5 15
 access-class non in
 login
 transport input none
 transport output none
```

### Explication

Les lignes **VTY 5 à 15 sont désactivées** afin de :

* réduire la surface d’attaque
* limiter les connexions distantes

---

# Bannière de sécurité

```cisco
banner motd ^CACCES RESERVE!^C
```

Message affiché lors de la connexion :

```
ACCES RESERVE!
```

Ce message indique que l'accès au système est réservé aux utilisateurs autorisés.

---

# Fonctionnalités principales

Le routeur assure les fonctions suivantes :

* routage inter-VLAN
* relais DHCP
* administration sécurisée en SSH
* authentification locale
* filtrage des accès via ACL
* segmentation du réseau en VLAN

---

# Objectif du projet

Mettre en place une infrastructure réseau sécurisée permettant :

* la segmentation du réseau
* l’attribution automatique des adresses IP
* l’administration sécurisée des équipements réseau
