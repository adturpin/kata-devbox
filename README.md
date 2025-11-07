<<<<<<< Updated upstream
# Kata DevBox 🥋

Bienvenue dans votre boîte à outils pour les kata et coding dojos ! 
Ce repository contient l'essentiel pour démarrer rapidement vos sessions de pratique de code.
=======
# Kata DevBox - TypeScript / Node.js

Environnement pour katas en TypeScript avec Node.js et Vitest.
>>>>>>> Stashed changes

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

<<<<<<< Updated upstream
Cette commande crée un fichier `devbox.json` qui contient la configuration de votre environnement.
=======
# Ajouter Node.js
devbox add nodejs_20
>>>>>>> Stashed changes

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
<<<<<<< Updated upstream
=======

# Vérifier l'installation
node --version
npm --version
>>>>>>> Stashed changes
```

Une fois dans le shell DevBox, tous les outils installés sont disponibles. 
Votre prompt change pour indiquer que vous êtes dans un environnement DevBox.

```bash
<<<<<<< Updated upstream
# Vérifier que Python est disponible (si vous l'avez ajouté)
python3 --version

# Quitter l'environnement
exit
```

#### 4. Exécuter une commande dans l'environnement

```bash
# Exécuter une commande sans entrer dans le shell
devbox run python3 script.py
=======
# 0. Définir le nom du projet
PROJECT_NAME="kata"  # Changer ici pour votre kata (ex: "fizzbuzz", "string-calculator")

# 1. Lancer DevBox (si pas déjà dans le shell)
devbox shell

# 2. Créer le projet
mkdir $PROJECT_NAME
cd $PROJECT_NAME
npm init -y

# 3. Installer TypeScript et les outils de dev
npm install --save-dev typescript @types/node
npm install --save-dev tsx  # Pour exécuter TypeScript directement

# 4. Installer Vitest et les outils de test
npm install --save-dev vitest @vitest/ui
npm install --save-dev @vitest/coverage-v8

# 5. Configurer TypeScript
npx tsc --init

# 6. Créer la structure du projet
mkdir -p src tests

# 7. Configurer les scripts dans package.json
npm pkg set scripts.test="vitest"
npm pkg set scripts.test:watch="vitest --watch"
npm pkg set scripts.test:ui="vitest --ui"
npm pkg set scripts.test:coverage="vitest --coverage"
npm pkg set scripts.build="tsc"

# 8. Lancer les tests
npm test

# 9. Mode watch (auto-reload)
npm run test:watch
```

**Structure créée :**
```
$PROJECT_NAME/
├── src/              → Code de production
├── tests/            → Tests
├── tsconfig.json     → Config TypeScript
└── package.json      → Dépendances et scripts
```

---

## ⚙️ Configuration tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "types": ["node", "vitest/globals"]
  },
  "include": ["src/**/*", "tests/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

---

## ⚙️ Configuration vitest.config.ts

```typescript
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    globals: true,
    environment: 'node',
    coverage: {
      provider: 'v8',
      reporter: ['text', 'html', 'lcov'],
      exclude: ['node_modules/', 'tests/']
    }
  }
});
>>>>>>> Stashed changes
```

#### 5. Configurer des scripts personnalisés

Éditez votre `devbox.json` pour ajouter des scripts :

<<<<<<< Updated upstream
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
=======
**tests/calculator.test.ts**
```typescript
import { describe, it, expect } from 'vitest';
import { Calculator } from '../src/calculator';

describe('Calculator', () => {
  it('should add two numbers and return sum', () => {
    const calculator = new Calculator();
    const result = calculator.add(2, 3);
    expect(result).toBe(5);
  });

  it('should multiply two numbers', () => {
    const calculator = new Calculator();
    const result = calculator.multiply(4, 5);
    expect(result).toBe(20);
  });
});
```

**src/calculator.ts**
```typescript
export class Calculator {
  add(a: number, b: number): number {
    return a + b;
  }

  multiply(a: number, b: number): number {
    return a * b;
  }
}
>>>>>>> Stashed changes
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

<<<<<<< Updated upstream
**Happy Coding! 🥋💻**
=======
## 🧪 Guide des Outils

### 1️⃣ Vitest - Framework de test

