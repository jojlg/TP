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


## Configurer le dépôt Git pour utiliser SSH
Ouvrez le terminal intégré de VS Code.
Changez l'URL du dépôt pour utiliser SSH :

`git remote set-url origin git@github.com:<your-username>/<your-repo>.git`



## Téléchargez et installez VS Code

https://code.visualstudio.com/

## Installer l'extension WSL pour VS Code
* Dans VS Code, allez dans l'onglet Extensions (Ctrl+Shift+X) et recherchez `Remote - WSL` puis installez-la.

## Lier votre projet GitHub
## Configurer Git
* Dans le terminal WSL, exécutez :
```bash
git config --global user.name >VotreNomGit< 
```
```bash
git config --global user.email "VotreEmail@example.com" 
```
## Cloner votre projet GitHub git clone 
* HTTPS ou SSH :

Exemple : 

`git clone https://github.com/votre-nom-dutilisateur/votre-repo.git`

* Accédez au répertoire du projet cloné :

```bash
cd votre-repo
```
```bash
code .
```

## Créer une nouvelle branche pour vos modifications
* Il est recommandé de créer une nouvelle branche pour vos modifications :

```bash
git checkout -b nom-de-la--nouvelle-branche
```

## Utilisation quotidienne

> ⚠️ Après avoir fait vos modifications, vous devez ajouter les fichiers modifiés et committer vos changements :

Ajouter des fichiers : `git add .`

Valider des modifications : `git commit -m "Votre message de commit"`

Si vous souhaitez publier vos modifications sur votre propre dépôt GitHub, vous devrez d'abord créer un nouveau dépôt sur GitHub. Ensuite, ajoutez ce dépôt en tant que remote et poussez vos modifications :

```bash
git remote add origin https://github.com/votre-utilisateur/projet.git
git push -u origin nouvelle-branche-projet
```

Pousser les modifications vers GitHub : `git push origin main`


## Créer une Pull Request
Si vous pensez que vos modifications peuvent être utiles à la communauté, vous pouvez créer une Pull Request vers le dépôt original.
