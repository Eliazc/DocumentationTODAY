# Mise à jour et gestion des paquets avec APT ou autre

## 1. Mise à jour des paquets sur différentes distributions

### Debian / Ubuntu / Linux Mint
```bash
# Mettre à jour la liste des paquets disponibles
sudo apt update

# Mettre à niveau les paquets installés (sans supprimer de paquets)
sudo apt upgrade

# Mettre à niveau avec gestion des dépendances (peut supprimer des paquets obsolètes)
sudo apt full-upgrade

# Nettoyer les paquets inutiles
sudo apt autoremove

# Mettre à jour tous les paquets pour fedora, centOS et RHEL 
sudo dnf upgrade

# Mettre à jour tous les paquets arch(btw)
sudo pacman -Syu

### Ajouter une clé avec `apt-key` (déconseillé sur les nouvelles versions d’Ubuntu) si vous avez un linux un peut vieux
```bash
# Télécharger et ajouter la clé GPG depuis keyserver.ubuntu.com (exemple avec la clé de Docker)
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 9DC858229FC7DD38854AE2D88D81803C0EBFCD88

# Ajouter le dépôt APT correspondant
echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu \$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list

# Mettre à jour la liste des paquets
sudo apt update


