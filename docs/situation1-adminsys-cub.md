# Situation 1 – Administration système CUB
---
title: "[]{#_8ger0jugicg1 .anchor}Préparation de la maquette et premiers
  paramétrages du serveur
  Windows2019![](media/image1.png){width=\"3.123611111111111in\"
  height=\"1.3284722222222223in\"}"
---

> **Présenté par Joris Texier**

**Date de rédaction : 03 septembre 2025**

**Version 1**

### 

###  

SOMMAIRE

[[Réaliser un Sysprep pour réinitialiser le SID :
3]{.underline}](#réaliser-un-sysprep-pour-réinitialiser-le-sid)

[[Changer le nom du serveur :
4]{.underline}](#changer-le-nom-du-serveur)

[[Modifier le VLAN et l\'adresse IP :
5]{.underline}](#modifier-le-vlan-et-ladresse-ip)

[[Installation et configuration du service SSH
5]{.underline}](#installation-et-configuration-du-service-ssh)

[[Configurer OpenSSH Server sur Windows
7]{.underline}](#configurer-openssh-server-sur-windows)

[[Configuration d\'OpenSSH Server sur Windows
8]{.underline}](#configuration-dopenssh-server-sur-windows)

[[Utiliser le client OpenSSH sur Windows
10]{.underline}](#utiliser-le-client-openssh-sur-windows)

##  

## Réaliser un Sysprep pour réinitialiser le SID :

> Le sysprep est nécessaire pour réinitialiser le SID de Windows et
> permettre une configuration propre de la machine clonée.
>
> Cet utilitaire se trouve dans **C:\\Windows\\System32\\Sysprep.\
> **![](media/image18.png){width="6.135416666666667in"
> height="4.09375in"}
>
> **Sysprep s'utilise en mode graphique ou en ligne de commandes.**
>
> ![](media/image12.png){width="4.531496062992126in"
> height="2.9468383639545057in"}

Quand tout de passe bien, il vous affiche cette petite fenêtre qui reste
ouverte plusieurs minutes :

![](media/image9.png){width="2.9305555555555554in"
height="1.5277777777777777in"}

En revanche, lorsque l'utilitaire rencontre un problème, il vous
affichera l'un des **messages** ci-dessous *(soit directement à
l'exécution de Sysprep soit après quelques secondes)* :

![](media/image21.png){width="6.267716535433071in"
height="3.0416666666666665in"}

### 

## Changer le nom du serveur :

Redémarrez la machine virtuelle après le sysprep.

Ouvrir le gestionnaire de Serveur \> Serveur Local \> Nom de
l'ordinateur \> ServeurPrimaire13

### 

###  

## Modifier le VLAN et l\'adresse IP :

![](media/image10.png){width="4.104166666666667in"
height="4.677083333333333in"}

## Installation et configuration du service SSH 

L\'installation d\'OpenSSH Server sur Windows 10 ou Windows Server 2019
peut s\'effectuer de deux façons : à partir de PowerShell ou de
l\'interface graphique. Quoi qu\'il en soit, OpenSSH Server correspond à
une fonctionnalité facultative de Windows.

À partir de l\'interface graphique suivez les étapes suivantes. Cliquez
sur le menu Démarrer, sur Paramètres puis sur la section
\"Applications\".

![](media/image11.png){width="6.25in" height="2.9791666666666665in"}

Sur la gauche, choisissez \"Applications et fonctionnalités\" et à
droite cliquez sur \"Fonctionnalités facultatives\".

![](media/image14.png){width="6.267716535433071in" height="3.125in"}

sur le bouton \"Ajouter une fonctionnalité\".\
Utilisez la barre de recherche pour saisir le mot clé \"ssh\" et trouver
plus facilement la fonctionnalité \"**Serveur OpenSSH**\". Cochez la
case associée et cliquez sur le bouton dans le bas de la fenêtre pour
démarrer l\'installation.

![](media/image20.png){width="6.267716535433071in"
height="2.0833333333333335in"}

Patientez pendant l\'installation\... Elle ne prendra que quelques
secondes.

![](media/image17.jpg){width="5.013888888888889in"
height="1.0972222222222223in"}

L\'installation d\'OpenSSH Server ne nécessite pas de redémarrer votre
machine. Maintenant, passons à la configuration et la connexion sur un
hôte équipé d\'OpenSSH Server.

## Configurer OpenSSH Server sur Windows

Pour commencer la configuration, nous allons démarrer le serveur OpenSSH
d\'une part, et d\'autre part nous allons le configurer en démarrage
automatique.

Plutôt que de le faire à partir de la console \"Services\", je vous
propose d\'exécuter deux commandes PowerShell, ce sera plus efficace.
Voici l\'état actuel du service \"OpenSSH SSH Server\" :

![](media/image15.png){width="6.267716535433071in" height="1.125in"}

Pour démarrer le service \"sshd\" correspondant à \"OpenSSH SSH
Server\", on va utiliser la commande suivante : Start-Service -Name
\"sshd\"

Ensuite, pour le modifier et définir le mode de démarrage sur
\"Automatique\" au lieu de \"Manuel\" : Set-Service -Name \"sshd\"
-StartupType Automatic

La commande ci-dessous vous permettra de vérifier qu\'il est bien en
cours d\'exécution.

Get-Service -Name \"sshd\"

![](media/image22.png){width="6.267716535433071in"
height="3.111111111111111in"}

## Configuration d\'OpenSSH Server sur Windows

Sur Windows, la configuration d\'OpenSSH est stockée à l\'emplacement
ci-dessous où l\'on va trouver le fichier sshd_config :
%programdata%\\ssh\\

Modifier le port d'écoute SSH :

Pour modifier le port d\'écoute par défaut (recommandé) et utiliser un
autre port que le n°22, il faut modifier l\'option \"Port\". Pour
modifier le fichier de configuration, vous devez ouvrir l\'éditeur de
texte en tant qu\'administrateur pour avoir le droit d\'enregistrer le
fichier.

Ensuite, il faut décommenter la ligne \"#Port 22\" et changer le numéro
de port, comme ceci :

![](media/image8.png){width="6.267716535433071in"
height="4.333333333333333in"}

Après chaque modification de la config, il est indispensable de
redémarrer le service SSH pour charger les nouveaux paramètres.
PowerShell permet de le faire facilement :\
Restart-Service \"sshd"

A la suite de la modification du port, si la connexion SSH ne fonctionne
pas, regardez du côté du pare-feu Windows. En effet, lors de
l\'installation d\'OpenSSH Server, une règle est créée pour autoriser
les connexions sur le port 22.

Voici la commande pour créer une règle de pare-feu qui autorise les
connexions entrantes sur le port 222 :

New-NetFirewallRule -Name sshd -DisplayName \'OpenSSH Server (sshd) -
Port 222\' -Enabled True -Direction Inbound -Protocol TCP -Action Allow
-LocalPort 222

![](media/image4.png){width="6.267716535433071in"
height="2.3055555555555554in"}

![](media/image7.png){width="6.267716535433071in"
height="4.513888888888889in"}

## Utiliser le client OpenSSH sur Windows

Pour établir une connexion SSH avec ce client, c\'est très simple :

ssh \<utilisateur\>@\<hote\>

ssh administrateur@172.16.56.1 -p 222

Lorsque la connexion SSH est établie, on peut bien sûr exécuter des
commandes PowerShell. Cela nécessite d\'exécuter la commande
\"powershell.exe\" pour lancer le shell et après c\'est parti !

![](media/image2.png){width="5.5in" height="2.7708333333333335in"}

Comment activer (et configurer) le Bureau à distance ?

Par défaut, l\'accès Bureau à distance est désactivé sur Windows et
Windows Server. De ce fait, si l\'on veut se connecter à distance sur
une machine, il convient d\'activer le Bureau à distance au préalable.
Cela s\'effectue manuellement sur chaque machine, ou par GPO

Activer le Bureau à distance sous Windows Server :\
Sur Windows Server, l\'accès \"Bureau à distance\" que l\'on appelle
couramment l\'accès RDP s\'active à partir du \"Gestionnaire de
serveur\". Dans la section \"Serveur local\", il y a un paramètre nommé
\"Bureau à distance\" qui indique l\'état du service. Lorsque c\'est
désactivé (valeur par défaut), il n\'est pas possible de se connecter en
RDP sur la machine, mais cette même machine peut se connecter sur une
autre machine où le Bureau à distance est actif.

![](media/image6.png){width="5.751968503937008in"
height="4.213882327209099in"}

En cliquant sur \"Activé\" ou \"Désactivé\", on accède directement à
l\'interface permettant de changer l\'état du Bureau à distance. Pour
activer les connexions, la valeur \"Autoriser les connexions à distance
à cet ordinateur\" doit être sélectionnée.

![](media/image13.png){width="2.326771653543307in"
height="2.7836089238845143in"}

Activer le Bureau à distance sous Windows Server

Aller dans paramètre \> Système \> Bureau à distance \> activer bureau à
distance

Ensuite Ouvrir le logiciel "Connexion Bureau à distance"

![](media/image5.png){width="4.1875in" height="2.6458333333333335in"}

Création nouvel utilisateur sous Windows 2019

nom : adminssh; mot de passe : Cub_Admin_Ssh_007

Interdiction Connexion SSH compte administrateur

Il faut utiliser la directive DenyUsers pour refuser l\'accès à certains
groupes, notre cible sera le groupe \"Administrateurs\" pour empêcher
les comptes admins locaux de se connecter en SSH.

Dans le fichier de configuration, il faut commenter les deux lignes
suivantes, sinon la directive DenyUsers ne fonctionne pas :

\# Match Group administrators

\# AuthorizedKeysFile
\_\_PROGRAMDATA\_\_/ssh/administrators_authorized_keys

A la suite ajouté la ligne DenyUsers Administrateur

![](media/image23.png){width="6.267716535433071in" height="4.25in"}

On voit donc bien que la connexion en ssh pour que le compte adminssh
fonctionne correctement :

![](media/image3.png){width="6.267716535433071in"
height="1.9444444444444444in"}

Test de la connexion RDP pour le compte "adminssh"

modifié le type de compte de l'utilisateur du compte adminssh car par
defaut les paramètres sont sur "utilisateur standard". il faut le mettre
en compte administrateur pour pouvoir avoir accès ensuite à la connexion
RDP :

![](media/image16.png){width="6.267716535433071in"
height="4.958333333333333in"}

![](media/image19.png){width="6.267716535433071in" height="5.125in"}
