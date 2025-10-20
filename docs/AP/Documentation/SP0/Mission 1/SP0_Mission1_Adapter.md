# SP0 — Mission 1 : Adapter le plan d’adressage aux nouveaux besoins

## Objectif
Concevoir un plan d’adressage IP cohérent, évolutif et documenté pour l’ensemble des réseaux de l’entreprise.

## Contexte
- Sites / VLAN / sous-réseaux à prévoir
- Contraintes (sécurité, interco, croissance prévue, etc.)

## Plan d’adressage (proposition)
| VLAN | Usage | Réseau | Masque | Gateway |
|---|---|---|---|---|
| 10 | Utilisateurs | 192.168.10.0 | /24 | 192.168.10.254 |
| 20 | Admin | 192.168.20.0 | /24 | 192.168.20.254 |
| … | … | … | … | … |

## Schémas
![Schéma logique](../media/mission1/schema-logique.png)
![Schéma physique](../media/mission1/schema-physique.png)
![Schéma de câblage](../media/mission1/schema-cablage.png)

## Table de routage
*(à compléter)*

## Table de NAT
*(à compléter)*

## Justification des choix
*(à compléter)*
