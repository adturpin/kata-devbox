# Kata DevBox - TypeScript / Node.js

Environnement pour katas en TypeScript avec Node.js et Vitest.

---

## 📦 Initialiser DevBox (première fois uniquement)

```bash
# Initialiser devbox
devbox init

# Ajouter Node.js
devbox add nodejs_20

# Entrer dans l'environnement
devbox shell

# Vérifier l'installation
node --version
npm --version
```

---

## 🚀 Setup du Projet

```bash
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
npx tsc --init --target ES2022 --module ESNext --moduleResolution bundler --strict --esModuleInterop --skipLibCheck --forceConsistentCasingInFileNames --outDir ./dist --rootDir ./src

# 6. Créer la structure du projet
mkdir -p src tests

# 7. Configurer vitest
cat > vitest.config.ts << 'EOF'
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
EOF

# 8. Configurer les scripts dans package.json
npm pkg set scripts.test="vitest"
npm pkg set scripts.test:watch="vitest --watch"
npm pkg set scripts.test:ui="vitest --ui"
npm pkg set scripts.test:coverage="vitest --coverage"
npm pkg set scripts.build="tsc"

# 9. Lancer les tests
npm test

# 10. Mode watch (auto-reload)
npm run test:watch
```

**Structure créée :**
```
$PROJECT_NAME/
├── src/              → Code de production
├── tests/            → Tests
├── tsconfig.json     → Config TypeScript
├── vitest.config.ts  → Config Vitest
└── package.json      → Dépendances et scripts
```

---

## ✏️ Exemple

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
});
```

**src/calculator.ts**
```typescript
export class Calculator {
  add(a: number, b: number): number {
    return a + b;
  }
}
```

---

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

### 2️⃣ Expect - Assertions lisibles

```typescript
// Valeurs
result.toBe(5);
result.toEqual(5);
result.toBeGreaterThan(0);
result.toBeLessThanOrEqual(10);
value.toBeTruthy();
value.toBeFalsy();
value.toBeNull();
value.toBeUndefined();
value.toBeDefined();

// Chaînes
name.toBe("Alice");
name.toContain("lic");
name.toMatch(/^Al/);

// Tableaux et objets
array.toHaveLength(3);
array.toContain(item);
array.toEqual([1, 2, 3]);
obj.toEqual({ name: "Alice", age: 30 });
obj.toMatchObject({ name: "Alice" });

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
fn.toHaveBeenCalled();
fn.toHaveBeenCalledTimes(2);
fn.toHaveBeenCalledWith(arg1, arg2);
```

### 3️⃣ Vi - Mocks et stubs

```typescript
import { vi, describe, it, expect, beforeEach } from 'vitest';

// Créer un mock
const mockFn = vi.fn();

// Setup : configurer le comportement
mockFn.mockReturnValue(42);
mockFn.mockResolvedValue('async result');

// Mock d'un module
vi.mock('../src/userRepository', () => ({
  UserRepository: vi.fn().mockImplementation(() => ({
    getById: vi.fn().mockResolvedValue({ id: 1, name: 'Alice' })
  }))
}));

// Utiliser
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

**Matchers utiles :**
```typescript
vi.fn()                                    // Créer un mock
expect.any(Number)                         // N'importe quelle valeur
expect.anything()                          // N'importe quelle valeur non null/undefined
expect.arrayContaining([1, 2])             // Tableau contenant
expect.objectContaining({ name: 'Alice' }) // Objet contenant
```

### 4️⃣ Cucumber - Tests BDD (optionnel)

**Installation :**
```bash
npm install --save-dev @cucumber/cucumber @cucumber/gherkin
```

**features/calculator.feature**
```gherkin
# language: fr
Fonctionnalité: Calculatrice

Scénario: Addition de deux nombres
  Étant donné les nombres 2 et 3
  Quand je les additionne
  Alors le résultat est 5
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

### 5️⃣ Approvals - Tests de snapshot (optionnel)

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
});
```

**Alternative : Vitest snapshots**
```typescript
it('should match snapshot', () => {
  const data = { id: 1, name: 'Alice' };
  expect(data).toMatchSnapshot();
});
```

### 6️⃣ Couverture de code

```bash
# Couverture simple
npm run test:coverage

# Voir le rapport HTML
open coverage/index.html  # macOS
xdg-open coverage/index.html  # Linux
```

**Configuration dans vitest.config.ts :**
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

**Bon kata ! 🥋**
