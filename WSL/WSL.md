# Procédure pour lier, cloner et ouvrir un projet GitHub dans VS Code via WSL Installer WSL

Dans un Powershell (en admin) `wsl --install`
## Installer Git
```bash
sudo apt-get update && sudo apt-get install git Installer VS Code
```

## Téléchargez et installez VS Code

https://code.visualstudio.com/

## Installer l'extension WSL pour VS Code
* Dans VS Code, allez dans l'onglet Extensions (Ctrl+Shift+X) et recherchez `Remote - WSL` puis installez-la.

## Lier votre projet GitHub
## Configurer Git
* Dans le terminal WSL, exécutez :
```bash
git config --global user.name "VotreNom" 
```
```bash
git config --global user.email "VotreEmail@example.com" 
```
## Cloner votre projet GitHub git clone 
https://github.com/votre-nom-dutilisateur/votre-repo.git

* Remplacez votre-nom-dutilisateur par votre nom d'utilisateur GitHub et votre-repo par le nom du dépôt.


## Ouvrir VS Code depuis WSL
* Naviguer dans le répertoire du projet cloné

`cd votre-repo` 

`code . `

## Utilisation quotidienne

Ajouter des fichiers : `git add .`

Valider des modifications : `git commit -m "Votre message de commit"`

Pousser les modifications vers GitHub : `git push origin main`
