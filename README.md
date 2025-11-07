# Kata DevBox - Python Edition 🐍

Bienvenue dans votre environnement Python pour les kata et coding dojos !
Cette configuration vous permet de démarrer rapidement vos sessions de pratique avec un environnement Python isolé et reproductible.

## 📦 Configuration Python

Ce repository utilise DevBox pour créer un environnement Python isolé avec tous les outils essentiels pré-configurés.

### Environnement Inclus

- **Python 3.11+** : Dernière version stable
- **pytest** : Framework de tests
- **pytest-cov** : Couverture de code
- **mypy** : Vérification de types statique
- **black** : Formateur de code
- **ruff** : Linter ultra-rapide
- **ipython** : REPL interactif amélioré

## 🚀 Démarrage Rapide

### 1. Installation de DevBox (si nécessaire)

```bash
curl -fsSL https://get.jetpack.io/devbox | bash
```

### 2. Installer Python avec DevBox

```bash
# Ajouter Python à votre environnement
devbox add python3
```

### 3. Activer l'environnement

```bash
# Entrer dans l'environnement Python
devbox shell

# Vérifier que Python est disponible
python --version
pytest --version
```

### 4. Structure de Projet Recommandée

```
kata-nom/
├── devbox.json          # Configuration DevBox
├── pyproject.toml       # Configuration Python moderne
├── src/
│   └── kata.py         # Votre code
└── tests/
    └── test_kata.py    # Vos tests
```

## 🧪 Commandes Essentielles

Une fois dans l'environnement DevBox (`devbox shell`), utilisez ces commandes :

```bash
# Lancer les tests
pytest

# Lancer les tests avec couverture
pytest --cov=src --cov-report=term-missing

# Formater le code
black src tests

# Vérifier le style
ruff check src tests

# Lancer le REPL Python interactif
python
```

## 💡 Bonnes Pratiques Python pour Katas

1. **Type Hints** : Utilisez les annotations de types
```python
def fibonacci(n: int) -> int:
    ...
```

2. **Docstrings** : Documentez vos fonctions
```python
def kata_function(param: str) -> str:
    """
    Description de la fonction.

    Args:
        param: Description du paramètre

    Returns:
        Description du retour
    """
```

3. **Tests Paramétrés** : Utilisez `pytest.mark.parametrize`
```python
@pytest.mark.parametrize("input,expected", [
    (1, "1"),
    (2, "2"),
    (3, "Fizz"),
])
def test_multiple_cases(input, expected):
    assert fizzbuzz(input) == expected
```

4. **Garde le Code Simple** : KISS (Keep It Simple, Stupid)

5. **Refactorez Constamment** : Le code doit être propre à chaque étape

## 📚 Ressources Python

- [Python Documentation](https://docs.python.org/3/)
- [pytest Documentation](https://docs.pytest.org/)
- [Real Python](https://realpython.com/)
- [Python Koans](https://github.com/gregmalcolm/python_koans)
- [Exercism Python Track](https://exercism.org/tracks/python)

## 🔧 Dépannage

### Installation de packages supplémentaires

```bash
# Dans le shell DevBox
pip install <package-name>

# Ou ajoutez-le au init_hook du devbox.json
```

### Réinitialiser l'environnement

```bash
# Sortir du shell
exit

# Réentrer
devbox shell
```

**Happy Coding! 🐍🥋**
