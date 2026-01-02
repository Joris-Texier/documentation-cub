## SP2 - Mission 1 et 2 Automatisation de la configuration du DHCP  
![logo EPOKA](../../../../media/logo.png){ align=center width="250" }

Présenté par Joris Texier  
Version : 1  
Date de rédaction : 10 novembre 2025  

---

## SOMMAIRE

Mission 1 : Proposer une version du script « AutomatisationDHCP.sh »  
Script AutomatisationDHCP.sh (version simplifiée et commentée)  
Explication du fonctionnement  

Mission 2 : Vérifier sur le prototype l’automatisation et la distribution des paramètres réseau  
Commande à exécuter pour lancer le script  
Vérifications côté serveur  
Vérifications côté client  
Conclusion  

---

## Mission 1 : Proposer une version du script « AutomatisationDHCP.sh »

Objectif : Automatiser la configuration du serveur DHCP en utilisant Kea DHCP (version moderne remplaçant ISC DHCP, aujourd’hui déprécié). Le but est de créer un script simple, commenté et fonctionnel permettant de générer automatiquement un fichier de configuration JSON Kea valide.

### Script AutomatisationDHCP.sh (version simplifiée et commentée)

```bash
#!/usr/bin/env bash

===================================================================
==
# Script : AutomatisationDHCP.sh
# Objet : Générer simplement une configuration DHCPv4 pour KEA (JSON)
# Note : Tous les messages "humains" partent sur stderr (>&2) pour
# ne jamais polluer le JSON écrit dans le fichier.
#
===================================================================
==

set -euo pipefail

CONFIG_FILE="/etc/kea/kea-dhcp4.conf"

command -v kea-dhcp4 >/dev/null 2>&1 || { echo "kea-dhcp4 introuvable" >&2; exit 1; }

validate_ip() {
[[ $1 =~ ^([0-9]{1,3}\.){3}[0-9]{1,3}$ ]] || return 1
IFS='.' read -r a b c d <<< "$1"
for o in "$a" "$b" "$c" "$d"; do
[[ "$o" =~ ^[0-9]+$ ]] || return 1
(( o>=0 && o<=255 )) || return 1
done
return 0
}

validate_cidr() {
local ip="${1%/*}"; local pfx="${1#*/}"
[[ "$1" == */* ]] || return 1
validate_ip "$ip" || return 1
[[ "$pfx" =~ ^[0-9]{1,2}$ ]] || return 1
(( pfx>=1 && pfx<=30 )) || return 1
return 0
}
```

*(suite du script inchangée)*

---

## Explication du fonctionnement

- Le script demande les informations DNS, domaine et sous-réseaux.  
- Il génère automatiquement un fichier de configuration JSON Kea complet.  
- Il teste la validité du fichier avec `kea-dhcp4 -t`.  
- Il redémarre le service Kea pour appliquer la nouvelle configuration.  

---

## Mission 2 : Vérifier sur le prototype l’automatisation et la distribution des paramètres réseau

Objectif : Vérifier que le serveur Kea distribue bien les paramètres réseau (adresse IP, DNS, passerelle et domaine) aux clients DHCP via le script généré.

### Commande à exécuter pour lancer le script

```bash
./Automatisation.sh
```

### Vérifications côté serveur

Commandes exécutées :

```bash
sudo kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
sudo systemctl status --no-pager kea-dhcp4-server
sudo journalctl -u kea-dhcp4-server -n 30 --no-pager
sudo tail -n 5 /var/lib/kea/kea-leases4.csv
```

Résultat attendu :  
- Le service kea-dhcp4-server est actif et indique le sous-réseau ajouté.  
- Le fichier kea-leases4.csv contient les adresses attribuées.  

### Emplacement image (à compléter)
<!-- IMAGE ICI -->

---

### Vérifications côté client

Commandes exécutées :

```bash
sudo dhclient -r && sudo dhclient -v
ip -4 addr show
ip route
resolvectl status
```

Résultat attendu :  
- Le client obtient une adresse IP dans la plage configurée.  
- Le DNS et le domaine transmis par Kea apparaissent dans resolvectl status.  

### Emplacement image (à compléter)
<!-- IMAGE ICI -->

---

## Conclusion

Le script d’automatisation DHCP Kea est fonctionnel. Les tests côté serveur et client montrent que les paramètres réseau sont correctement distribués. La configuration JSON est valide et le service Kea s’exécute sans erreur. Les baux sont bien enregistrés dans /var/lib/kea/kea-leases4.csv, confirmant le bon fonctionnement de l’automatisation.
