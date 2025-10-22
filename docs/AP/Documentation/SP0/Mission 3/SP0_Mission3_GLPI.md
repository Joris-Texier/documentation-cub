# SP0 – Mission 3 : Mise en place d’un serveur de gestion d’incidents (GLPIEPOKA)

![logo EPOKA](../../../../media/logo.png){ align=center width="250" }

**Présenté par :** Joris Texier  
**Contexte :** EPOKA  
**Version :** 1  
**Date de rédaction :** 08 septembre 2025  

---

## Sommaire
1. [Outil de gestion d’incidents](#outil-de-gestion-dincidents)  
2. [Installation de Debian 12](#installation-de-debian-12)  
3. [Préparer le serveur pour installer GLPI](#préparer-le-serveur-pour-installer-glpi)  
4. [Installer le socle LAMP](#installer-le-socle-lamp)  
5. [Préparer une base de données pour GLPI](#préparer-une-base-de-données-pour-glpi)  
6. [Télécharger GLPI et préparer son installation](#télécharger-glpi-et-préparer-son-installation)  
7. [Préparer la configuration Apache2](#préparer-la-configuration-apache2)  
8. [Utilisation de PHP8.2-FPM avec Apache2](#utilisation-de-php82-fpm-avec-apache2)  
9. [Installation de GLPI](#installation-de-glpi)  
10. [Création des 2 agents sur GLPI](#création-des-2-agents-sur-glpi)  
11. [Création d’une VM Windows pour les agents](#création-dune-vm-windows-pour-les-agents)  
12. [Automatisation de l’inventaire](#automatisation-de-linventaire)  

---

## Outil de gestion d’incidents

Outils possibles : **GLPI**, OTRS, Request Tracker, Zendesk, Freshservice.  
En analysant nos besoins (fonctionnalités, facilité d'installation, documentation, communauté), nous avons choisi **GLPI**.

---

## Installation de Debian 12

1. Cloner sur le serveur Nutanix une machine virtuelle Debian 12 déjà existante.

![logo EPOKA](../../../../media/85.png){ align=center width="700" }  

2. Démarrer la machine virtuelle.  
3. Connexion :  
   ```
   login : etudiant
   mot de passe : etudiant_007
   ```
![logo EPOKA](../../../../media/86.png){ align=center width="700" }

4. Vérifier la configuration réseau :  
   ```bash
   ip addr
   ```
![logo EPOKA](../../../../media/87.png){ align=center width="700" }

---

## Préparer le serveur pour installer GLPI

Mettre à jour les paquets et configurer l’adresse IP :  
```bash
sudo apt-get update && sudo apt-get upgrade
```

---

## Installer le socle LAMP

Installer Apache2, MariaDB et PHP :
```bash
sudo apt-get install apache2 php mariadb-server
```

Installer les extensions PHP nécessaires :
```bash
sudo apt-get install php-xml php-common php-json php-mysql php-mbstring php-curl php-gd php-intl php-zip php-bz2 php-imap php-apcu
```

Si vous souhaitez intégrer un LDAP :
```bash
sudo apt-get install php-ldap
```

---

## Préparer une base de données pour GLPI

Sécuriser MariaDB :
```bash
sudo mysql_secure_installation
```
![logo EPOKA](../../../../media/88.png){ align=center width="700" }

Créer la base de données :
```bash
sudo mysql -u root -p
```

Puis dans le prompt MariaDB :
```sql
CREATE DATABASE db_GLPIEPOKA;
CREATE USER 'glpi_admin'@'localhost' IDENTIFIED BY 'etudiant_007';
GRANT ALL PRIVILEGES ON db_GLPIEPOKA.* TO 'glpi_admin'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
![logo EPOKA](../../../../media/89.png){ align=center width="700" }

---

## Télécharger GLPI et préparer son installation

Télécharger l’archive GLPI :
```bash
cd /tmp
wget https://github.com/glpi-project/glpi/releases/download/10.0.10/glpi-10.0.10.tgz
```

Décompresser et déplacer :
```bash
sudo tar -xzvf glpi-10.0.10.tgz -C /var/www/
sudo chown www-data /var/www/glpi/ -R
```

Créer les répertoires sécurisés :
```bash
sudo mkdir /etc/glpi /var/lib/glpi /var/log/glpi
sudo chown www-data /etc/glpi /var/lib/glpi /var/log/glpi
```

Déplacer les fichiers :
```bash
sudo mv /var/www/glpi/config /etc/glpi
sudo mv /var/www/glpi/files /var/lib/glpi
```

Créer le fichier `/var/www/glpi/inc/downstream.php` :
```php
<?php
define('GLPI_CONFIG_DIR', '/etc/glpi/');
if (file_exists(GLPI_CONFIG_DIR . '/local_define.php')) {
    require_once GLPI_CONFIG_DIR . '/local_define.php';
}
```

Créer le fichier `/etc/glpi/local_define.php` :
```php
<?php
define('GLPI_VAR_DIR', '/var/lib/glpi/files');
define('GLPI_LOG_DIR', '/var/log/glpi');
```

---

## Préparer la configuration Apache2

Créer un VirtualHost :
```bash
sudo nano /etc/apache2/sites-available/support.it-connect.tech.conf
```
![logo EPOKA](../../../../media/90.png){ align=center width="700" }

Activer la configuration :
```bash
sudo a2ensite support.it-connect.tech.conf
sudo a2dissite 000-default.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
```

---

## Utilisation de PHP8.2-FPM avec Apache2

Installer PHP-FPM :
```bash
sudo apt-get install php8.2-fpm
```

Activer les modules :
```bash
sudo a2enmod proxy_fcgi setenvif
sudo a2enconf php8.2-fpm
sudo systemctl reload apache2
```

Modifier `/etc/php/8.2/fpm/php.ini` :
```ini
session.cookie_httponly = on
```

Redémarrer PHP-FPM :
```bash
sudo systemctl restart php8.2-fpm.service
```

Modifier le VirtualHost :
```apache
<FilesMatch \.php$>
    SetHandler "proxy:unix:/run/php/php8.2-fpm.sock|fcgi://localhost/"
</FilesMatch>
```
![logo EPOKA](../../../../media/91.png){ align=center width="700" }

---

## Installation de GLPI

1. Accéder à GLPI via le navigateur.

![logo EPOKA](../../../../media/92.png){ align=center width="700" }  

2. Sélectionner la langue et cliquer sur **Installer**.

![logo EPOKA](../../../../media/93.png){ align=center width="700" }  

3. Vérifier la configuration serveur.

![logo EPOKA](../../../../media/94.png){ align=center width="700" }  

4. Entrer les identifiants SQL :
   ```
   Serveur : localhost
   Utilisateur : glpi_admin
   Mot de passe : etudiant_007
   ```
![logo EPOKA](../../../../media/95.png){ align=center width="700" }

5. Sélectionner la base `db_GLPIEPOKA`.

![logo EPOKA](../../../../media/96.png){ align=center width="700" } 
 
6. Terminer l’installation. 
 
![logo EPOKA](../../../../media/99.png){ align=center width="700" }

Connexion initiale :
```
Utilisateur : glpi
Mot de passe : glpi
```

![logo EPOKA](../../../../media/100.png){ align=center width="700" }

![logo EPOKA](../../../../media/101.png){ align=center width="700" }

---

## Création des 2 agents sur GLPI

- **Olivier Tondet** – Responsable des solutions techniques d’accès

![logo EPOKA](../../../../media/102.png){ align=center width="700" }
  
- **Pamela Tremo** – Responsable des systèmes serveurs
  
![logo EPOKA](../../../../media/103.png){ align=center width="700" }
---

## Création d’une VM Windows pour les agents

Créer deux machines virtuelles Windows pour les agents.

![logo EPOKA](../../../../media/104.png){ align=center width="700" }

---

## Automatisation de l’inventaire

1. Télécharger l’agent GLPI 1.10.  
2. Pendant l’installation, définir le serveur GLPI :
   ```
   http://172.16.56.10/glpi
   ```
![logo EPOKA](../../../../media/105.png){ align=center width="700" }

3. Activer l’inventaire :
   ```
   Administration → Inventaire
   ```
![logo EPOKA](../../../../media/106.png){ align=center width="700" }

4. Forcer un inventaire : 
   ```
   http://127.0.0.1:62354
   ```
![logo EPOKA](../../../../media/107.png){ align=center width="700" }


   

