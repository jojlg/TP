
# Qu'est ce que `Snort`

Snort est le premier système de prévention des intrusions (IPS) open source au monde. Snort IPS utilise une série de règles qui aident à définir l’activité réseau malveillante et utilisent ces règles pour trouver les paquets qui leur correspondent, et génère des alertes pour les utilisateurs.

Snort peut également être déployé en ligne pour arrêter ces paquets. Snort a trois utilisations principales : En tant que renifleur de paquets comme tcpdump, en tant qu’enregistreur de paquets ce qui est utile pour le débogage du trafic réseau, ou il peut être Utilisé comme un système complet de prévention des intrusions sur le réseau. Snort peut être téléchargé et configuré pour et à des fins professionnelles.



# Étape 1 : Installer les dépendances nécessaires 

```bash
sudo apt update
sudo apt upgrade -y
```

#installer Cmake si manquant

```bash
sudo apt install -y build-essential libpcap-dev libpcre3-dev zlib1g-dev \
libluajit-5.1-dev libtins-dev libhwloc-dev cmake pkg-config git flex bison \
libdumbnet-dev libssl-dev libnghttp2-dev libcurl4-openssl-dev \
libgoogle-perftools-dev libhs-dev libsafec-dev uuid-dev libunwind-dev \
liblzma-dev libnetfilter-queue-dev libmnl-dev autotools-dev autoconf libtool
```

# Étape 2 : Installation de la bibliothèque libdnet

* Cloner et installer libdnet :

```bash
git clone https://github.com/dugsong/libdnet.git
cd libdnet
./configure
make
sudo make install
sudo ldconfig
```

# Étape 3 : Installation de DAQ (Data Acquisition Library)
* Cloner et installer DAQ :
```bash

git clone https://github.com/snort3/libdaq.git
cd libdaq
./bootstrap
./configure
make
sudo make install
sudo ldconfig
```

# Étape 4 : Installation de Snort 3
* Cloner le dépôt Snort 3 :

```bash
cd ~
git clone https://github.com/snort3/snort3.git
cd snort3
```

## Configurer Snort 3 :

```bash
./configure_cmake.sh --prefix=/usr/local --enable-tcmalloc
```
* Si vous rencontrez des erreurs liées à TCMalloc, vous pouvez omettre --enable-tcmalloc : 

```bash
./configure_cmake.sh --prefix=/usr/local
```

## Compiler Snort 3 :

```bash
cd build
make -j$(nproc)
```
## Installer Snort 3 :

```bash
sudo make install
```

# Étape 5 : Post-installation
* Mettre à jour le cache des bibliothèques partagées :

```bash
sudo ldconfig
```

## Vérifier l'installation de Snort :

```bash
snort -V
```



# Étape 7 (Facultatif) : Configurer Snort en tant que Service
* Pour exécuter Snort en tant que service sur votre système, vous pouvez créer un fichier de service systemd.

* Créer un fichier de service :

```bash
sudo nano /etc/systemd/system/snort3.service
```
Ajoutez-y les lignes suivantes :

```ini
[Unit]
Description=Snort 3 NIDS Daemon
After=network.target

[Service]
ExecStart=/usr/local/bin/snort -c /usr/local/etc/snort/snort.lua -i eth0 -D
User=snort
Group=snort

[Install]
WantedBy=multi-user.target
```


* Création du user snort & droits
```bash
sudo groupadd snort
sudo useradd -r -s /sbin/nologin -g snort snort
sudo chown -R snort:snort /usr/local/etc/snort
sudo chown snort:snort /usr/local/bin/snort
```

* Activer et démarrer le service :

```bash
sudo systemctl daemon-reload
sudo systemctl start snort3
sudo systemctl status snort3
```

* Vérifier le service :

* Consultez les journaux pour vérifier que Snort fonctionne correctement :

```bash
sudo journalctl -u snort3
```




## Intégrer une règle personnalisée pour détecter les tentatives d'accès SSH

### Étape 1 : Inclure un fichier de règles dans snort.lua (étape à confirmer, fonctionne sans celle ci)
* Ajoute cette ligne sous la section ips

```bash
ips = {
    -- use this to enable decoder and inspector alerts
    --enable_builtin_rules = true,

    -- use include for rules files; be sure to set your path
    -- note that rules files can include other rules files
    -- (see also related path vars at the top of snort_defaults.lua)

    variables = default_variables,
    
    -- Inclure des fichiers de règles personnalisées
    include = 'local.rules'
}
```

## Étape 2 : Créer ou modifier local.rules
(si besoin mkdir /usr/local/etc/snort/rules)
```bash
sudo nano /usr/local/etc/snort/rules/local.rules
```

* Ajouter la règle SSH dans ce fichier
```bash
alert tcp any any -> any 22 (msg:"Tentative de connexion SSH détectée"; sid:1000001; rev:1; flags:S; flow:to_server,established; content:"SSH"; depth:4; classtype:attempted-admin; priority:1;)
```

* Règle pour ICMP
```bash
alert icmp any any -> any any (msg:"ICMP Ping Request"; itype:8; sid:1000001; classtype:attempted-recon; )
```


## CONFIGURATION DES LOGS

```bash
sudo mkdir /var/log/snort
sudo chmod 777 /var/log/snort
```

## Modifier le fichier nort.lua

```bash
sudo nano /usr/local/etc/snort/snort.lua
7. configure outputs

alert_fast = { file = true, limit = 1000000 }

alert_full = { file = true, limit = 1000000 }
```


```bash
sudo systemctl restart snort3
```

## Test

Lancer un ping à destination de votre machine

​
```bash
snort -c /usr/local/etc/snort/snort.lua -R /usr/local/etc/snort/rules/local.rules -i ens18 -A alert_fast -l /var/log/snort
```
​

Autres options d'alerte :

alert_fast
alert_full

## Étape 8 : Surveillance et Gestion de Snort
* Vous pouvez maintenant surveiller les alertes générées par Snort en consultant les fichiers journaux situés dans `/var/log/snort`.
