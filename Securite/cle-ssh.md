# Comprendre et utiliser les clés SSH

> Un guide pas-à-pas pour générer, configurer et utiliser une clé SSH pour travailler efficacement avec GitHub sans jamais retaper son mot de passe, et en toute sécurité.

## 📘 Introduction

Quand on commence à utiliser GitHub (ou n’importe quel dépôt Git distant), une question revient très vite :

> ❓ Comment puis-je m'authentifier de manière simple, sécurisée et professionnelle ?

Beaucoup commencent avec **HTTPS**, car GitHub propose cette URL par défaut. Mais rapidement, cela devient fastidieux :

à chaque git push ou git pull, Git demande un mot de passe ou un **token d’accès personnel** (depuis 2021).

➡️ C’est là qu’interviennent les **clés SSH**, qui permettent :

- Une **authentification silencieuse** (pas besoin de saisir quoi que ce soit)

- Une **sécurité renforcée** (cryptographie asymétrique)

- Une **approche pro**, standard chez les devs expérimentés

## 🧠 Qu’est-ce qu’une clé SSH ?

Une **clé SSH** est un **mécanisme d’authentification** basé sur un système de **cryptographie asymétrique**.
Concrètement, elle est composée de **2 fichiers** générés ensemble :

| Fichier          | Rôle                                 | Exemple                 |
| ---------------- | ------------------------------------ | ----------------------- |
| `id_ed25519`     | 🔒 Clé **privée** (à garder secrète) | `~/.ssh/id_ed25519`     |
| `id_ed25519.pub` | 🔓 Clé **publique** (à partager)     | `~/.ssh/id_ed25519.pub` |

Le **principe** est simple :

1. Vous **générez** une paire de clés.

2. Vous **gardez la clé privée** sur votre machine.

3. Vous **ajoutez la clé publique** à GitHub.

4. GitHub **vous reconnaît** automatiquement à chaque connexion via cette paire.

> 📌 Contrairement à un mot de passe, vous n’avez **rien à taper** à chaque action Git !

## 📦 Pourquoi utiliser SSH avec GitHub ?

### Exemple concret :

Supposons que vous travailliez sur plusieurs projets, avec plusieurs dépôts GitHub.

En HTTPS, à chaque git pull :

```bash
Username: mathieu
Password: **************
```

➡️ Ça casse le rythme.

Avec SSH, vous tapez simplement :

```bash
git pull
```

✅ Et c’est tout. GitHub sait déjà que c’est vous, grâce à la clé.

### Résumé des avantages :

| HTTPS                                    | SSH                                           |
| ---------------------------------------- | --------------------------------------------- |
| Nécessite un token ou mot de passe       | Authentification automatique                  |
| Moins sécurisé (surtout si mot de passe) | Clé cryptée avec passphrase en option         |
| Demande des identifiants à chaque action | Une fois configuré, plus besoin d’interaction |
| Facile à mettre en place (au début)      | Demande une configuration initiale            |

## 🧰 Étapes détaillées pour générer et utiliser une clé SSH

### 🔍 Étape 1 – Vérifier si une clé SSH existe déjà

Avant de générer une nouvelle clé, il est **utile de vérifier** si une clé existe déjà sur votre machine.

Dans votre terminal (macOS / Linux / Git Bash Windows) :

```bash
ls -al ~/.ssh
```

Cela liste les fichiers du dossier `~/.ssh`.

Vous pouvez y voir des fichiers comme :

- `id_rsa` / `id_rsa.pub` (ancienne norme, RSA)

- `id_ed25519` / `id_ed25519.pub` (norme actuelle, plus rapide et plus sécurisée)

> 🔑 Si vous trouvez déjà une **clé publique existante** et que vous savez à quoi elle sert, vous pouvez l'utiliser. Sinon, générons-en une nouvelle.

### 🛠️ Étape 2 – Générer une nouvelle paire de clés

> 🔐 Recommandé : **ED25519**, plus rapide et plus sûr que RSA.

Dans le terminal :

```bash
ssh-keygen -t ed25519 -C "votre.email@example.com"
```

> Remplacez bien sûr l’e-mail par celui lié à votre compte GitHub.

#### 👉 Explication de la commande :

- `ssh-keygen` : outil de génération de clé

- `-t ed25519` : type de clé à générer

- `-C` : un commentaire pour reconnaître votre clé (souvent un e-mail)

#### Le terminal vous demande :

```bash
Enter file in which to save the key (/home/mathieu/.ssh/id_ed25519):
```

✅ Appuyez sur Entrée pour accepter le nom et l’emplacement par défaut.

```bash
Enter passphrase (empty for no passphrase):
```

👉 Ici, vous pouvez :

- Soit appuyer sur Entrée pour **aucune passphrase**

- Soit saisir une **phrase secrète** (comme un mot de passe) pour plus de sécurité

> 🔐 **Bonne pratique** : utiliser une passphrase si vous êtes sur un ordinateur portable ou partagé.

### 📂 Étape 3 – Localiser vos fichiers SSH

