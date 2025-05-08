# Comprendre et utiliser les clés SSH

> Un guide pas-à-pas pour générer, configurer et utiliser une clé SSH en toute sécurité, notamment avec GitHub.

## 📘 Introduction

Lorsque vous interagissez avec des dépôts GitHub, vous avez deux options principales pour l’authentification :

- Via **HTTPS** : demande le mot de passe (ou un token) à chaque fois.

- Via **SSH** : utilise une **clé cryptographique** pour vous authentifier **automatiquement et en toute sécurité**.

> Résultat : plus de saisie de mot de passe, une meilleure sécurité, et une configuration professionnelle.

## 🧠 Pourquoi utiliser une clé SSH ?

| Méthode | Avantages                     | Inconvénients                        |
| ------- | ----------------------------- | ------------------------------------ |
| HTTPS   | Simple à comprendre           | Demande un mot de passe/token        |
| SSH     | Automatique, sécurisé, rapide | Nécessite une configuration initiale |

L’utilisation de SSH est **recommandée par GitHub** si vous travaillez régulièrement depuis une machine personnelle ou professionnelle.

## 🛠️ Étapes pour générer et utiliser une clé SSH

### 1. Vérifier si vous avez déjà une clé SSH

Dans votre terminal :

```bash
ls ~/.ssh
```

Vous pourriez voir des fichiers comme :

- `id_rsa` et `id_rsa.pub` (clé privée et clé publique RSA)

- `id_ed25519` et `id_ed25519.pub` (clé ED25519, plus moderne)

Si aucun fichier ne correspond, vous pouvez en créer une.

### 2. Générer une nouvelle paire de clés

Commande recommandée (clé moderne, plus sécurisée que RSA) :

```bash
ssh-keygen -t ed25519 -C "votre.email@example.com"
```

Si votre système ne supporte pas ed25519 :

```bash
ssh-keygen -t rsa -b 4096 -C "votre.email@example.com"
```

#### Ce que fait cette commande :

- `-t` : type de clé (ed25519 ou rsa)

- `-C` : un commentaire pour identifier la clé (souvent votre e-mail)

#### Vous verrez alors :

```bash
Enter file in which to save the key (/home/user/.ssh/id_ed25519):
```

Appuyez sur **Entrée** pour accepter l’emplacement par défaut.

Puis :

```bash
Enter passphrase (empty for no passphrase):
```

> 💡 Une passphrase ajoute une couche de sécurité à votre clé privée (recommandé si vous partagez l'ordi).

### 3. Ajouter la clé SSH à l’agent SSH

L’agent SSH est un programme qui garde votre clé en mémoire pour éviter de la déverrouiller à chaque fois.

#### Démarrer l’agent (Linux/macOS) :

```bash
eval "$(ssh-agent -s)"
```

#### Ajouter votre clé :

```bash
ssh-add ~/.ssh/id_ed25519
```

> 💡 Si vous utilisez Windows avec Git Bash ou WSL, c’est très similaire. Avec Windows 10+, vous pouvez aussi activer l’agent dans ssh-agent.

### 4. Ajouter la clé publique à GitHub

#### Copier la clé publique dans le presse-papiers :

```bash
cat ~/.ssh/id_ed25519.pub
```

Copiez le **contenu complet**, puis :

1. Connectez-vous à https://github.com

<img width="680" alt="Capture d’écran 2025-05-08 à 22 33 38" src="https://github.com/user-attachments/assets/5e770f78-497d-4d31-9ced-00fdb56258bf" />

2. Allez dans : Settings

<img width="201" alt="Capture d’écran 2025-05-08 à 22 34 29" src="https://github.com/user-attachments/assets/22b6da2d-8729-4204-94dd-102c82573761" />

3. Puis SSH and GPG keys

<img width="201" alt="Capture d’écran 2025-05-08 à 22 35 22" src="https://github.com/user-attachments/assets/8bc084aa-1d77-43a2-a233-a8bb739a0979" />

4. Cliquez sur New SSH key

<img width="572" alt="Capture d’écran 2025-05-08 à 22 36 00" src="https://github.com/user-attachments/assets/6ef3a53e-14b4-4376-a8a8-5a3188a9e382" />

6. Donnez-lui un nom (ex : MacBook de Mathieu)

<img width="590" alt="Capture d’écran 2025-05-08 à 22 36 27" src="https://github.com/user-attachments/assets/d2c4e3b3-a4f2-4c67-a2c1-cf82d2175a96" />

7. Collez la clé dans la zone prévue

8. Cliquez sur Add SSH key

### 5. Tester la connexion

```bash
ssh -T git@github.com
```

Si tout est bien configuré, vous verrez :

```bash
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

> ✅ Votre machine est maintenant autorisée à interagir avec GitHub sans mot de passe.

## 🧽 Bonnes pratiques

- **Ne partagez jamais votre clé privée** (`id_ed25519` ou `id_rsa`)

- **Ajoutez une passphrase** à votre clé si vous travaillez sur un portable ou un ordi partagé

- **Stockez votre clé privée uniquement dans** `~/.ssh/`

- **N’ajoutez pas les clés au dépôt Git ou en pièce jointe**

## 🧰 Astuces utiles

| Commande                         | Description                              |
| -------------------------------- | ---------------------------------------- |
| `ssh-keygen`                     | Crée une nouvelle paire de clés          |
| `ssh-add`                        | Ajoute une clé à l’agent SSH             |
| `ssh -T git@github.com`          | Teste la connexion                       |
| `pbcopy < ~/.ssh/id_ed25519.pub` | (macOS) Copie la clé publique            |
| `clip < ~/.ssh/id_ed25519.pub`   | (Windows Git Bash) Copie la clé publique |

## 🚀 Aller plus loin

- [Clés SSH sur GitHub – doc officielle](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

- [Utiliser plusieurs clés SSH](https://gist.github.com/jexchan/2351996)

- [Authentification sécurisée SSH avec YubiKey (niveau avancé)](https://developers.yubico.com)

## 🎉 Félicitations !

Vous êtes désormais capable de :
- Générer une clé SSH sécurisée

- La connecter à GitHub

- Travailler sans entrer votre mot de passe à chaque push ou pull

Bon développement !
