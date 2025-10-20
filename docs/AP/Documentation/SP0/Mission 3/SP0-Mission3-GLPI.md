# SP0 — Mission 3 : Mise en place d’un serveur de gestion d’incidents (GLPI)

## Objectif
Installer et configurer GLPI (ou équivalent) pour la gestion des tickets/incidents.

## Architecture
![Schéma GLPI](../media/mission3/schema-glpi.png)

## Installation (exemple Debian/Ubuntu)
```bash
sudo apt update && sudo apt install -y apache2 mariadb-server php php-cli php-mysql php-xml php-curl php-mbstring
# Déployer GLPI (indiquer version/source), configurer VirtualHost, droits, etc.
```

## Base de données
- Création d’un utilisateur et d’une base dédiés
- Sécurisation (mots de passe, sauvegardes)

## Paramétrages GLPI
- Catégories, SLA, notifications
- Modèle de ticket

## Tests
- Ouverture et cycle de vie d’un ticket de bout en bout
