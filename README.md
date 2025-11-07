# Kata DevBox 🥋

Bienvenue dans votre boîte à outils pour les kata et coding dojos ! 
Ce repository contient l'essentiel pour démarrer rapidement vos sessions de pratique de code.

## 📦 Concept

Ce repository est organisé de manière simple et efficace :

- **Branch `main`** : Contient les informations de base et la documentation des outils essentiels
- **Autres branches** : Dédiées à des langages ou technologies spécifiques (Python, JavaScript, Rust, etc.)

L'objectif est de vous permettre de démarrer un kata ou coding dojo en quelques commandes, sans perdre de temps en configuration.

## 🛠️ Premier Outil : DevBox

### Qu'est-ce que DevBox ?

[DevBox](https://www.jetpack.io/devbox) est un outil de gestion d'environnements de développement isolés et reproductibles. Il vous permet de :

- Créer des environnements de développement isolés par projet
- Installer des outils et dépendances sans polluer votre système
- Garantir que tous les membres d'une équipe utilisent les mêmes versions d'outils
- Partager facilement des configurations d'environnement

DevBox utilise [Nix](https://nixos.org/) sous le capot, mais vous n'avez pas besoin de connaître Nix pour l'utiliser.

### Installation de DevBox

#### Linux et macOS

```bash
curl -fsSL https://get.jetpack.io/devbox | bash
```

#### Windows (WSL2)

Installez d'abord WSL2, puis exécutez la commande Linux ci-dessus.

#### Vérification de l'installation

```bash
devbox version
```

### Concepts de Base

#### 1. Initialiser un projet

```bash
# Créer un nouveau projet DevBox
devbox init
```

Cette commande crée un fichier `devbox.json` qui contient la configuration de votre environnement.

#### 2. Ajouter des packages

```bash
# Ajouter un langage ou un outil
devbox add python3
devbox add nodejs
devbox add go

# Rechercher des packages disponibles
devbox search <nom-du-package>
```

#### 3. Entrer dans l'environnement

```bash
# Activer l'environnement DevBox
devbox shell
```

Une fois dans le shell DevBox, tous les outils installés sont disponibles. 
Votre prompt change pour indiquer que vous êtes dans un environnement DevBox.

```bash
# Vérifier que Python est disponible (si vous l'avez ajouté)
python3 --version

# Quitter l'environnement
exit
```

#### 4. Exécuter une commande dans l'environnement

```bash
# Exécuter une commande sans entrer dans le shell
devbox run python3 script.py
```

#### 5. Configurer des scripts personnalisés

Éditez votre `devbox.json` pour ajouter des scripts :

```json
{
  "packages": ["python3"],
  "shell": {
    "init_hook": [
      "echo 'Bienvenue dans votre environnement Kata !'"
    ],
    "scripts": {
      "test": "python3 -m pytest",
      "run": "python3 main.py"
    }
  }
}
```

Puis exécutez vos scripts :

```bash
devbox run test
devbox run run
```

### Commandes Essentielles

| Commande | Description |
|----------|-------------|
| `devbox init` | Initialise un nouveau projet DevBox |
| `devbox add <package>` | Ajoute un package à l'environnement |
| `devbox remove <package>` | Retire un package de l'environnement |
| `devbox shell` | Entre dans l'environnement DevBox |
| `devbox run <script>` | Exécute un script défini dans devbox.json |
| `devbox search <nom>` | Recherche des packages disponibles |
| `devbox update` | Met à jour les packages |
| `devbox info` | Affiche les informations sur l'environnement |
| `devbox generate` | Génère des fichiers de configuration (Dockerfile, etc.) |

### Avantages de DevBox pour les Katas

1. **Isolation** : Chaque kata peut avoir ses propres dépendances sans conflit
2. **Reproductibilité** : Partagez votre `devbox.json` et tout le monde a le même environnement
3. **Rapidité** : Pas besoin d'installer globalement tous les langages et outils
4. **Propreté** : Supprimez le dossier du kata, l'environnement disparaît avec
5. **Versatilité** : Testez différentes versions d'un langage sans impacter votre système

## 📚 Ressources Utiles

- [Documentation DevBox officielle](https://www.jetpack.io/devbox/docs/)
- [Catalogue de packages Nix](https://search.nixos.org/packages)
- [Exemples de Katas](https://codingdojo.org/kata/)
- [Catalog Katas](http://codekata.com/)

## 🤝 Contribution

N'hésitez pas à proposer de nouvelles configurations, des branches pour d'autres langages, ou des améliorations à la documentation !

## 📝 Licence

Ce repository est mis à disposition pour l'apprentissage et la pratique. 
Utilisez-le librement pour vos sessions de kata et coding dojos.

---

**Happy Coding! 🥋💻**