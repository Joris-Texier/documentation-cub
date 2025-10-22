# SP0 – Mission 3 : Mise en place d’un serveur NAS (NASEPOKA)

![logo EPOKA](../../../../media/logo.png){ align=center width="250" }

**Auteur :** Joris Texier  
**Contexte :** EPOKA  
**Version :** 1  
**Date de rédaction :** 15 septembre 2025  

---

## Sommaire
1. [Création de la VM sur Nutanix](#création-de-la-vm-sur-nutanix)  
2. [Installation de TrueNAS](#installation-de-truenas)  
3. [Configuration Réseau de TrueNAS](#configuration-réseau-de-truenas)  
4. [Création d’un Pool dans TrueNAS](#création-dun-pool-dans-truenas)  
5. [Intégration de TrueNAS dans le domaine Active Directory](#intégration-de-truenas-dans-le-domaine-active-directory)  
6. [Test et Configuration SMB](#test-et-configuration-smb)

---

## Création de la VM sur Nutanix
1. Cliquer sur **“Create VM from catalog item”**.  

![logo EPOKA](../../../../media/52.png){ align=center width="700" }

2. Choisir l’image ISO de **TrueNAS**.  
3. Configurer les paramètres comme suit :
   - Nom : `NASEPOKA`
   - RAM : `4 Go`
   - CPU : `2 vCPU`

![logo EPOKA](../../../../media/53.png){ align=center width="700" }

   - Disque : `20 Go`
   - Réseau : **Projet F**

![logo EPOKA](../../../../media/54.png){ align=center width="700" }

4. Ajouter **3 disques supplémentaires** de 20 Go chacun.  

5. Définir le fuseau horaire sur **Europe/Paris**.

![logo EPOKA](../../../../media/55.png){ align=center width="700" }

![logo EPOKA](../../../../media/56.png){ align=center width="700" }

---

## Installation de TrueNAS
1. Lancer la VM.  

![logo EPOKA](../../../../media/57.png){ align=center width="700" }

2. Appuyer sur **Entrée** ou attendre la fin du compte à rebours.
  
3. Sélectionner **Install/Upgrade**.

![logo EPOKA](../../../../media/58.png){ align=center width="700" }
  
4. Choisir le disque **NUTANIX VDISK – 20.0 GiB**.

![logo EPOKA](../../../../media/59.png){ align=center width="700" }

5. Sélectionner **Yes**.  

![logo EPOKA](../../../../media/60.png){ align=center width="700" }

6. Définir le mot de passe administrateur (**root**).

![logo EPOKA](../../../../media/61.png){ align=center width="700" }
  
7. Sélectionner les options par défaut jusqu’à la fin de l’installation.

![logo EPOKA](../../../../media/62.png){ align=center width="700" }

![logo EPOKA](../../../../media/63.png){ align=center width="700" }

8. Cliquer sur **Shutdown System**.

![logo EPOKA](../../../../media/64.png){ align=center width="700" }
  
9. Retirer l’image ISO avant de redémarrer.  

![logo EPOKA](../../../../media/65.png){ align=center width="700" }

---

## Configuration Réseau de TrueNAS
1. Attribuer une **adresse IP fixe** :  
   - IP : `172.16.56.20/24`  
   - Passerelle : `172.16.56.254`  
2. Vérifier la connectivité via un navigateur :  
   ```
   http://172.16.56.20
   ```

![logo EPOKA](../../../../media/66.png){ align=center width="700" }

![logo EPOKA](../../../../media/67.png){ align=center width="700" }

3. Depuis l’interface Web, aller dans **Network → Interfaces → Edit**.

![logo EPOKA](../../../../media/68.png){ align=center width="700" }

![logo EPOKA](../../../../media/69.png){ align=center width="700" }

4. Définir la description, l’adresse IP et valider avec **Apply**.

![logo EPOKA](../../../../media/70.png){ align=center width="700" }

5. L’adresse IP est désormais fixe.  

---

## Création d’un Pool dans TrueNAS
1. Aller dans **Storage → Pools → Add**.

![logo EPOKA](../../../../media/71.png){ align=center width="700" }

2. Cliquer sur **Create new pool** puis **Create Pool**.  
3. Sélectionner les disques à inclure dans le RAID.  
4. Choisir le type de RAID selon le nombre de disques :
   - **Stripe (RAID0)** : aucune tolérance de panne.  
   - **Mirror (RAID1)** : perte d’un disque tolérée.  
   - **RAIDZ1 (RAID5)** : perte d’un disque tolérée.

![logo EPOKA](../../../../media/72.png){ align=center width="700" }
  
   - **RAIDZ2 / RAIDZ3** : tolérance plus élevée.  
5. Nommer le pool et cliquer sur **Create**.

![logo EPOKA](../../../../media/73.png){ align=center width="700" }
  
![logo EPOKA](../../../../media/74.png){ align=center width="700" }

---

## Intégration de TrueNAS dans le domaine Active Directory

### Configuration du DNS sur TrueNAS

![logo EPOKA](../../../../media/75.png){ align=center width="700" }

1. Aller dans **Network → Global Configuration**.  
2. Saisir :
   - Serveur DNS : `172.16.56.1`  
   - Domaine : `local.epoka6.lan`

### Configurer l’Active Directory
1. Aller dans **Directory Services → Active Directory**.

![logo EPOKA](../../../../media/76.png){ align=center width="700" }
  
2. Remplir :
   - Domain Name : `local.epoka6.lan`  
   - Domain Account Name : `Administrateur`  
   - Password : `Etudiant_007`  
   - Computer Name : `TRUENAS`  
3. Cocher **Activer**.  

### Joindre le domaine
- Cliquer sur **Save**.  
- TrueNAS rejoint automatiquement le domaine AD.

### Vérification de l’intégration
1. Ouvrir le **Shell** dans TrueNAS.  
2. Vérifier la connectivité :
   ```bash
   ping 172.16.56.1
   wbinfo -u
   wbinfo -g
   ```
![logo EPOKA](../../../../media/77.png){ align=center width="700" }

![logo EPOKA](../../../../media/78.png){ align=center width="700" }

![logo EPOKA](../../../../media/79.png){ align=center width="700" }


3. Sur l’Active Directory, vérifier la présence du serveur **TRUENAS**.  

![logo EPOKA](../../../../media/80.png){ align=center width="700" }

---

## Test et Configuration SMB

### Création du Dataset
1. Aller dans **Storage → Volumes → Add Dataset**.  
2. Nommer le dataset : `test`.  
3. Définir les autorisations :
   - Type : **Groupe**
   - Nom : `LOCAL\admins du domaine`
   - Droits : Lecture, Écriture, Modification  
4. Cocher **Appliquer récursivement** puis **Enregistrer**.  
5. Vérifier avec :
   ```bash
   getfacl /mnt/Pool/test
   ```
![logo EPOKA](../../../../media/81.png){ align=center width="700" }


### Configuration du Partage SMB
1. Aller dans **Sharing → Windows (SMB) Shares → Add**.  
2. Chemin : `/mnt/Pool/test`  
3. Nom : `Test`  
4. Cochez **Activé** et **Utiliser les ACL**.  
5. Enregistrer.

![logo EPOKA](../../../../media/81.png){ align=center width="700" }


### Vérification de l’Accès depuis Windows
1. Sur un poste du domaine, ouvrir l’Explorateur et entrer :
   ```
   \\172.16.56.20
   ```

2. Se connecter avec :
   ```
   elise.arbourg@local.epoka6.lan
   Mot de passe : etudiant_007
   ```
![logo EPOKA](../../../../media/82.png){ align=center width="700" }

3. Tester la création, modification et suppression de fichiers.  

![logo EPOKA](../../../../media/83.png){ align=center width="700" }

---
