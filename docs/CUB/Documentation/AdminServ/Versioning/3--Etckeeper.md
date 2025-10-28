# Gestion de la configuration système avec Etckeeper — Agence Frankfurt

## Objectif  
Mettre en place un suivi de version automatisé du répertoire `/etc` sur l’ensemble des serveurs Debian de l’agence Frankfurt, afin de conserver une trace fiable de chaque modification de configuration système.  
L’outil Etckeeper permet d’intégrer les changements de configuration au système de gestion de versions Git, facilitant le suivi, la restauration et la collaboration entre les administrateurs.

---

## 1. Présentation d’Etckeeper

### 1.1. Pourquoi versionner `/etc`
Le répertoire `/etc` contient la configuration essentielle du système et des services : réseau, utilisateurs, sécurité, etc.  
Un simple oubli, une erreur ou une mise à jour mal gérée peuvent perturber gravement un serveur.

Grâce à Etckeeper, il est possible de :
- historiser toutes les modifications du répertoire `/etc` ;
- identifier rapidement les changements récents ;
- revenir à un état stable en cas d’erreur ;
- synchroniser les configurations entre plusieurs serveurs de l’agence Frankfurt.

### 1.2. Fonctionnement général
Etckeeper crée un dépôt Git directement dans le dossier `/etc`.  
Chaque fois qu’un changement est détecté (fichier modifié, ajouté ou supprimé), un commit Git enregistre l’état du répertoire.  
Etckeeper peut aussi être lié à APT : chaque installation ou mise à jour de paquet déclenche automatiquement un commit.

---

## 2. Installation d’Etckeeper

Sur tous les serveurs Debian de l’agence Frankfurt :

```bash
sudo apt update
sudo apt install etckeeper git -y
```

Pendant l’installation, Etckeeper initialise automatiquement un dépôt Git dans `/etc`.

---

## 3. Configuration d’Etckeeper

Les fichiers de configuration principaux d’Etckeeper sont :
- `/etc/etckeeper/etckeeper.conf` : paramètres généraux ;
- `/etc/etckeeper/commit.d/` : scripts exécutés avant chaque commit.

### 3.1. Modification du fichier de configuration
Ouvre le fichier principal :
```bash
sudo nano /etc/etckeeper/etckeeper.conf
```

Vérifie ou adapte les lignes suivantes :
```bash
VCS="git"
HIGHLEVEL_PACKAGE_MANAGER=apt
LOWLEVEL_PACKAGE_MANAGER=dpkg
```

Tu peux aussi choisir si tu veux que les commits soient automatiques :
```bash
AVOID_DAILY_AUTOCOMMITS=0
```
0 = activer les commits quotidiens automatiques.

---

## 4. Premier commit

Une fois installé et configuré, initialise le dépôt :
```bash
cd /etc
sudo etckeeper init
sudo etckeeper commit "Initialisation du suivi de /etc sur le serveur Frankfurt"
```

Pour vérifier :
```bash
sudo git log --oneline
```

---

## 5. Fonctionnement automatique avec APT

Etckeeper s’intègre avec le gestionnaire de paquets Debian.  
Lorsqu’un administrateur installe ou met à jour un paquet :
```bash
sudo apt install <nom_du_paquet>
```

Un commit automatique est créé :
Exemple : “autocommit avant apt run” puis “autocommit après apt run”.

Cela garantit que toutes les modifications liées aux mises à jour sont historisées sans action manuelle.

---

## 6. Gestion manuelle des commits

### 6.1. Ajouter un commit personnalisé
Lorsqu’une modification manuelle est effectuée dans `/etc`, il est conseillé de faire un commit clair :
```bash
cd /etc
sudo etckeeper commit "Ajout de la configuration du serveur DNS interne"
```

### 6.2. Consulter l’historique
```bash
cd /etc
sudo git log --oneline --graph --decorate
```

### 6.3. Comparer les changements récents
```bash
sudo git diff
```

### 6.4. Revenir à une version précédente
```bash
sudo git checkout <ID_commit> -- <fichier>
```
Exemple : revenir à une ancienne version du fichier `/etc/ssh/sshd_config`.

---

## 7. Sauvegarde et collaboration

### 7.1. Sauvegarder le dépôt `/etc`
Pour centraliser la configuration des serveurs Frankfurt :
```bash
cd /etc
sudo git remote add origin git@github.com:agence-frankfurt/etckeeper.git
sudo git push -u origin master
```

Chaque serveur peut ainsi pousser ses commits vers un dépôt Git central (interne à l’agence ou hébergé sur GitHub/Gitea).

### 7.2. Récupérer une configuration sur un nouveau serveur
Sur un nouveau serveur Debian :
```bash
sudo apt install etckeeper git -y
cd /etc
sudo git clone git@github.com:agence-frankfurt/etckeeper.git .
```

---

## 8. Bonnes pratiques à l’agence Frankfurt

- Effectuer un commit manuel après toute modification importante de configuration.  
- Vérifier régulièrement l’historique avec `git log` pour identifier les changements récents.  
- Synchroniser les dépôts `/etc` entre les serveurs afin d’uniformiser la configuration de l’agence.  
- Sauvegarder le dépôt `/etc` sur un serveur Git interne sécurisé.  
- Ne jamais ignorer un commit automatique : il peut contenir des modifications critiques.

---

## 9. Exemple d’utilisation complète

1. Modification d’un fichier dans `/etc/ssh/sshd_config`.  
2. Test et validation du service SSH :
   ```bash
   sudo systemctl restart ssh
   ```
3. Commit des changements :
   ```bash
   sudo etckeeper commit "Mise à jour de la configuration SSH Frankfurt"
   ```
4. Synchronisation vers le dépôt central :
   ```bash
   sudo git push
   ```

---

## Résultat attendu

- Tous les serveurs Debian de l’agence Frankfurt disposent d’un historique fiable des configurations.  
- Chaque modification dans `/etc` est suivie, datée et réversible.  
- L’équipe d’administration peut facilement identifier la source d’un problème après une mise à jour ou un changement système.
