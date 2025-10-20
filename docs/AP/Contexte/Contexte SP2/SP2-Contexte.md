# Contexte SP2 – Automatisation de la configuration du DHCP

## Méthode de gestion du projet informatique

Le travail en équipe rend inévitable l’utilisation d’un **outil collaboratif** (ex : ELEA dans Netocentre, cours : *« AP-SIO2-SISR-2025 »*).

Dès la première séance, il est demandé de fournir :

- La **liste des tâches intermédiaires** pour chaque mission (exemple : prototypage de l’architecture réseau).  
  Une tâche intermédiaire correspond à un élément *livrable* de la mission.
- La **répartition des tâches** entre les membres de l’équipe.
- Les **dates prévisionnelles** de livraison de chaque mission.

L’application en ligne **[Notion](https://www.notion.so)** est utilisée pour gérer ces éléments.

**Nombre d’étudiants :** 1  
**Nombre de séances :** 3  

---

## Présentation de la situation

Cf. contexte EPOKA Presse.

La réorganisation du réseau de la société **EPOKA Presse** a laissé des traces auprès de l’administrateur réseau.  
Celui-ci avait créé un script nommé `calcadr.sh`, permettant de **calculer un plan d’adressage** à partir d’une adresse de réseau et d’un masque initial, selon la méthode **VLSM (Variable Length Subnet Masking)**.  

Malheureusement, ce script a été **égaré**…

Le script original générait un plan d’adressage affiché à l’écran et sauvegardé dans un fichier texte nommé `SP2planadressage.txt`.

---

### Format attendu du fichier `SP2planadressage.txt`

```
sr : adresse_du_sous_réseau
masque : masque_du_sous_réseau
diff : adresse_de_diffusion
```

**Exemple d’extrait du fichier :**

```
sr:192.168.1.0
masque:255.255.255.192
diff:192.168.1.63

sr:192.168.1.64
masque:255.255.255.224
diff:192.168.1.95
```

---

## Objectif

L’administrateur souhaite désormais un **nouveau script**, nommé `AutomatisationDHCP.sh`, qui permettra de générer automatiquement un **fichier de configuration DHCP** à partir du fichier d’adressage fourni.

Ce script devra remplacer le fichier de configuration du service DHCP installé sur le serveur.

---

## Missions proposées

### Mission 1 – Création du script

Créer un script nommé **`AutomatisationDHCP.sh`** qui :

- lit le fichier `SP2planadressage.txt`,  
- extrait les informations nécessaires (sous-réseau, masque, diffusion),  
- génère automatiquement un **fichier de configuration DHCP** complet.

---

### Mission 2 – Tests sur l’infrastructure

Vérifier sur le **prototype d’infrastructure** la bonne **automatisation** et la **distribution des paramètres réseau** via le nouveau script.

---

## Livrables attendus

- Une **planification** des différentes tâches (cartes Notion ou tableau).  
- Le **script complet** `AutomatisationDHCP.sh`.  
- Une **fiche de recette** pour chaque livrable.

---

## Fonctionnalités attendues

Le fichier de configuration DHCP doit contenir, pour **chaque étendue** :

- le **sous-réseau** et le **masque** (issus du fichier texte créé par `calcadr.sh`),  
- la **plage d’adresses** (adresse de début et de fin),  
- l’**adresse du serveur DNS**,  
- le **nom de domaine DNS**,  
- l’**adresse de la passerelle**,  
- l’**adresse de diffusion** du sous-réseau.

---

### Remarques importantes

- L’**adresse du serveur DNS** et le **nom de domaine DNS** doivent être demandés **une seule fois** à l’utilisateur.  
- La **plage d’adresses** et la **passerelle** doivent être demandées **pour chaque étendue DHCP**.  
- Le script doit **sauvegarder automatiquement les fichiers initiaux de configuration DHCP** avant toute modification.

---

## Contraintes matérielles et technologiques

- Ferme de serveurs avec **hyperviseur NUTANIX**  
- Système d’exploitation : **Linux (Debian privilégiée)**  
- **Commutateur Cisco de niveau 3**  
- **Commutateur Cisco 2960**  
- **Pare-feu Stormshield** traduisant les adresses privées de l’organisation via **NAT/PAT**  
- Adresse IP privée fournie par le professeur pour la traduction

---

## 📸 Emplacements pour schémas ou illustrations

```markdown
![Architecture réseau SP2](../../media/sp2-architecture.png)

![Exemple de script AutomatisationDHCP.sh](../../media/sp2-script-example.png)

![Schéma du service DHCP automatisé](../../media/sp2-dhcp-schema.png)
```

---

## Conclusion

Cette situation SP2 vise à **automatiser la configuration du service DHCP** à partir d’un plan d’adressage calculé en VLSM.  
Elle permet de renforcer la **sécurisation et la standardisation** des configurations réseau tout en développant les compétences en **scripting Bash**, **administration réseau** et **automatisation système**.
