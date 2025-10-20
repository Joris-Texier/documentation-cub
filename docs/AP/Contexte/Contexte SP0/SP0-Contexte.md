# Contexte SP0 – Infrastructure et services réseaux EPOKA

## Introduction

### Méthode de gestion du projet informatique

Le travail en équipe rend inévitable l’utilisation d’un outil collaboratif  
(exemple : ELEA dans Netocentre, cours *« AP-SIO2-SISR-2025 »*).

Dès la première séance, il est demandé de fournir :
- La **liste des tâches intermédiaires** pour chaque mission (ex : prototypage de l’architecture réseau).  
  Une tâche intermédiaire correspond à un élément « livrable » de la mission.
- La **répartition des tâches** entre les membres de l’équipe.
- Les **dates prévisionnelles** de livraison de chaque mission.

L’application en ligne [Notion](https://www.notion.so) est utilisée pour gérer ces éléments.

**Nombre d’étudiants :** 2  
**Date de livraison de l’ensemble de la situation :** 29 septembre 2025  

---

## Présentation de la situation

La société **EPOKA Presse** dispose d’un réseau accueillant l’ensemble de ses services.  
Chaque poste possède une **adresse IP fixe** attribuée manuellement par l’administrateur réseau suivant un plan d’adressage interne.  

Cependant, plusieurs problèmes sont apparus :
- Le trafic du réseau subit de **nombreux ralentissements**.
- Une **ferme de serveurs** permettant la virtualisation via **AHV (Nutanix)** a été installée par la société Bull.
- Les **serveurs virtuels** hébergés doivent être adressés dans la plage `172.16.5X.0/24`  
  (avec `X = 1` pour le VLAN 51, `X = 2` pour le VLAN 52, etc.).

---

## Missions proposées

### Mission 1 : Adapter le plan d’adressage aux nouveaux besoins

La société EPOKA Presse souhaite **segmenter son réseau 192.168.X.0/24** en utilisant la méthode **VLSM** (*Variable Length Subnet Masking*).  
Le réseau des serveurs reste inchangé.

Les informations nécessaires pour le calcul du plan d’adressage sont disponibles dans le document *Annexe 2*.  
Une **marge de 20 %** d’adresses supplémentaires doit être prévue pour chaque sous-réseau.  

**Livrable attendu :**  
Un nouveau plan d’adressage validé par un enseignant.

---

### Mission 2 : Mise en place d’un contrôleur de domaine et d’un service DHCP

Une **ferme de serveurs Nutanix** hébergeant des machines virtuelles est utilisée pour installer deux serveurs :

#### Serveur ADEPOKA
- IP : `172.16.5X.1`
- OS : Windows Server 2019  
- Rôles : **DNS dynamique** et **Active Directory**  
- Domaine : `local.epokaX.lan`  
- Peuplement de l’AD à l’aide d’un **script PowerShell** et d’un **fichier CSV** fourni.

#### Serveur DHCPEPOKA
- IP : `172.16.5X.2`
- OS : Debian  
- Rôle : **DHCP** (tous les sous-réseaux sauf les serveurs doivent être dynamiques)  
- Le service DHCP doit distribuer les paramètres réseau à l’ensemble des postes.

---

### Mission 3 : Mise en place d’un serveur de gestion d’incidents et d’un serveur NAS

#### Serveur GLPIEPOKA
- IP : `172.16.5X.40`
- OS : *(à déterminer et justifier)*  
- Services : **gestion d’inventaire et de tickets d’incidents**  
- Responsables :
  - Olivier Tondet (accès et interconnexion)
  - Pamela Tremo (services et systèmes serveurs)

#### Serveur NAS (NASSCA)
- IP : `172.16.5X.20`
- OS : *(à déterminer)*  
- Service : **Sauvegarde et stockage**  
- Configuration en **RAID 5**, avec au moins 20 Go disponibles.  
- Accès via **authentification Active Directory**.

---

### Mission 4 : Mise en place d’une partie de l’architecture réseau

Configurer :
- un **commutateur d’accès** (segmentation et VLAN),
- un **routeur inter-VLAN**,
- un **pare-feu Stormshield SN210** pour la sécurité réseau.

Une maquette doit être réalisée sous **Packet Tracer**, testée, puis validée.  
Les tests incluent la vérification du **routage** et de la **distribution DHCP**.

---

## Contraintes matérielles et technologiques

- Ferme de serveurs **NUTANIX AHV**
- **Windows Server 2019** pour l’Active Directory et le DNS
- **Debian** pour le DHCP et autres services
- Routeur **Cisco 1921**
- Pare-feu **Stormshield SN210**
- Commutateur **Cisco 2960**
- Borne Wi-Fi **D-Link**
- Ordinateur portable
- Adressage privé traduit en **NAT/PAT** via le routeur cœur de réseau

---

## Documentation technique

### Document 1 : Définition du NAT et du PAT

Les protocoles **NAT** et **PAT** permettent la traduction d’adresses pour limiter l’usage d’adresses IP publiques.  
Le **PAT (Port Address Translation)** utilise également les ports pour différencier plusieurs connexions partageant la même IP.

> Source : [waytolearnx.com - Différence entre NAT et PAT](https://waytolearnx.com/2018/07/difference-entre-nat-et-pat.html)

📸 *Emplacement pour schéma explicatif :*  
```markdown
![Schéma NAT/PAT](../../media/nat-pat.png)
```

---

### Document 2 : Nombre d’hôtes dans le parc informatique

| VLAN | Service | Nombre d’hôtes (hors passerelles) |
|------|----------|----------------------------------|
| 11 | Administration (RH / Compta / Juridique / Secrétariat) | 45 |
| 21 | Développement | 20 |
| 31 | Direction | 12 |
| 41 | Réseau | 8 |
| 61 | Rédaction | 50 |
| 71 | Visiteurs | 25 |
| 5X | Serveurs (X=1 pour VLAN 51, X=2 pour VLAN 52…) | — |

📸 *Emplacement pour le plan d’adressage :*  
```markdown
![Plan d’adressage EPOKA](../../media/plan-adressage-epoka.png)
```

---

### Document 3 : Extrait d’architecture réseau attendue

📸 *Insérer ici le schéma de l’architecture réseau :*  
```markdown
![Architecture réseau EPOKA](../../media/architecture-reseau-epoka.png)
```

---

## Conclusion

Cette situation professionnelle SP0 vise à :
- Concevoir et implémenter une **infrastructure réseau fonctionnelle et sécurisée**,
- Mettre en œuvre des **services essentiels** (DNS, AD, DHCP, NAS),
- Appliquer des **bonnes pratiques de virtualisation et d’adressage réseau**.
