# SP0 – Mission 1 : Adapter le plan d’adressage aux nouveaux besoins
![logo EPOKA](../../media/logo.png){ align=center width="250" }
**Documentation AP – Groupe 6**  
**Lahcen Yousri / Esteban Touzet / Joris Texier**

---

## Objectif

La société **EPOKA Presse** souhaite maintenant segmenter son réseau `192.168.6.0 /24`  
en utilisant la méthode **VLSM (Variable Length Subnet Masking)**.  

Le réseau **serveurs** reste inchangé.  
Les informations sur le nombre d’hôtes de chaque service sont disponibles en *Annexe 2*.  
Une **marge de 20 %** d’adresses supplémentaires est ajoutée pour chaque sous-réseau.  

---

## Calculs des sous-réseaux

### Service : Rédaction  
**Numéro de VLAN : 61**

| Élément | Format binaire | Format décimal pointé |
|:--|:--|:--|
| Masque | `11111111.11111111.11111100.00000000` | `/26` → `255.255.255.192` |
| Adresse réseau | `11000000.10101000.00000110.00000000` | `192.168.6.0` |
| Premier hôte | `11000000.10101000.00000110.00000001` | `192.168.6.1` |
| Dernier hôte | `11000000.10101000.00000110.00111110` | `192.168.6.62` |
| Adresse de diffusion | `11000000.10101000.00000110.00111111` | `192.168.6.63` |
| Passerelle |  | `192.168.6.1` |

---

### Service : Administration  
**Numéro de VLAN : 11**

| Élément | Format décimal pointé |
|:--|:--|
| Masque | `/26` → `255.255.255.192` |
| Adresse réseau | `192.168.6.64` |
| Premier hôte | `192.168.6.65` |
| Dernier hôte | `192.168.6.126` |
| Adresse de diffusion | `192.168.6.127` |
| Passerelle | `192.168.6.65` |

---

### Service : Visiteurs  
**Numéro de VLAN : 71**

| Élément | Format décimal pointé |
|:--|:--|
| Masque | `/27` → `255.255.255.224` |
| Adresse réseau | `192.168.6.128` |
| Premier hôte | `192.168.6.129` |
| Dernier hôte | `192.168.6.158` |
| Adresse de diffusion | `192.168.6.159` |
| Passerelle | `192.168.6.129` |

---

### Service : Développement  
**Numéro de VLAN : 21**

| Élément | Format décimal pointé |
|:--|:--|
| Masque | `/27` → `255.255.255.224` |
| Adresse réseau | `192.168.6.160` |
| Premier hôte | `192.168.6.161` |
| Dernier hôte | `192.168.6.190` |
| Adresse de diffusion | `192.168.6.191` |
| Passerelle | `192.168.6.161` |

---

### Service : Direction  
**Numéro de VLAN : 31**

| Élément | Format décimal pointé |
|:--|:--|
| Masque | `/28` → `255.255.255.240` |
| Adresse réseau | `192.168.6.192` |
| Premier hôte | `192.168.6.193` |
| Dernier hôte | `192.168.6.222` |
| Adresse de diffusion | `192.168.6.223` |
| Passerelle | `192.168.6.193` |

---

### Service : Réseau  
**Numéro de VLAN : 41**

| Élément | Format décimal pointé |
|:--|:--|
| Masque | `/28` → `255.255.255.240` |
| Adresse réseau | `192.168.6.224` |
| Premier hôte | `192.168.6.225` |
| Dernier hôte | `192.168.6.238` |
| Adresse de diffusion | `192.168.6.239` |
| Passerelle | `192.168.6.225` |

---

📘 *Documentation AP – Groupe 6 : Lahcen Yousri / Esteban Touzet / Joris Texier*
