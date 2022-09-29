# TP4 Gestion des paquets
## _Exercice 1_  

1 - Il faut faire un apt update puis apt upgrade.  
2 - Il faut faire un alias [alias maj (nom de l'alias)='apt update ; apt upgrade' (commandes)], pour qu'il soit permanent il faut l'ajouter directement dans le .bashrc pour qu'il soit recréé à chaque démarrage.   
3 -   
- status installed python3-gdbm:amd64 3.10.6-1~22.04  
- status installed man-db:amd64 2.10.2-1  
- status installed install-info:amd64 6.8-4build1  
- status installed libc-bin:amd64 2.35-0ubuntu3.1  
- status installed python3-distutils:all 3.10.6-1~22.04  

4 - les informations se trouvent dans /var/log/apt/history.log  
- linux-headers-5.15.0-48  
- linux-modules-extra-5.15.0-48-generic  
- linux-modules-5.15.0-48-generic  
- linux-headers-5.15.0-48-generic  
- linux-image-5.15.0-33-generic  

5 - Pour les packet dpkg il faut faire un dkpg --list (662 packets) / pour apt list --installed | wc -l (615 packets). La différence s'explique par le fait que dkgp ne gère pas les dépendances et n'est donc pas bloqué  comme apt par certaines dépendances.  

6 - Il faut faire apt list | wc -l (68 953)  

7 - Il faut faire un apt install (nom du packet)   
- glances : C'est un outils de monitoring   
- Hollywood est un packet inutile  
- tldr permet de simplifier le manuel   

8 - Le packet permettant d'installer un sudoku est sudoku   

## _Exercice 2_  

La commande ls vient du package GNU Core Utilities. En faisant (which -a ls) | xargs dpkg -s.   
La commande est la suivante :  

#!/bin/bash  

(which -a $1) | xargs dpkg -S 2>dev/null  

## _Exercice 3_   

## _Exercie 4_   

Il faut faire un [dpkg -L coreutils | xargs which], le programme '[' represente une commande test pour faire des expressions conditionnelles.  

## _Exercice 5_
Emacs est un éditeur de texte et Lynx est un navigateur web executable depuis une console ou un terminal.  

## _Exercice 6_  
1-  
Après avoir executé :   
- sudo add-apt-repository ppa:linuxuprising/java
- sudo apt update
- sudo apt install oracle-java15-installer

On m'affiche un message d'erreur :

```
User@localhost:/home$ sudo apt install oracle-java15-installer
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
E: Unable to locate package oracle-java15-installer 
```
C'est normal puisque java15-installer n'est pas dans les packages d'ubuntu.  

2 -  
Un nouveau fichier 'linuxuprising-ubuntu-java-jammy.list' a été créé et il contient le lien vers le packet de java15.  

## _Exercice 7_ 
1- 
````
git clone https://gitlab.com/jallbrit/cbonsai
````
2- Il faut installer un ncursesw qui fonction via 
````
sudo apt install libncursesw5-dev
````
On peut ensuite l'installer via un simple apt install cbansai

3- Il faut d'abord installer checkinstall 
````
User@localhost:/home/cbonsai$ sudo apt install checkinstall
````  
4- Il faut faire un :
````
sudo checkinstall 
````
Puis bien configurer l'installation en vérifiant bien que cbonsai qu'on installe 
````
This package will be built according to these values:

0 -  Maintainer: [ root@localhost ]
1 -  Summary: [ Package created with checkinstall 1.6.3 ]
2 -  Name:    [ cbonsai ]
3 -  Version: [ 20220929 ]
4 -  Release: [ 1 ]
5 -  License: [ GPL ]
6 -  Group:   [ checkinstall ]
7 -  Architecture: [ amd64 ]
8 -  Source location: [ cbonsai ]
9 -  Alternate source location: [  ]
10 - Requires: [  ]
11 - Recommends: [  ]
12 - Suggests: [  ]
13 - Provides: [ cbonsai ]
14 - Conflicts: [  ]
15 - Replaces: [  ]
````  

## _Exercice 8_

_Création d’un paquet Debian avec dpkg-deb_
1-  Création des dossiers : 
````
User@localhost:/home/script$ sudo mkdir origine-commande
User@localhost:/home/script$ cd origine-commande
User@localhost:/home/script/origine-commande$ sudo mkdir DEBIAN
````
Ajout de DEBIAN dans le $PATH : 
````
User@localhost:/home/script/origine-commande$ PATH=$PATH:/home/script/origine-commande/DEBIAN
User@localhost:/home/script/origine-commande$ $PATH
-bash: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/script/origine-commande/DEBIAN: No such file or directory
````

2-  
```
User@localhost:/home/script/origine-commande/DEBIAN$ sudo nano control
Package: origine-commande
Version: 0.1
Maintainer: Foo Bar
Architecture: all
Description: Cherche l'origine d'une commande
Section: utils
Priority: optional
```

3- 
```
User@localhost:/home/script$ sudo dpkg-deb --build origine-commande
dpkg-deb: building package 'origine-commande' in 'origine-commande.deb'.
```

_Création du dépôt personnel avec reprepro_

1- 
```
User@localhost:/home/script$ cd ~
User@localhost:~$ mkdir repo-cpe
User@localhost:~$ ls
clean.sh  repo-cep  script  typescript
```
2- 
```
User@localhost:~/repo-cpe$ mkdir conf
User@localhost:~/repo-cpe$ mkdir packages
User@localhost:~/repo-cpe$ ls
conf  packages
```
3- 
````
User@localhost:~/repo-cpe$ cd conf
User@localhost:~/repo-cpe/conf$ sudo nano distributions
Origin: Un nom, une URL, ou tout texte expliquant la provenance du dépôt
Label: Nom du dépôt
// Suite: stable
Codename: focal #!! A MODIFIER selon la distribution cible !!
Architectures: i386 amd64 #(architectures cibles)
Components: universe #(correspond à notre cas)
Description: Une description du dépôt
````
4- A l'execution de la commande "reprepro -b . export" j'ai le message d'erreur suivant : 
```
User@localhost:~/repo-cpe$ reprepro -b . export
Error parsing ./conf/distributions, line 3: Unexpected space in header name!
There have been errors!
```
Il suffisait de supprimer les commantaires du fichier distributions 

5- Pour copier le packet origine-commande.deb créé précédemment dans le dossier packages du dépôt, puis,
à la racine du dépôt il faut faire le commande donnée dans me TP 
```
User@localhost:~/script$ cp *.deb ~/repo-cpe/packages
```
Une fois qu'il est bien dans le dossier package :
```
reprepro -b . includedeb cosmic packages/origine-commande.deb
```
Seulement, un nouveau message d'erreur apparait, j'ai oublié de modifier le code name du fichier distributions pour qu'il puisse fonctionner avec la commande (j'aurais aussi pu saisir le code name qui était de base dans le fichier distributions ça aurait aussi fonctionné)
```
Origin: Un nom, une URL, ou tout texte expliquant la provenance du dépôt
Label: CPE repo
Suite: stable
Codename: cosmic
Architectures: i386 amd64
Components: universe
Description: Une description du dépôt
```

6-  Je me rends dans le dossier "/etc/apt/sources.list.d"
Je fais ensuite le fichier "repo-cpe.list" et j'y met le chemin vers le dossier repo-cpe là où il y a mon packet à installer.
```
User@localhost:/etc/apt/sources.list.d$ sudo nano repo-cpe.list
deb file:/home/User/repo-cpe cosmic universe
```
7- J'execute un "sudo apt update" pour m'assurer qu'il n'y ait pas d'erreur 
`````
User@localhost:/etc/apt/sources.list.d$ sudo apt update
Get:1 file:/home/User/repo-cpe cosmic InRelease
Ign:1 file:/home/User/repo-cpe cosmic InRelease
Get:2 file:/home/User/repo-cpe cosmic Release [1,752 B]
Get:2 file:/home/User/repo-cpe cosmic Release [1,752 B]
Get:3 file:/home/User/repo-cpe cosmic Release.gpg
Ign:3 file:/home/User/repo-cpe cosmic Release.gpg
Hit:4 https://ppa.launchpadcontent.net/linuxuprising/java/ubuntu jammy InRelease
Hit:5 http://us.archive.ubuntu.com/ubuntu jammy InRelease
Hit:6 http://us.archive.ubuntu.com/ubuntu jammy-updates InRelease
Hit:7 http://us.archive.ubuntu.com/ubuntu jammy-backports InRelease
Hit:8 http://us.archive.ubuntu.com/ubuntu jammy-security InRelease
Reading package lists... Done
````