```typescript
import { describe, it, expect, beforeEach, afterEach } from 'vitest';

describe('Test Suite', () => {
  beforeEach(() => {
    // Avant chaque test
  });

  afterEach(() => {
    // Après chaque test
  });

  it('simple test', () => {
    expect(true).toBe(true);
  });

  it.each([
    [2, 3, 5],
    [0, 0, 0],
    [-1, 1, 0]
  ])('add(%i, %i) should return %i', (a, b, expected) => {
    expect(a + b).toBe(expected);
  });
});
```

### 2️⃣ Expect - Assertions

```typescript
// Valeurs
expect(result).toBe(5);
expect(result).toEqual(5);
expect(result).toBeGreaterThan(0);
expect(result).toBeLessThanOrEqual(10);
expect(value).toBeTruthy();
expect(value).toBeFalsy();
expect(value).toBeNull();
expect(value).toBeUndefined();
expect(value).toBeDefined();

// Chaînes
expect(name).toBe("Alice");
expect(name).toContain("lic");
expect(name).toMatch(/^Al/);

// Tableaux et objets
expect(array).toHaveLength(3);
expect(array).toContain(item);
expect(array).toEqual([1, 2, 3]);
expect(obj).toEqual({ name: "Alice", age: 30 });
expect(obj).toMatchObject({ name: "Alice" });

// Exceptions
expect(() => {
  throw new Error('Oops');
}).toThrow();
expect(() => {
  throw new Error('Oops');
}).toThrow('Oops');
expect(() => {
  throw new Error('Oops');
}).toThrow(Error);

// Fonctions
expect(fn).toHaveBeenCalled();
expect(fn).toHaveBeenCalledTimes(2);
expect(fn).toHaveBeenCalledWith(arg1, arg2);
```

### 3️⃣ Vi - Mocks et Spies

```typescript
import { vi, describe, it, expect, beforeEach } from 'vitest';

// Mock d'une fonction
const mockFn = vi.fn();
mockFn.mockReturnValue(42);
mockFn.mockResolvedValue('async result');

// Spy sur une méthode
const obj = {
  method: (x: number) => x * 2
};
const spy = vi.spyOn(obj, 'method');
obj.method(5);
expect(spy).toHaveBeenCalledWith(5);

// Mock d'un module
vi.mock('../src/userRepository', () => ({
  UserRepository: vi.fn().mockImplementation(() => ({
    getById: vi.fn().mockResolvedValue({ id: 1, name: 'Alice' })
  }))
}));

// Exemple complet avec interface
interface IUserRepository {
  getById(id: number): Promise<User>;
  save(user: User): Promise<void>;
}

describe('UserService', () => {
  let mockRepo: IUserRepository;

  beforeEach(() => {
    mockRepo = {
      getById: vi.fn().mockResolvedValue({ id: 1, name: 'Alice' }),
      save: vi.fn().mockResolvedValue(undefined)
    };
  });

  it('should get user by id', async () => {
    const service = new UserService(mockRepo);
    const user = await service.getUser(1);

    expect(user.name).toBe('Alice');
    expect(mockRepo.getById).toHaveBeenCalledWith(1);
    expect(mockRepo.getById).toHaveBeenCalledTimes(1);
  });
});
```

**Matchers utiles avec vi:**
```typescript
vi.fn()                                    // Créer un mock
expect.any(Number)                         // N'importe quel nombre
expect.anything()                          // N'importe quelle valeur
expect.arrayContaining([1, 2])             // Tableau contenant
expect.objectContaining({ name: 'Alice' }) // Objet contenant
```

### 5️⃣ Cucumber - Tests BDD (optionnel)

**Installation :**
```bash
npm install --save-dev @cucumber/cucumber
npm install --save-dev @cucumber/gherkin
```

**features/calculator.feature**
```gherkin
# language: fr
Fonctionnalité: Calculatrice

Scénario: Addition de deux nombres
  Étant donné les nombres 2 et 3
  Quand je les additionne
  Alors le résultat est 5

Scénario: Addition de plusieurs nombres
  Étant donné les nombres suivants:
    | nombre |
    | 1      |
    | 2      |
    | 3      |
  Quand je les additionne tous
  Alors le résultat est 6
```

