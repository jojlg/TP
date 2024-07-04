# Dockerfile
Deploiement d'un conteneur Ubuntu avec l'application Vulnerable light app récupérer sur Github



```yaml
#Utilise l'image de base Ubuntu 22.04
FROM ubuntu:22.04

#Installe les dépendances nécessaires
RUN apt-get update && \
    apt-get install -y wget apt-transport-https && \
    wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb && \
    dpkg -i packages-microsoft-prod.deb && \
    apt-get update && \
    apt-get install -y dotnet-sdk-8.0 git && \
    apt-get clean

#Clone le dépôt GitHub
RUN git clone https://github.com/Aif4thah/VulnerableLightApp.git

#Défini le répertoire de travail
WORKDIR /VulnerableLightApp

#Construire l'application
RUN dotnet build

#Commande par défaut
CMD ["dotnet", "run"]
```


## Construire l'image
```bash
docker build -t vulnerablelightapp .
```

## Créer le conteneur
```bash
docker run -d -p 80:4000 --name myvulnerableapp vulnerablelightapp
```


Installation réussie

![vulnapp](https://github.com/jojlg/DOCKER/assets/135955870/18892e99-6b61-46f9-a464-c1568c7a3992)


## Modification du code
Repérer le chemin complet du merged afin de pouvoir modifier par la suite l'IP URL de l'app

```bash
Docker inspect xxxxxxxx
```

```bash
"GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/828ebfe30db61c05e4da5f47477233dde4bcc38fba5ee630f35229bd58d29d12-init/diff:/var/lib/docker/overlay2/7khpgd630usg0g8iaryhmukbc/diff:/var/lib/docker/overlay2/nx1l93act6u327v5usfiwz3kc/diff:/var/lib/docker/overlay2/r2phk3r9vwtxjln2ayggoqlj7/diff:/var/lib/docker/overlay2/5jpcd9eim4lpbqxzqm29h1r2w/diff:/var/lib/docker/overlay2/1u73uhbshadvbdy3zkhf9dw6v/diff:/var/lib/docker/overlay2/71d067723209e27e9cdafe8e76b352249bdcd12ce77918f416f0b7e688cf3c65/diff",
                "MergedDir": "/var/lib/docker/overlay2/828ebfe30db61c05e4da5f47477233dde4bcc38fba5ee630f35229bd58d29d12/merged",
                "UpperDir": "/var/lib/docker/overlay2/828ebfe30db61c05e4da5f47477233dde4bcc38fba5ee630f35229bd58d29d12/diff",
                "WorkDir": "/var/lib/docker/overlay2/828ebfe30db61c05e4da5f47477233dde4bcc38fba5ee630f35229bd58d29d12/work"
```

Le chemin est `/var/lib/docker/overlay2/828ebfe30db61c05e4da5f47477233dde4bcc38fba5ee630f35229bd58d29d12/merged` 
j'ajout `/VulnerableLightApp/Program.cs`

```bash
sudo nano /var/lib/docker/overlay2/828ebfe30db61c05e4da5f47477233dde4bcc38fba5ee630f35229bd58d29d12/merged/VulnerableLightApp/Program.cs
```
### Modifier l'ip "appUrls.Add("http://0.0.0.0:4000"); app.Urls.Add("https://0.0.0.0:3000"); par l'IP du conteneur

```bash
// Arguments :

string url = args.FirstOrDefault(arg => arg.StartsWith("--url="));
string test = args.FirstOrDefault(arg => arg.StartsWith("--test"));

if(!string.IsNullOrEmpty(test))
{
    Console.WriteLine("Start CPU Testing");
    TestCpu.TestAffinity();
}

if (string.IsNullOrEmpty(url))
{
    app.Urls.Add("http://172.17.0.2:4000");
    app.Urls.Add("https://172.17.0.2:3000");
}
```
### L'appli est joignable 
![Vulnerablelightapp](https://github.com/jojlg/DOCKER/assets/135955870/aa04a237-e19e-4466-ba9e-8cae99c6b5bf)


## KANBAN
![kanban](https://github.com/jojlg/DOCKER/assets/135955870/163c6c13-fe78-492a-9424-a1513301865f)