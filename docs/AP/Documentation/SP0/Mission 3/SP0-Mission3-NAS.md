# SP0 — Mission 3 : Mise en place d’un serveur NAS

## Objectif
Déployer un serveur **NAS** pour le stockage centralisé (partages utilisateurs / services), avec
contrôle des accès, sauvegardes et supervision.

## Contexte
- Plateforme cible (ex. **TrueNAS CORE**, **OpenMediaVault**, **Debian + Samba/NFS**).
- Rôle : partage de données des équipes, dépôt d’images/ISO, sauvegardes.
- Intégration à l'infrastructure (DNS/AD, VLAN, routage).

## Topologie
![Topologie NAS](../media/mission3/nas-topologie.png)

## Prérequis
- Machine/VM avec stockage suffisant (RAID recommandé).
- IP fixe, enregistrement DNS.
- Compte d’admin, accès Web/SSH.

## 1) Installation du NAS
> Adapter selon la solution choisie (TrueNAS/OMV/Debian).

### Exemple TrueNAS / OMV (via interface Web)
1. Accéder à l’interface d’administration.
2. Configurer la **carte réseau** (IP, gateway, DNS).
3. Préparer les **disques/pools** (RAID / ZFS / ext4…).
4. Créer les datasets/points de montage.

![NAS — Configuration réseau](../media/mission3/nas-config-reseau.png)
![NAS — Pool/Disques](../media/mission3/nas-pool.png)

### Exemple Debian + Samba (commandes)
```bash
sudo apt update && sudo apt install -y samba
sudo mkdir -p /srv/partage/equipeA /srv/partage/equipeB
sudo chown -R root:root /srv/partage
sudo chmod -R 770 /srv/partage
# Éditez /etc/samba/smb.conf puis :
sudo systemctl enable --now smbd nmbd
```

## 2) Partages et protocoles
- **SMB/CIFS** pour les postes Windows, **NFS** pour serveurs Linux.
- Définir les **partages** (ex. `\nas\equipeA`, `\nas\equipeB`), droits (RW/RO).

![NAS — Partages CIFS/NFS](../media/mission3/nas-partages.png)

## 3) Gestion des accès
- Intégration **AD** (si présent) ou comptes locaux.
- Groupes par équipe / service, mappage des lecteurs au logon.

![NAS — ACL et groupes](../media/mission3/nas-acl.png)

## 4) Sauvegardes & Snapshots
- Planifier des **snapshots** (ZFS/btrfs) et **sauvegardes** vers un dépôt externe.
- Vérifier la restauration de fichiers (tests).

![NAS — Snapshots](../media/mission3/nas-snapshots.png)
![NAS — Sauvegardes](../media/mission3/nas-backup.png)

## 5) Supervision
- Remonter les métriques (disque, I/O, CPU, température) dans votre outil (ex. Zabbix/GLPI Agent).

![NAS — Supervision](../media/mission3/nas-supervision.png)

## 6) Tests
- Montage du partage depuis un poste Windows (`\\nas\equipeA`) et Linux (`mount -t nfs ...`).
- Tests de **lecture/écriture**, quotas, verrouillage de fichiers, restauration.

## 7) Documentation livrée
- Screenshots de l’ensemble des écrans clés.
- Schéma de l’architecture, plan d’adressage, table des partages, stratégie de sauvegarde.
- Procédures d’exploitation (création d’utilisateur, restauration d’un fichier, extension d’un pool).