**features/step_definitions/calculator.steps.ts**
```typescript
import { Given, When, Then, setDefaultTimeout } from '@cucumber/cucumber';
import { expect } from 'chai';
import { Calculator } from '../../src/calculator';

setDefaultTimeout(5000);

let calculator: Calculator;
let a: number;
let b: number;
let result: number;

Given('les nombres {int} et {int}', (num1: number, num2: number) => {
  calculator = new Calculator();
  a = num1;
  b = num2;
});

When('je les additionne', () => {
  result = calculator.add(a, b);
});

Then('le résultat est {int}', (expected: number) => {
  expect(result).to.equal(expected);
});
```

**cucumber.js (configuration)**
```javascript
module.exports = {
  default: {
    require: ['features/step_definitions/**/*.ts'],
    requireModule: ['ts-node/register'],
    format: ['progress', 'html:reports/cucumber-report.html'],
    publishQuiet: true
  }
};
```

**Scripts package.json**
```bash
npm pkg set scripts.test:bdd="cucumber-js"
npm pkg set scripts.test:bdd:watch="cucumber-js --watch"
```

### 6️⃣ Approvals - Tests de snapshot avancés (optionnel)

**Installation :**
```bash
npm install --save-dev approvals
```

**Utilisation :**
```typescript
import { verify } from 'approvals';
import { describe, it } from 'vitest';

describe('Report Generation', () => {
  it('should generate correct report', () => {
    const report = generateReport({
      title: 'Monthly Report',
      data: [1, 2, 3, 4, 5]
    });

    // Au premier run, crée un fichier .received.txt
    // Renommer en .approved.txt si OK
    // Les runs suivants comparent avec .approved.txt
    verify(__dirname, 'report', report);
  });

  it('should generate JSON output', () => {
    const data = {
      users: [
        { id: 1, name: 'Alice' },
        { id: 2, name: 'Bob' }
      ]
    };

    verify(__dirname, 'users', JSON.stringify(data, null, 2));
  });
});
```

**Alternative : jest-json-snapshot**
```bash
npm install --save-dev jest-json-snapshot
```

```typescript
import { it, expect } from 'vitest';
import { toMatchSnapshot } from 'jest-json-snapshot';

expect.extend({ toMatchSnapshot });

it('should match complex object', () => {
  const complexObject = {
    id: 123,
    timestamp: Date.now(),
    data: { /* ... */ }
  };

  expect(complexObject).toMatchSnapshot({
    timestamp: expect.any(Number) // Ignorer les champs dynamiques
  });
});
```

### 7️⃣ Couverture de code

```bash
# Générer le rapport de couverture
npm run test:coverage

# Voir le rapport HTML
open coverage/index.html
```

**Configuration minimale dans vitest.config.ts :**
```typescript
export default defineConfig({
  test: {
    coverage: {
      provider: 'v8',
      reporter: ['text', 'html', 'json-summary'],
      lines: 80,
      branches: 80,
      functions: 80,
      statements: 80
    }
  }
});
```

---

## 🛠️ Commandes Essentielles

```bash
# Tests
npm test                              # Lancer les tests
npm run test:watch                    # Mode watch
npm run test:ui                       # Interface web interactive
npm run test:coverage                 # Avec couverture de code
npm test -- --run                     # Sans watch (CI)
npm test -- calculator                # Filtrer par nom de fichier

# Build
npm run build                         # Compiler TypeScript
npm run build -- --watch              # Mode watch pour build

# DevBox
devbox shell                          # Entrer dans l'env
exit                                  # Sortir
devbox info                           # Voir les packages
```

---

## 📚 Alternative : Jest (si préféré)

Si vous préférez Jest à Vitest :

```bash
# Installation
npm install --save-dev jest ts-jest @types/jest
npm install --save-dev @jest/globals

# Configuration
npx ts-jest config:init

# Scripts package.json
npm pkg set scripts.test="jest"
npm pkg set scripts.test:watch="jest --watch"
npm pkg set scripts.test:coverage="jest --coverage"
```

**jest.config.js**
```javascript
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  roots: ['<rootDir>/tests'],
  testMatch: ['**/*.test.ts'],
  collectCoverageFrom: [
    'src/**/*.ts',
    '!src/**/*.d.ts'
  ]
};
```

**Bon kata ! 🥋**
>>>>>>> Stashed changes
