# Configuration DHCP Snooping contre le DHCP Spoofing

```bash
enable
configure terminal

! Active le DHCP Snooping globalement
ip dhcp snooping

! Active le DHCP Snooping sur le VLAN 1
ip dhcp snooping vlan 1

! Configure l'interface GigabitEthernet0/1 comme "trusted" (confiance)
interface gigabitEthernet0/1
 ip dhcp snooping trust
 ip dhcp snooping information option
 exit

! Limite le taux de requêtes DHCP sur les ports FastEthernet 0/1-24
interface range fastEthernet 0/1-24
 ip dhcp snooping limit rate 4
 exit

! Sauvegarde la configuration
write memory
