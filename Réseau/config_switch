# Documentation de la configuration du switch Cisco

## 1. Informations générales

```bash
version 15.0
hostname switch0
service password-encryption
```

**Explication :**

* `version 15.0` : Version de l'IOS Cisco utilisée par l'équipement.
* `hostname switch0` : Définit le nom du switch. Ce nom apparaît dans le prompt CLI.
* `service password-encryption` : Chiffre les mots de passe en clair dans la configuration pour éviter qu'ils soient lisibles.

---

# 2. Sécurisation de l'accès au switch

## Mot de passe privilégié

```bash
enable secret 5 <hash>
```

**Explication :**

* Définit le mot de passe permettant d'accéder au **mode privilégié (enable)**.
* Le type `5` indique que le mot de passe est **haché en MD5**.

---

## Comptes utilisateurs

```bash
username admin secret 5 <hash>
username stagiaire secret 5 <hash>
```

**Explication :**

Création de deux comptes locaux :

| Utilisateur | Rôle                          |
| ----------- | ----------------------------- |
| admin       | Administrateur                |
| stagiaire   | Compte utilisateur secondaire |

Les mots de passe sont stockés sous forme **hachée (MD5)**.

---

# 3. Configuration SSH

```bash
ip ssh version 2
ip domain-name MediCare.intra
```

**Explication :**

* `ip ssh version 2` : Active **SSH version 2**, plus sécurisé que la version 1.
* `ip domain-name` : Nécessaire pour générer les **clés RSA** utilisées par SSH.

SSH permet l'administration **sécurisée à distance**.

---

# 4. DHCP Snooping (Sécurité réseau)

```bash
ip dhcp snooping
ip dhcp snooping vlan 10,20,30
```

**Explication :**

Le **DHCP Snooping** protège le réseau contre les **serveurs DHCP malveillants**.

Fonctionnement :

* Seuls les ports **trusted** peuvent envoyer des réponses DHCP.
* Les autres ports sont considérés **non fiables**.

VLAN protégés :

* VLAN 10
* VLAN 20
* VLAN 30

---

# 5. Spanning Tree

```bash
spanning-tree mode pvst
spanning-tree extend system-id
```

**Explication :**

* `pvst` : **Per VLAN Spanning Tree** → un arbre STP par VLAN.
* Évite les **boucles réseau** dans l'infrastructure.

---

# 6. Configuration des ports utilisateurs

## Exemple : FastEthernet0/1

```bash
interface FastEthernet0/1
 switchport access vlan 10
 switchport mode access
 ip dhcp snooping limit rate 5
 switchport port-security
 switchport port-security maximum 5
 switchport port-security mac-address sticky
 switchport port-security mac-address sticky 0050.0FE5.92E3
```

**Explication :**

| Commande                        | Fonction                         |
| ------------------------------- | -------------------------------- |
| `switchport mode access`        | Configure le port en mode access |
| `switchport access vlan 10`     | Affecte le port au VLAN 10       |
| `ip dhcp snooping limit rate 5` | Limite les requêtes DHCP         |
| `port-security`                 | Active la sécurité de port       |
| `maximum 5`                     | Maximum 5 adresses MAC           |
| `sticky`                        | Apprend automatiquement les MAC  |
| `mac-address sticky`            | Enregistre la MAC dans la config |

Objectif : **éviter les attaques type MAC flooding ou branchement non autorisé**.

---

# 7. Ports désactivés

Ports **FastEthernet 0/4 à 0/24**

```bash
switchport access vlan 66
switchport mode access
shutdown
```

**Explication :**

* Placés dans le **VLAN 66 (VLAN parking)**.
* `shutdown` désactive les ports.

Objectif :

* Empêcher les connexions physiques non autorisées.

---

# 8. Ports trunk

## GigabitEthernet0/1 et 0/2

```bash
interface GigabitEthernet0/1
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30
 ip dhcp snooping trust
 switchport mode trunk
 switchport nonegotiate
```

**Explication :**

| Commande                | Fonction                |
| ----------------------- | ----------------------- |
| `mode trunk`            | Autorise plusieurs VLAN |
| `allowed vlan 10,20,30` | VLAN autorisés          |
| `native vlan 99`        | VLAN natif du trunk     |
| `dhcp snooping trust`   | Port de confiance DHCP  |
| `nonegotiate`           | Désactive DTP           |

Utilisé pour connecter :

* un **routeur**
* un **autre switch**

---

# 9. Interfaces VLAN

## VLAN 30

```bash
interface Vlan30
 ip address 192.168.30.253 255.255.255.0
```

**Explication :**

Interface virtuelle utilisée pour :

* **la gestion du switch**
* accès réseau depuis le VLAN 30.

Adresse de management :

```
192.168.30.253
```

---

# 10. Bannière de sécurité

```bash
banner motd ^CACCES RESERVE!^C
```

Message affiché lors de la connexion.

Objectif :

* avertissement légal
* sécurité informatique.

---

# 11. Contrôle d'accès SSH

## ACL SSH

```bash
ip access-list standard ACCES_SSH
 permit 192.168.30.0 0.0.0.255
```

**Explication :**

Autorise uniquement les connexions SSH depuis :

```
192.168.30.0 /24
```

Empêche l'accès depuis les autres réseaux.

---

# 12. Accès console

```bash
line con 0
 login local
```

**Explication :**

Connexion physique via **port console**.

Authentification avec les **comptes locaux**.

---

# 13. Accès distant VTY (SSH)

```bash
line vty 0 4
 access-class ACCES_SSH in
 login local
 transport input ssh
 transport output ssh
```

**Explication :**

| Paramètre                | Fonction                   |
| ------------------------ | -------------------------- |
| `access-class ACCES_SSH` | applique l'ACL             |
| `login local`            | utilise les comptes locaux |
| `transport input ssh`    | autorise uniquement SSH    |

Telnet est **désactivé pour la sécurité**.

---

## Désactivation des lignes VTY supplémentaires

```bash
line vty 5 15
 access-class non in
 login
 transport input none
 transport output none
```

**Explication :**

* Bloque les connexions sur ces lignes.
* Réduit la surface d'attaque.

---

# 14. Résumé de la sécurité mise en place

Ce switch implémente plusieurs mécanismes de sécurité :

### Sécurité d'accès

* SSH uniquement
* ACL pour limiter l'accès
* comptes utilisateurs

### Sécurité réseau

* DHCP Snooping
* Port Security
* VLAN segmentation

### Sécurité physique

* Ports inutilisés **désactivés**
* VLAN parking

### Protection contre les boucles

* **Spanning Tree PVST**

---

# Conclusion

Cette configuration met en place un **switch sécurisé et segmenté** :

* séparation des réseaux avec **VLAN**
* contrôle des accès administrateur
* protection contre les attaques réseau courantes
* sécurisation des ports physiques.

Elle est adaptée à une **infrastructure d'entreprise ou de laboratoire pédagogique**.
