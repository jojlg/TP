# Procédure pour lier, cloner et ouvrir un projet GitHub dans VS Code via WSL 

## Installer WSL

Dans un Powershell (en admin) `wsl --install`
## Installer Git
```bash
sudo apt-get update && sudo apt-get install git Installer VS Code
```

## Générez une nouvelle clé SSH :

`ssh-keygen -t ed25519`

Démarrez l'agent SSH : `eval "$(ssh-agent -s)"`

Ajoutez votre clé privée à l'agent SSH :
`ssh-add ~/.ssh/id_git`

Copiez la clé SSH dans le presse-papiers :
`cat ~/.ssh/id_git.pub`

* Accédez à GitHub, connectez-vous à votre compte.
* Allez dans Settings > SSH and GPG keys.
* Cliquez sur New SSH key, donnez-lui un titre et collez la clé publique dans le champ "Key".
* Cliquez sur Add SSH key.



## Téléchargez et installez VS Code

https://code.visualstudio.com/

## Installer l'extension WSL pour VS Code
* Dans VS Code, allez dans l'onglet Extensions (Ctrl+Shift+X) et recherchez `Remote - WSL` puis installez-la.



* Pour cet exemple, nous utiliserons le projet "Awesome Python" disponible à l'adresse suivante :
https://github.com/vinta/awesome-python.

## Lier votre projet GitHub
## Configurer Git
* Dans le terminal WSL, exécutez :
```bash
git config --global user.name >VotreNomGit< 
```
```bash
git config --global user.email "VotreEmail@example.com" 
```

## Cloner le projet
* Pour cloner le projet, ouvrez votre terminal et exécutez la commande git clone suivie de l'URL du projet GitHub. Cela téléchargera une copie locale du projet sur votre ordinateur.

```bash
git clone https://github.com/vinta/awesome-python.git
```


## Accéder au répertoire du projet cloné
* Après le clonage, vous devez naviguer dans le répertoire du projet pour commencer à travailler dessus. Utilisez la commande cd pour entrer dans le répertoire cloné.

```bash
cd awesome-python
```


## Créer une nouvelle branche pour vos modifications
* Il est conseillé de créer une nouvelle branche avant de faire des modifications pour ne pas altérer la branche principale (main ou master). Cela vous permet de travailler sur une copie indépendante de la branche principale.

```bash
git checkout -b my-awesome-python
```

## Accédez au répertoire du projet cloné :

```bash
cd votre-repo
```
```bash
code .
```


## Ajouter les fichiers modifiés et committer les changements
* Une fois les modifications terminées, vous devez les ajouter à l'index de Git avant de les committer. Utilisez git add pour ajouter les fichiers modifiés, puis git commit pour enregistrer vos changements avec un message de commit descriptif.

```bash
git add .
git commit -m "Ajout de nouvelles ressources à Awesome Python"
```

## Publier vos modifications sur GitHub
* Si vous souhaitez publier vos modifications sur votre propre dépôt GitHub, commencez par créer un nouveau dépôt sur GitHub. Notez l'URL de votre nouveau dépôt.

* Ensuite, ajoutez ce dépôt en tant que remote (origin) et poussez vos modifications vers votre dépôt GitHub. Remplacez votre-utilisateur par votre nom d'utilisateur GitHub.

```bash
git remote add origin https://github.com/votre-utilisateur/awesome-python.git
git push -u origin my-awesome-python
```

Pousser les modifications vers GitHub (branche main): `git push origin main`





## Configurer le dépôt Git pour utiliser SSH
* Changez l'URL du dépôt pour utiliser SSH :

`git remote set-url origin git@github.com:<your-username>/<your-repo>.git`


# Récapitulatif
* Choisir le projet : Identifier et noter l'URL du projet à cloner.
* Cloner le projet : Utiliser git clone <URL> pour copier le projet localement.
* Accéder au répertoire : Naviguer dans le répertoire cloné avec cd.
* Créer une branche : Créer une nouvelle branche pour les modifications avec git checkout -b <branch-name>.
* Faire les modifications : Modifier le code ou les fichiers selon vos besoins.
* Ajouter et committer : Ajouter (git add .) et committer (git commit -m <message>) les changements.
* Publier sur GitHub : Ajouter le dépôt distant et pousser les changements avec git remote add origin <URL> et git push -u origin <branch-name>.
* Créer une Pull Request : Optionnellement, proposer vos modifications au dépôt original via une Pull Request.

