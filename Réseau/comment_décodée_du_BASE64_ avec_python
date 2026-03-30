# 🧠 Script Python – Résolution d’un challenge réseau (Base64)

## 📌 Description

Ce script Python se connecte à un serveur distant via un socket TCP, récupère une chaîne encodée en Base64, la décode, puis renvoie la réponse au serveur.

---

## ⚙️ Fonctionnement détaillé

### 1. Import des modules

```python
import socket
import re
import time
import base64
```

* `socket` : permet la communication réseau
* `re` : utilisé pour extraire une partie du texte avec une expression régulière
* `time` : ajoute un délai avant l'envoi
* `base64` : permet de décoder la chaîne reçue

---

### 2. Définition du pattern

```python
pat = re.compile(r"my string is '(.*)'. What is your answer ?")
```

* Expression régulière utilisée pour capturer la chaîne Base64 envoyée par le serveur
* Le groupe `(.*)` récupère le contenu entre les guillemets

---

### 3. Connexion au serveur

```python
soc = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
soc.connect(("challenge01.root-me.org", 52023))
```

* Création d’un socket TCP (`AF_INET`, `SOCK_STREAM`)
* Connexion au serveur distant

---

### 4. Réception des données

```python
raw = soc.recv(10_000).decode('utf-8')
print(raw)
```

* Réception des données envoyées par le serveur
* Décodage en UTF-8
* Affichage du message brut

---

### 5. Extraction de la chaîne Base64

```python
b64 = re.findall(pat, raw)[0]
print(f'{b64=}')
```

* Extraction avec regex
* Récupération du premier résultat

---

### 6. Décodage Base64

```python
rep = base64.b64decode(b64.encode('utf-8')) + b'\n'
print(f'{rep=}')
```

* Encodage en bytes
* Décodage Base64
* Ajout d’un saut de ligne (`\n`) requis par le serveur

---

### 7. Envoi de la réponse

```python
time.sleep(1)
soc.send(rep)
```

* Petite pause (souvent nécessaire pour éviter les erreurs serveur)
* Envoi de la réponse décodée

---

### 8. Réception du résultat

```python
raw = soc.recv(10_000).decode('utf-8')
print(raw)
```

* Affichage de la réponse du serveur (souvent un flag ou un message de validation)

---

### 9. Fermeture de la connexion

```python
soc.close()
```

* Libération des ressources réseau

---

## 🚀 Résumé du flux

1. Connexion au serveur
2. Réception d’un message contenant une chaîne Base64
3. Extraction avec regex
4. Décodage Base64
5. Envoi de la réponse
6. Réception du résultat

---

## 💡 Remarques

* Ce type de script est typique des challenges CTF (Capture The Flag)
* Le délai (`time.sleep`) peut être important selon le serveur
* Toujours ajouter `\n` si le protocole le demande

---

## 🧪 Exemple d’entrée/sortie

**Entrée serveur :**

```
my string is 'SGVsbG8gd29ybGQ='. What is your answer ?
```

**Réponse envoyée :**

```
Hello world
```

---

## 🏁 Conclusion

Ce script automatise une tâche simple mais fréquente en cybersécurité :
➡️ Décoder une donnée reçue et répondre correctement via un protocole réseau.

---
