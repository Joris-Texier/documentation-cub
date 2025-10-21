# Activités Professionnelles
## Situation professionnelle – REMÉDIATION
![logo EPOKA](../../media/logo.png){ align=center width="250" }
### Automatisation de la configuration du DHCP
**SP2 – Sécuriser l’architecture réseau et le service DHCP**  
**AP - SIO2 : Contexte EPOKA – REMEDIATION SP2**

---

## Méthode de gestion du projet informatique

Le travail en équipe rend inévitable l’utilisation d’un outil collaboratif (ex : **ELEA** dans Netocentre, cours :  
« AP-SIO2-SISR-2025 »).

Vous devez rendre dès la première séance :  
- La **liste des tâches intermédiaires** pour chaque mission (exemple : prototypage de l’architecture réseau).  
  Une tâche intermédiaire correspond à un élément *livrable* de la mission.  
- La **répartition des tâches** entre les membres de l’équipe.  
- Les **dates prévisionnelles de livraison** de chaque mission.  

Pour cela, vous utiliserez l’application en ligne **[Notion](https://www.notion.so/265ade1f09c7817dab4ef77e031bdb73?v=265ade1f09c781fcac3e000c9071ab1c)**.

**Nombre d’étudiants :** 1  
**Nombre de séances pour la situation :** 3  

---

## Présentation de la situation

Cf. **Contexte EPOKA Presse**.

La réorganisation du réseau de la société **EPOKA Presse** a laissé des traces auprès de l’administrateur réseau.  
Celui-ci avait mis en place un script nommé `calcadr.sh` lui permettant de calculer un **plan d’adressage** à partir d’une adresse réseau et d’un masque de départ, en utilisant la méthode **VLSM (Variable Length Subnet Masking)**.  

Cependant, le script a été égaré…  

Le script devait générer un plan d’adressage **affiché à l’écran** et **sauvegardé dans un fichier texte** nommé :  
`SP2planadressage.txt`

### Format attendu du fichier :
```
sr : adresse_du_sous_réseau
masque : masque_du_sous_réseau
diff : adresse_de_diffusion
```

### Exemple :
```
sr:192.168.1.0
masque:255.255.255.192
diff:192.168.1.63

sr:192.168.1.64
masque:255.255.255.224
diff:192.168.1.95
```

---

## Objectif de la remédiation

À partir du fichier `SP2planadressage.txt`, l’administrateur souhaite disposer d’un **nouveau script** permettant de générer directement un fichier de configuration DHCP prêt à être utilisé.  

Ce script sera nommé **`AutomatisationDHCP.sh`**.

---

## Missions proposées

### Mission 1 – Créer le script d’automatisation

Proposer une version du script `AutomatisationDHCP.sh` qui :
- Lit le fichier texte généré par `calcadr.sh`.
- Produit un fichier de configuration DHCP complet pour chaque sous-réseau.

---

### Mission 2 – Vérifier le fonctionnement

Tester le script sur votre prototype d’infrastructure pour valider :
- L’automatisation de la génération du fichier DHCP.
- La distribution correcte des **paramètres réseau**.

---

## Livrables attendus (à déposer sur Netocentre)

- Une **planification des tâches** (cartes Notion)  
- Le **script `AutomatisationDHCP.sh`**  
- Une **fiche de recette** pour chaque livrable  

---

## Fonctionnalités attendues

Le fichier de configuration DHCP doit contenir, pour **chaque étendue** :

- Le **sous-réseau** et le **masque** (issus du fichier texte)  
- La **plage d’adresses** (début et fin)  
- L’**adresse du serveur DNS**  
- Le **nom de domaine DNS**  
- L’**adresse de la passerelle**  
- L’**adresse de diffusion** (issue du fichier texte)

---

## Rappels importants

- L’**adresse du DNS** et le **nom de domaine DNS** doivent être demandés **une seule fois** (valables pour toutes les étendues DHCP).  
- La **plage d’adresses** et la **passerelle** doivent être demandées **pour chaque étendue DHCP**.  
- Le script doit **sauvegarder automatiquement** les fichiers de configuration DHCP initiaux avant toute modification.

---

## Contraintes matérielles et technologiques

- Ferme de serveurs avec **hyperviseur NUTANIX**  
- **Linux (Debian recommandé)** pour tous les services, y compris DHCP  
- **Commutateur Cisco de niveau 3**  
- **Commutateur Cisco 2960**  
- Traduction **NAT/PAT** via un **pare-feu Stormshield**, utilisant une adresse IP privée fournie par le professeur  
