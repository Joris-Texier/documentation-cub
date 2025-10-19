# Documentation CUB

Ce dépôt contient toute la documentation liée à l'agence CUB :
- Plan d'adressage
- Routage
- NAT
- Configuration des switchs et pare-feu
- DNS, etc.

Le site est automatiquement généré grâce à **MkDocs Material** et publié via **GitHub Pages**.

## 🔧 Commandes utiles

### Lancer le serveur local
```bash
mkdocs serve
```

### Générer le site statique
```bash
mkdocs build
```

### Déploiement automatique
Chaque push sur la branche `main` déclenche une action GitHub qui publie automatiquement le site sur Pages.
