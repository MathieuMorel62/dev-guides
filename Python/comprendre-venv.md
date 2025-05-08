![ChatGPT Image 8 mai 2025 à 19_19_17](https://github.com/user-attachments/assets/6c358dc2-ed41-43d6-b068-522896fa148e)

> Un guide pas-à-pas pour bien débuter avec `venv`, `pip`, et la gestion des dépendances en Python.

## 📘 Introduction

Lorsque vous débutez un projet Python, une des premières questions à se poser est la suivante :

> *Comment puis-je m'assurer que ce projet sera propre, stable, reproductible, et indépendant des autres projets Python sur mon ordinateur ?*

La réponse : **utiliser un environnement virtuel.**

### Qu'est-ce qu'un environnement virtuel ?

Un environnement virtuel, ou *virtual environment* en anglais, est un **espace de travail isolé** dans lequel vous pouvez :
- Installer des bibliothèques spécifiques uniquement à un projet ;
- Tester des versions précises de modules sans interférer avec le reste du système ;
- Partager un projet avec ses dépendances facilement grâce à un fichier de configuration.

En d'autres termes, un environnement virtuel permet d'avoir une bulle Python locale qui vous appartient, propre à chaque projet.

## 🧠 Pourquoi est-ce si important ?

Imaginez que vous travaillez sur deux projets :

- **Projet A** utilise `Django 3.2`
- **Projet B** utilise `Django 4.0`

Sans environnement virtuel, l'installation de l'un écraserait l'autre.  
Avec un `venv`, chaque projet a sa propre version, sans conflit.

### Résumé des avantages

| Sans venv | Avec venv |
|-----------|-----------|
| Modules partagés, risques de conflits | Modules isolés par projet |
| Projets difficiles à reproduire | Reproductibilité assurée |
| Problèmes fréquents de compatibilité | Gestion fiable des versions |
| Risques d'impacts globaux | Aucun risque pour le système |

> ✏️ **Remarque :** L'environnement virtuel est l'équivalent Python de ce que sont les environnements `node_modules` en JavaScript (Node.js).

## 🧰 Historique et outils existants

Avant Python 3.3, la gestion des environnements virtuels passait par des outils externes comme `virtualenv`.  
Depuis Python 3.3, une commande intégrée dans la bibliothèque standard est disponible : **`venv`**.

Cela signifie que vous n'avez **rien à installer** pour commencer à créer des environnements virtuels — tout est déjà prêt dans Python lui-même.

## ⚙️ Mise en place d'un environnement virtuel

Dans cette section, vous allez apprendre à créer, activer, utiliser et supprimer un environnement virtuel de manière propre.

### 1. Créer un dossier pour votre projet

Avant tout, organisons notre travail. Créez un nouveau dossier pour héberger votre projet :

```bash
mkdir mon_projet
cd mon_projet
```

Pourquoi ? Car tout ce qui concerne ce projet — code, dépendances, fichiers de configuration — sera contenu ici. C'est une **bonne pratique de base** pour tous vos développements.

### 2. Créer l'environnement virtuel
Vous pouvez maintenant créer un environnement virtuel dans ce dossier :

```python
# Windows
python -m venv env

# macOS / Linux
python3 -m venv env
```

>💡 Le dossier `env/` contiendra l'interpréteur Python, les outils pip, et les packages installés localement.

Pourquoi l'appeler `env` ? C'est une convention, mais vous pouvez aussi le nommer `.venv`, `venv`, ou `mon_env` selon vos préférences.

### 3. Activer l'environnement virtuel
Avant de pouvoir utiliser le venv, vous devez l'activer.

| Système       | Commande                  |
| ------------- | ------------------------- |
| Windows       | `.\env\Scripts\activate`  |
| macOS / Linux | `source env/bin/activate` |

Une fois activé, vous verrez quelque chose comme ceci dans votre terminal :

<img width="730" alt="Capture d’écran 2025-05-08 à 18 48 13" src="https://github.com/user-attachments/assets/d472cd0b-d9db-49c2-b285-4ff7100b757d" />

Ce `(env)` au début de la ligne confirme que vous êtes **dans l'environnement virtuel**.

### 4. Installer des bibliothèques dans le venv
Lorsque le venv est activé, les bibliothèques installées via `pip` sont enregistrées dans **le dossier `env/`** uniquement.

Exemple :

```python
pip install pyfiglet emoji
```

> ✏️ Ces deux bibliothèques sont utilisées dans le script de test pour générer du texte stylisé et des emojis. Ne vous inquiétez pas, vous les verrez en action plus loin !

### 5. Tester le bon fonctionnement
Nous allons maintenant créer un petit script qui montre que :
- Les bibliothèques externes sont bien installées.
- Le venv est activé.
- Tout fonctionne correctement.

Fichier `test_venv_fun.py` :

```python
try:
    import pyfiglet
    from emoji import emojize

    print(pyfiglet.figlet_format("Hello Venv!"))
    print(emojize("Tout fonctionne parfaitement :rocket:", language="alias"))

except ModuleNotFoundError as e:
    print("❌ Une erreur s'est produite :", e)
    print("➡️ Activez votre venv et installez les dépendances :")
    print("   source env/bin/activate")
    print("   pip install -r requirements.txt")
```

Lancez le script avec la commande :

```python
python3 test_venv_fun.py
```

Si tout se passe bien, vous devriez voir apparaître :

<img width="737" alt="Capture d’écran 2025-05-08 à 18 09 12" src="https://github.com/user-attachments/assets/bfde798c-7a2c-48de-a253-557fc45a8bfc" />

## 📋 Le fichier requirements.txt
Le fichier `requirements.txt` est **un élément clé dans la gestion de projet Python**.
Il contient une **liste figée des bibliothèques** nécessaires à votre projet, avec leur version exacte.

### Pourquoi est-ce utile ?

- Pour recréer l'environnement à l'identique sur une autre machine
- Pour collaborer avec d'autres développeurs
- Pour déployer votre projet sur un serveur ou une plateforme cloud

### Comment le créer ?

```python
pip freeze > requirements.txt
```

Cela génère un fichier contenant les bibliothèques **actuellement installées dans le venv**.

### Exemple de contenu :

<img width="430" alt="Capture d’écran 2025-05-08 à 18 12 28" src="https://github.com/user-attachments/assets/1a5384be-ffea-4fa1-bc70-501a51d6b803" />

> 📌 Vous pouvez maintenant partager ce fichier. Toute personne pourra recréer votre environnement avec :

```python
pip install -r requirements.txt
```

## 📦 Supprimer un venv
Un `venv` n'est qu'un dossier. Pour le supprimer, il suffit de le retirer :

```bash
rm -r env
```

## 🔐 Protéger votre dépôt Git

> ⚠️ Le dossier `env/` ne doit jamais être inclus dans votre dépôt Git.

Ajoutez dans votre fichier .gitignore :

```bash
env/
```

## ✅ Bonnes pratiques à retenir

- Créez un venv pour chaque projet Python
- Ne jamais installer de dépendances globales inutilement
- Versionnez toujours un requirements.txt
- Organisez votre projet avec un dossier dédié
- Ajoutez un .gitignore pour exclure l'environnement virtuel

## 🔍 Commandes utiles au quotidien

Voici un récapitulatif des commandes les plus utilisées avec votre environnement virtuel :

| Commande | Description |
|----------|-------------|
| `pip list` | Affiche toutes les bibliothèques installées |
| `pip show [package]` | Affiche les détails d'un package |
| `pip install --upgrade [package]` | Met à jour un package |
| `pip uninstall [package]` | Désinstalle un package |
| `deactivate` | Désactive l'environnement virtuel |

## 🚀 Aller plus loin

### Utilisation avec VS Code

VS Code détecte automatiquement les environnements virtuels. Pour en profiter :

1. Ouvrez votre projet dans VS Code
2. Appuyez sur `Ctrl+Shift+P` (Windows/Linux) ou `Cmd+Shift+P` (macOS)
3. Tapez "Python: Select Interpreter"
4. Sélectionnez l'interpréteur de votre venv

## 🎓 Pour approfondir

- [Documentation officielle de venv](https://docs.python.org/fr/3/library/venv.html)
- [Guide pip](https://pip.pypa.io/en/stable/)
- [Python Packaging User Guide](https://packaging.python.org/)


> #### 🌟 **Félicitations !** Vous maîtrisez maintenant les bases des environnements virtuels Python. Bon développement !
