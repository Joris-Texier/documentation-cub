# SP0 — Mission 2 : Mise en place AD / DHCP

## Objectif
Déployer un contrôleur de domaine (AD DS) et un service DHCP opérationnel et sécurisé.

## Pré-requis
- VM/Serveur Windows Server
- IP fixe et résolution DNS fonctionnelle

## Étapes
### 1) Mise à jour
```bash
# Debian/Ubuntu (exemple)
sudo apt update && sudo apt upgrade -y
```

### 2) Installation rôles (ex. Windows Server)
*(décrire les rôles, captures, commandes PowerShell si applicable)*

### 3) DHCP : portée(s) et options
| Portée | Réseau | Masque | Gateway | DNS | Baux |
|---|---|---|---|---|---|
| Principale | 192.168.10.0 | /24 | 192.168.10.254 | 192.168.20.10 | 8h |

### Captures / schémas
![Topo DHCP](../media/mission2/topo-dhcp.png)

## Tests
- Joindre un poste au domaine
- Attribution d’IP par DHCP