Une fois la clé créée, vous trouverez dans `~/.ssh` :

- `id_ed25519` → **votre clé privée** : ne la partagez JAMAIS

- `id_ed25519.pub` → **votre clé publique** : à copier dans GitHub

### 🚀 Étape 4 – Ajouter la clé au SSH Agent

Le **SSH Agent** est un programme qui garde votre clé déverrouillée en mémoire pendant que vous travaillez.

Démarrer l’agent (s’il n’est pas déjà actif) :

```bash
eval "$(ssh-agent -s)"
```

Puis, ajoutez la clé :

```bash
ssh-add ~/.ssh/id_ed25519
```

> 🔑 Si vous avez utilisé une passphrase, elle vous sera demandée maintenant.

### 🌐 Étape 5 – Ajouter la clé à votre compte GitHub

1. **Copier la clé publique**

```bash
cat ~/.ssh/id_ed25519.pub
```

Cela affichera une longue ligne commençant par :

```css
ssh-ed25519 AAAAC3Nz... votre.email@example.com
```

**➡️ Copiez TOUT.**

2. **Aller dans GitHub**

- Connectez-vous à GitHub

- Cliquez sur votre photo > `Settings`

- Menu gauche → `SSH and GPG keys`

- Cliquez sur **"New SSH key"**

- **Title** : nom de votre machine (ex. “MacBook Pro perso”)

- **Key** : collez votre clé publique

- Cliquez sur **Add SSH key**

### 🧪 Étape 6 – Tester la connexion SSH

Dans le terminal :

```bash
ssh -T git@github.com
```

Première fois : GitHub vous demande si vous faites confiance à `github.com`. Tapez `yes`.

Puis vous verrez :

```bash
Hi mathieu! You've successfully authenticated, but GitHub does not provide shell access.
```

✅ C’est bon, tout fonctionne !

## 📝 Résumé des commandes utiles

| Commande                          | Description                        |
| --------------------------------- | ---------------------------------- |
| `ssh-keygen -t ed25519 -C "mail"` | Génère une paire de clés SSH       |
| `eval "$(ssh-agent -s)"`          | Lance l’agent SSH                  |
| `ssh-add ~/.ssh/id_ed25519`       | Ajoute la clé à l’agent            |
| `cat ~/.ssh/id_ed25519.pub`       | Affiche la clé publique            |
| `ssh -T git@github.com`           | Teste la connexion SSH vers GitHub |
| `ls -al ~/.ssh`                   | Liste les clés déjà présentes      |

## 🔐 Bonnes pratiques

- **Clé privée = secret absolu** → Ne l’envoyez **jamais** par mail, Slack, ou dans un dépôt Git.

- **Utilisez une passphrase si vous travaillez sur un portable.**

- **Sauvegardez vos clés SSH** dans un gestionnaire sécurisé (si vous formatez votre PC, la clé est perdue sinon).

- **N’ajoutez jamais `~/.ssh` dans un dépôt Git**. Ajoutez `id_*` dans `.gitignore` si nécessaire.

- **Nommez vos clés si vous en avez plusieurs** (ex : `id_work_github`, `id_personal_gitlab`).

## 💡 Astuces avancées (facultatif)

- Utiliser **plusieurs clés SSH** (ex : une perso pour GitHub, une pro pour GitLab) → nécessite un `config` SSH

- Changer la **durée de vie d’une clé dans l’agent**

- Utiliser un **YubiKey ou autre clé matérielle** pour stocker votre clé privée

## 🎓 Pour approfondir

Voici quelques ressources utiles si tu veux aller plus loin avec SSH et GitHub :

- 📚 [Guide GitHub officiel sur les clés SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

- 🔐 [Article détaillé sur les clés ED25519 vs RSA](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

- 🧠 [StackOverflow – FAQ sur les erreurs SSH](https://stackoverflow.com/questions/tagged/ssh)

## 🌟 Conclusion

Les clés SSH peuvent sembler techniques au premier abord, mais elles font rapidement partie de l’environnement naturel d’un développeur. 

Une fois configurées, elles vous permettent de :

- ✅ Travailler plus vite

- ✅ Travailler plus proprement

- ✅ Travailler plus sereinement

Et surtout, elles montrent que **vous êtes à l’aise avec les outils pro du quotidien**.

> 🚀 **Configurer une clé SSH, c’est comme donner un badge sécurisé à votre machine pour qu’elle puisse entrer sans frapper**. Une fois ce badge validé par GitHub, la porte reste ouverte.

Alors n’hésitez plus : mettez en place vos clés SSH, et concentrez-vous sur ce qui compte vraiment : **coder**.

## ✍️ Author

**Mathieu MOREL**. 🔗 [Linkedin](https://www.linkedin.com/in/mathieumorel62/)

📍 Lille · 👨‍💻 Développeur Fullstack · 💼 Freelance · 👨‍🏫 Coach Technique chez [Holberton School](https://www.holbertonschool.fr/?utm_campaign=MV-Pmax&utm_medium=cpc&utm_source=google)
