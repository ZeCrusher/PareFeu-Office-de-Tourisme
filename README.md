# PareFeu-Office-de-Tourisme
**Recyclage et installation d'un Parefeu**



## Installation et configuration d’un pare-feu avec PfSense

Schéma type d’un réseau 

![Capture d'écran 2025-01-23 100640](https://github.com/user-attachments/assets/2480a606-7c74-4a76-b124-15c6f50ebd89)
schéma 1.


Qu'est-ce qu'un pare-feu ?


Un pare-feu est un dispositif de sécurité, matériel ou logiciel, qui surveille et contrôle le trafic réseau entrant et sortant en fonction de règles de sécurité prédéfinies. Il agit comme une barrière entre un réseau interne sécurisé et des réseaux externes potentiellement dangereux, tels qu'Internet.

Pourquoi en avons-nous besoin ?

Protéger contre les menaces externes : Les pare-feu empêchent les tentatives d'accès non autorisées à vos systèmes en bloquant les connexions suspectes ou malveillantes.


Filtrer le contenu : Ils permettent de restreindre l'accès à des sites ou services non sécurisés, limitant ainsi l’exposition aux menaces comme les logiciels malveillants.


Empêcher les attaques ciblées : Les pare-feu peuvent détecter et bloquer les activités suspectes, comme les attaques par déni de service (DDoS) ou les intrusions.


Protéger les données sensibles : En empêchant les cybercriminels d’accéder aux réseaux internes, ils réduisent le risque de fuite ou de vol de données.

















Prérequis
Matériel nécessaire :
Un PC dédié pour pfSense (avec un processeur 64 bits, au moins 4 Go de mémoire et un SSD recommandé). Nous pouvons facilement utiliser les anciens ordinateurs LENOVO. Ils sont largement suffisants pour héberger l’OS de PfSense. Et surtout déjà disponibles et interchangeables. 


Une carte réseau avec deux ports RJ45 - 10 Gbits secondes (Exemple de carte réseau Intel est un bon choix pour la compatibilité dans pfsense) qui sera ajouter au  PC Lenovo.  
A gauche, une carte réseau à ajouter à un PC Lenovo de l’Office ou autre possibilité, un mini PC dédié compatible avec Pfsense et OPNsense au niveau matériel (carte réseau reconnue par FreeBSD). 



Une clé USB (au moins 4 Go) pour pouvoir installer le système sur le PC.
Un autre PC pour créer la clé bootable.
Deux câbles réseau Rj45 pour faire le pont entre la box Orange de l’Office et le switch réseau rj45 POE de l’Office (voir schéma 1).

Les logiciels nécessaires (tous gratuit):
L’image ISO de pfSense, disponible sur le site officiel :
 https://www.pfsense.org/download/




Le logiciel Rufus pour créer une clé bootable :  https://rufus.ie/ 
Note : Rufus est un utilitaire windows permettant de formater et de créer des cléfs  USB démarrable. Nous allons l’utiliser pour notre test avec l’image ISO de pfsense.



Étape 1 : Préparation de la clé USB bootable.
Téléchargement de l’ISO de pfSense :

Note : Un fichier .iso est une image disque, c'est-à-dire une copie exacte d'un support physique comme un CD, un DVD ou un disque dur. Il contient tous les fichiers, dossiers et informations nécessaires pour reproduire le contenu original du support. Ici, c’est le système d’exploitation avec pfSense. Il s’agit de FreeBSD.

Et jusqu'en mai 2024, il était possible de télécharger une image d'installation de pfSense que l'on pouvait ensuite installer hors-ligne en utilisant un cd ou une clef usb avec RUFUS par exemple. Cette méthode d'installation simple et efficace n'est officiellement plus proposée par Netgate, l'éditeur du logiciel pfSense.

Il faut dorénavant passer par un logiciel tiers appelé "Netgate installer" permettant d'installer pfSense CE (la version communautaire sur base open-source) ou pfSense+ (la version propriétaire payante). Dans notre système de sécurité, nous allons utiliser pfSense CE. Il existe également un fork (reprise logicielle) appelé Opnsense. Il en reprend les bases de pfSense mais en change l’interface et quelques options. Mais le cœur du projet est resté similaire.


Dans les téléchargements disponibles, il faut sélectionner la version 64 bits pour AMD64 (compatible Intel et AMD).

- Le téléchargement peut se faire sur le github : https://github.com/CloudSentralDotNet/iso_pfsense 


La dernière version disponible en janvier 2025 est la 2.7.2 de décembre 2023. Il s'agit ici du noyau du système, tout comme Windows 11 qui est sorti le 5 octobre 2021. Mais les packages sont mis à jour (voir Mise à jour de PfSense, Package - Etape 7). 


Si disponible, choisir une image de type VGA (adaptée pour un PC standard) si possible (avec une installation à l’écran).
Télécharger l’ISO correspondant.



Télécharge et lance Rufus sur ton PC (version 4.6.2208 pour ce test). 
https://rufus.ie/fr/ 


Utilisation de Rufus pour créer une clé USB bootable avec pfSense :


Il faut insérer la clé USB dans le PC.
Dans Rufus, nous devons sélectionner la clé USB dans "Périphérique".
Ensuite, il faut cliquer sur "Sélectionner" pour choisir l’image ISO de pfSense que nous avons téléchargée au préalable.




Garde les options par défaut :
Schéma de partition : MBR.
Système de fichiers : FAT32.
Cliquez sur Démarrer pour créer la clé bootable. Cela est peut être long en fonction de la vitesse de la clef USB (6 à 10 minutes parfois).

Étape 2 : Installer pfSense sur le PC
Insèrez la clé USB dans le PC dédié à pfSense. Dans notre exemple, nous utiliserons le PC Lenovo avec une carte réseau Intel dédié. 


Accédez au BIOS du PC (souvent en appuyant sur bouton del/suppr selon la carte mère) et configure l’ordre de démarrage. Démarrage sur USB.


Mettre la clé USB en premier.


Démarre le PC :


pfSense démarre automatiquement depuis la clé USB. Si ce n’est pas le cas, il faut appuyer sur la touche F12 sur avoir le menu de Boot. 


Une interface texte s’affiche avec un menu d’installation.


Lance l’installation :


Appuie sur Entrée pour démarrer l’installation.
Choisis la disposition du clavier (par défaut, US ; pour un clavier français, sélectionne French).



Après quelques menus, il faut sélectionner dans l’installation de PfSense Stripe 


Note : Malheureusement Stripe ne fait pas de redondance de disque, comme pourrait le faire le Raid 1 ou le mirror. Il est important d’avoir 2 disques, l’un étant la copie de l’autre. Si un des deux disques tombent en panne. Le système fonctionnerait encore. Il suffit juste de changer le disque hors service. 

Étape 3 : Configurer le SSD
Choisis le disque d’installation :


Sélectionne le SSD sur lequel tu veux installer pfSense.
Autorise l’effacement complet si demandé.
Système de fichiers :


Garde les options par défaut (UFS recommandé pour une installation standard).
L’installation démarre et copie les fichiers nécessaires sur le SSD. Une fois terminé, tu seras invité à retirer la clé USB.


Retire la clé USB et redémarre le PC.



Étape 4 : Configuration initiale de pfSense
Après le redémarrage, pfSense démarre depuis le SSD.


Associe les interfaces réseau :


pfSense te demande de configurer les interfaces :
WAN : Le port connecté à ton modem/routeur.
LAN : Le port connecté à ton réseau interne.
Si ta carte réseau a deux ports RJ45, pfSense devrait les détecter automatiquement. Suis les instructions pour attribuer chaque port.
Vérifie que tout fonctionne correctement :


Une fois les interfaces configurées, tu verras les adresses IP par défaut attribuées au LAN (192.168.1.1 en général) et au WAN.

Étape 5 : Accéder à l’interface web de pfSense
Connecte un PC ou un appareil au port LAN.


Ouvre un navigateur web et accède à l’interface via l’adresse suivante :
 http://192.168.1.1


Identifie-toi :


Nom d’utilisateur par défaut : admin.
Mot de passe par défaut : pfsense.
Suis l’assistant de configuration pour finaliser les réglages :


Configure l’accès Internet (WAN).
Ajuste les paramètres du LAN selon tes besoins.
Change les identifiants par défaut pour renforcer la sécurité.

Étape 6 : Finaliser la configuration
Installe les paquets nécessaires pour ajouter des fonctionnalités (par exemple, Squid pour le proxy, Snort pour la détection d’intrusions).
Configure les règles de pare-feu pour sécuriser ton réseau.

Étape 7 : Mise à jour du système pfSense 
Connectez-vous à l'interface web de pfSense avec l’adresse 192.168.1.1.
Allez dans le menu System > Update.
L'onglet System Update vous indiquera si une mise à jour du système est disponible. Si c'est le cas, suivez les instructions pour effectuer la mise à jour.
 Documentation Netgate
Mise à jour des packages (modules) :
Dans l'interface web, naviguez vers System > Package Manager.
Sous l'onglet Installed Packages, vous verrez la liste des packages installés sur votre système.
Si des mises à jour sont disponibles pour certains packages, une icône de mise à jour apparaîtra à côté du nom du package. Cliquez sur cette icône pour mettre à jour le package concerné. 


Et voilà ! 🎉 Ton firewall pfSense est maintenant opérationnel et prêt à sécuriser ton réseau. Si tu veux ajouter des détails ou des configurations spécifiques, fais-le-moi savoir ! 😊



Filtrer des éléments spécifiques à l'intérieur des sites web n'est pas la fonction d'un pare-feu. C'est plutôt le travail d'un plugin/script de navigateur. Je veux dire, comment un pare-feu saurait-il rechercher des mots clés dans un site web ?

Vous pouvez bloquer des sites web entiers en utilisant quelque chose comme pfBlockerNG basé sur les domaines et leur type de page en utilisant DNSBL, mais un pare-feu n'est pas un rendu de site web. Sans une sorte d'agent logiciel sur votre PC, il n'aurait aucune idée de ce que vous regardez.

Non, vous ne pouvez pas. Le trafic Web est (presque) toujours crypté et non lisible par votre pare-feu. Pour ce faire, vous devrez décrypter chaque paquet, analyser les données, décider quoi faire, le crypter et l'envoyer. Ce n'est pas faisable avec PfSense.

Que voulez-vous accomplir exactement ? Bloquer un réseau professionnel ? Vous ne voulez pas voir de publicités ?

Une façon de réduire les publicités serait un piHole avec une liste noire personnalisée.


1.1.1.3
