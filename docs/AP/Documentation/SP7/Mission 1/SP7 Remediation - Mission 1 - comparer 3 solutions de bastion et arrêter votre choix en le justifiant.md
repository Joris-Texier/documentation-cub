# SP7 Remediation - Mission 1 - comparer 3 solutions de bastion et arrêter votre choix en le justifiant

![logo EPOKA](../../../../media/logo.png){ align=center width="250" }

Présenté par Joris Texier  
Version : 1  
Date de rédaction : 17 novembre 2025  

---

## SOMMAIRE

- Introduction  
- Étude des solutions  
  - 2.1 Wallix Bastion  
  - 2.2 Teleport  
  - 2.3 Apache Guacamole  
- Tableau comparatif  
- Choix final  
- Conclusion  

---

## Introduction

La société EPOKA souhaite renforcer la sécurité de son infrastructure informatique en intégrant un bastion d’administration, permettant de contrôler, filtrer et tracer les connexions des administrateurs vers les serveurs Windows, Linux et les équipements réseau.

L’objectif de cette mission est de comparer trois solutions de bastion puis de retenir celle qui sera intégrée dans l’architecture EPOKA.

Les trois solutions étudiées sont :
- Wallix Bastion  
- Teleport  
- Apache Guacamole  

---

## Étude des solutions

### 2.1 Wallix Bastion

#### Fonctionnalités
- PAM (Privileged Access Management) complet  
- Enregistrement vidéo des sessions  
- Intégration Active Directory  
- Gestion des accès RDP / SSH / Telnet  
- Contrôle avancé des comptes à privilèges  
- Audit détaillé de chaque session  
- Solution certifiée ANSSI  
- Déploiement on-premise  

#### Avantages
- Très haut niveau de sécurité  
- Meilleure solution professionnelle  
- Excellent support en France  
- Parfait pour tracer l’ensemble des opérations administrateurs  

#### Inconvénients
- Coût élevé  
- Déploiement et administration plus complexes  
- Surdimensionné pour des environnements simples  

#### Adaptation à EPOKA
Solution excellente mais coûteuse, plus complexe à intégrer dans une maquette pédagogique ou dans un environnement nécessitant une mise en œuvre rapide.

---

### 2.2 Teleport

#### Fonctionnalités
- Accès SSH, RDP, Kubernetes, bases de données  
- Authentification Zero-Trust  
- Journalisation des sessions  
- Compatible AD / LDAP  
- Version open-source disponible  

#### Avantages
- Moderne, flexible  
- Très bon support Linux  
- Possibilité de réduire les coûts grâce à la version open-source  

#### Inconvénients
- Interface orientée DevOps, plus technique  
- Gestion RDP moins intuitive  
- Enregistrements vidéo limités sans la version Enterprise  

#### Adaptation à EPOKA
Teleport fonctionne bien pour des environnements cloud / Linux, mais correspond moins à un datacenter classique avec serveurs Windows et équipements Cisco.

---

### 2.3 Apache Guacamole

#### Fonctionnalités
- Portail d’accès distant via navigateur web  
- Support RDP, SSH, VNC  
- Aucun client à installer (100% web)  
- Journalisation des connexions  
- Intégration LDAP / AD possible  
- Open-source et gratuit  

#### Avantages
- Déploiement simple et rapide  
- Très léger, parfait pour une maquette  
- Gratuit, aucun coût de licence  
- Accès facilement contrôlable via une interface unique  
- Compatible Windows, Linux, équipements réseau via SSH  
- Idéal pour centraliser les accès administrateurs  

#### Inconvénients
- Pas d’enregistrements vidéo natifs  
- Moins complet qu’un vrai PAM (comme Wallix)  
- Sécurité moins avancée que Wallix ou Teleport  
- Certaines fonctions avancées nécessitent des extensions  

#### Adaptation à EPOKA
Guacamole est largement suffisant pour centraliser et sécuriser les accès administrateurs tout en répondant au cahier des charges pédagogique.

Il est simple à mettre en place sur une maquette NUTANIX, compatible avec les accès Windows, Linux et Cisco.

---

## Tableau comparatif

Critères | Wallix | Teleport | Guacamole  
--- | --- | --- | ---  
Coût | Élevé | Payant ou gratuit | Gratuit  
RDP | ✔️ | ✔️ | ✔️  
SSH | ✔️ | ✔️ | ✔️  
Intégration AD | ✔️ | ✔️ | ✔️  
Enregistrement vidéo | ✔️ | Moyen | ❌  
Simplicité de déploiement | Moyen | Moyen | Très simple  
Adapté à un environnement pédagogique | ❌ | Moyen | ✔️  
Support équipement Cisco | ✔️ | ✔️ | ✔️  

---

## Choix final

### Solution retenue : Apache Guacamole

#### Justification du choix

J’ai choisi Guacamole pour les raisons suivantes :
- Gratuit et open-source, ce qui est idéal dans un contexte scolaire ou de POC.  
- Très simple à installer, idéal pour la mission et pour la maquette EPOKA.  
- Centralisation des accès RDP et SSH dans une seule interface web.  
- Compatible avec l’Active Directory, permettant d’utiliser un groupe "Bastion" comme demandé dans le cahier des charges.  

Fonctionne parfaitement pour administrer :
- les serveurs Windows (RDP)  
- les serveurs Linux (SSH)  
- les équipements réseau Cisco (SSH)  

Même si Wallix est plus complet, il est trop lourd et coûteux pour cette situation.

Guacamole répond à toutes les exigences de la Mission SP7 tout en restant simple, rapide et efficace.

---

## Conclusion

À l’issue de cette étude comparative, la solution Guacamole est celle qui offre le meilleur compromis entre efficacité, simplicité, compatibilité et coût pour répondre aux besoins d’EPOKA.

Elle sera intégrée dans la maquette lors de la Mission 2 et configurée selon le cahier des charges pour la Mission 3.
